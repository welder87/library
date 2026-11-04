# Валидация данных в Go

## Интерфейс Validator

Определяем интерфейс `Validator`. Любой тип, который его реализует, умеет сам
себя проверять:

```go
type Validator interface {
    Validate() map[string]string
}
```

Вместо сырой `map[string]string` удобнее завести отдельный тип `Problems`:

```go
type Problems map[string]string

func (p Problems) Add(field, msg string) {
    p[field] = msg
}

func (p Problems) String() string {
    if len(p) == 0 {
        return ""
    }
    msgs := make([]string, 0, len(p))
    for field, msg := range p {
        msgs = append(msgs, fmt.Sprintf("%s: %s", field, msg))
    }
    return strings.Join(msgs, "; ")
}
```

## Пример с реальным типом

Допустим, есть запрос на регистрацию периодической задачи:

```go
type TaskCreateRequest struct {
    Name     string
    Run      func(ctx context.Context) error
    Interval time.Duration
    Deadline time.Duration
}
```

Правила простые:

- имя обязательно
- функция не может быть nil
- интервал и дедлайн больше нуля
- дедлайн меньше интервала.

### Реализация Validate прямо на этом типе:

```go
func (r *TaskCreateRequest) Validate() validation.Problems {
    problems := make(validation.Problems)

    if r.Name == "" {
        problems.Add("Name", "is required")
    }
    if r.Run == nil {
        problems.Add("Run", "is required")
    }
    if r.Interval <= 0 {
        problems.Add("Interval", "must be greater than 0")
    }
    if r.Deadline <= 0 {
        problems.Add("Deadline", "must be greater than 0")
    }
    if r.Deadline >= r.Interval {
        problems.Add("Deadline", "must be less than Interval")
    }

    return problems
}
```

### Функция `Check` вместо прямого вызова `Validate`

`validation.Problems` не реализует интерфейс `error`, поэтому вызывающий код не
должен знать об этом типе. Для этого есть функция Check:

```go
func Check(v Validator) error {
    if v == nil {
        return errors.New("value is nil, cannot validate")
    }
    problems := v.Validate()
    if len(problems) == 0 {
        return nil
    }
    return fmt.Errorf("validation failed for %s: %s",
        typeName(v),
        problems.String(),
    )
}
```

### Использование

```go
func RegisterTask(req *TaskCreateRequest) error {
    if err := validation.Check(req); err != nil {
        return err
    }
    // ...
}
```

Если передать неполный запрос, получим понятное сообщение:

```txt
validation failed for TaskCreateRequest: Deadline: must be greater than 0; Name: is required
```

## Где размещать валидацию

В проектах с архитектурой `controller/service/repository` валидация живёт на
уровне сервиса. Сервис проверяет всё, что к нему приходит — будь то HTTP-запрос,
gRPC или stdin.

Удобно держать Validate рядом с определением типа. Когда что-то идёт не так —
плохие данные прошли до базы или хорошие данные были отклонены — сразу понятно,
куда смотреть.

## Выводы

Подход работает за счёт трёх вещей:

- единый интерфейс Validator
- тип Problems для читаемых ошибок
- функция Check, которая возвращает стандартный error.

Валидация остаётся рядом с типом, код не размазывается по проекту, и добавить
новую проверку дело одной строки.
