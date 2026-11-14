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
