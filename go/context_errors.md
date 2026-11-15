# Контекстные ошибки в Go

Модель ошибок в Go простая - функции возвращают error, и вызывающий решает, что
с этим делать. Проблема в том, что голое «вернуть наверх как есть» редко
подходит для живой системы.

Когда несколько слоев кода просто пробрасывают err без описания контекста, в
логах остается только формулировка из нижнего уровня. В распределенной системе
это означает, что придется гадать в каком именно месте цепочки произошел сбой.

Решение — добавлять уровень контекста в каждом слое с помощью
`fmt.Errorf("doing X for id=%s: %w", id, err)`. Так сохраняется исходная ошибка
для `errors.Is` и `errors.As` и одновременно появляется понятное человеку
сообщение.

Для повторяющихся состояний полезно вводить именованные значения ошибок, чтобы
логика могла различать их по типу, а логи оставались говорящими. В итоге, при
чтении стека ошибок складывается понятный рассказ о том, что происходило.

```go
var ErrTokenInvalid = errors.New("token invalid")

// Три разных места, где может что то пойти не так

func parseToken(raw string) (Token error) {
	const op = "parseToken"

	t, err := reallyParse(raw)
	if err != nil {
		return Token{}, fmt.Errorf("%s: parse raw token failed: %w", op, ErrTokenInvalid)
	}

	return t, nil
}

func validateClaims(t Token) error {
	const op = "validateClaims"

	if time.Now().After(t.ExpiresAt) {
		return fmt.Errorf("%s: token expired at %s: %w", op, t.ExpiresAt, ErrTokenInvalid)
	}

	if !t.HasScope("admin") {
		return fmt.Errorf("%s: missing admin scope: %w", op, ErrTokenInvalid)
	}

	return nil
}

func lookupSession(ctx context.Context, store SessionStore, t Token) (*Session, error) {
	const op = "lookupSession"

	s, err := store.GetByID(ctx, t.SessionID)
	if err != nil {
		if errors.Is(err, sql.ErrNoRows) {
			return nil, fmt.Errorf("%s: session %q not found: %w", op, t.SessionID, ErrTokenInvalid)
		}

		return nil, fmt.Errorf("%s: load session %q failed: %w", op, t.SessionID, err)
	}

	return s, nil
}
```

## Оборачивать каждую ошибку контекстом не нужно

Оборачивание уместно на стыках, где теряется контекст. Внутри пакета оно лишь
раздувает сообщения и аллокации.

Популярный совет в Go звучит разумно.

> Оборачивайте каждую ошибку, пока она всплывает по стеку вызовов, чтобы всегда
> знать, откуда она пришла:

```go
func getUser(id string) (*User, error) {
    u, err := db.QueryUser(id)
    if err != nil {
        return nil, fmt.Errorf("getUser: query failed: %w", err)
    }
    return u, nil
}
```

В маленьком проекте это работает. Но в системе, где один запрос проходит через
шесть сервисов, можно получить строки ошибок вот такого вида:

```txt
processPayment: validateAccount: fetchBalance: getUser: query failed: context deadline exceeded
```

Это не наблюдаемость, а стектрейс, переодетый в сообщение об ошибке. Здесь
дублируется то, что структурированное логирование уже даёт (имя функции, файл,
строку), и есть плата за аллокацию `fmt.Errorf` на каждом несчастливом пути.

### Что делать вместо

Оборачивать ошибки на границах доверия. Это вызов другого сервиса, слой базы
данных или внешняя зависимость. Внутри одного пакета пусть ошибка летит как
есть, а работу делает логгер:

```go
// На границе оборачивается
func (s *PaymentService) Process(ctx context.Context, req *ProcessRequest) error {
    if err := s.ledger.Debit(ctx, req.AccountID, req.Amount); err != nil {
        return fmt.Errorf("debit account %s: %w", req.AccountID, err)
    }
    return nil
}
```

```go
// Внутри пакета просто возвращается
func validate(req *ProcessRequest) error {
    if req.Amount <= 0 {
        return ErrInvalidAmount
    }
    return nil
}
```
