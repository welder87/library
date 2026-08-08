# Helix Cases

# Автоформатирование

Таблица на момент версии helix 25.07.1

https://github.com/helix-editor/helix/pull/15458

| config.toml | languages.toml | format |
| ----------- | -------------- | ------ |
| true        | true           | true   |
| true        | false          | false  |
| false       | true           | false  |
| false       | false          | false  |

## Markdown в Helix

1. Проверить, что грамматика есть

```bash
hx --health markdown
```

Покажет статус языка: дерево грамматики, LSP, форматтер. Если грамматики нет:

```bash
hx -g fetch   # скачать tree-sitter грамматики
hx -g build   # скомпилировать
```

2. Language Server

```bash
# Marksman
wget https://github.com/artempyanykh/marksman/releases/latest/download/marksman-linux-x64 -O ~/.local/bin/marksman
chmod +x ~/.local/bin/marksman
```

~/.config/helix/languages.toml

```toml
[language-server.marksman]
command = "marksman"

[[language]]
name = "markdown"
language-servers = ["marksman"]
soft-wrap = { enable = true, max-wrap = 25, wrap-indicator = "" }
```

Другой вариант — markdown-oxide

3. Форматирование (dprint)

https://dprint.dev/

~/.config/helix/languages.toml

```toml
[[language]]
name = "markdown"
formatter = { command = "dprint", args = ["fmt", "--stdin", "md", "--config", "~/.config/dprint.json"] }
```

4. Preview (предпросмотр рендеринга)

Helix сам Markdown не рендерит — нужен внешний viewer. Варианты:

glow — рендерит Markdown в терминале.

```bash
sudo snap install glow
```

```text
:sh glow %
```

## Case 1. Мультикурсор. Одно слово встречается несколько раз - заменить все

| Клавиша     | Действие                                                  |
| ----------- | --------------------------------------------------------- |
| C           | добавить следующий курсор на строку ниже                  |
| A-C (Alt+C) | добавить курсоры на конец каждой строки в выделении       |
| S-C         | выделить все вхождения текущего выделения в файле         |
| A-;         | убрать последний курсор ( secondary selection → primary ) |
| ,           | оставить только основной курсор (сбросить остальные)      |

1. Поставьте курсор на нужном слове.
2. Нажмите `miw` (select inner word) — выделится слово.
3. Нажмите `S-C` — выделятся все вхождения этого слова в файле, курсор появится
   на каждом.
4. Нажмите `c` (change) — перейдёте в insert сразу для всех выделений.
5. Введите новое слово, Esc.

Все вхождения заменятся одновременно.

## Case 2. Скопировать строку в в системный буфер обмена

x # выделить строку Space+y # в системный буфер обмена classMu

## Case 3.

### Основные команды (normal mode)

| Клавиша | Команда                   | Что делает                 |
| ------- | ------------------------- | -------------------------- |
| w       | move_next_word_start      | В начало следующего слова  |
| b       | move_prev_word_start      | В начало предыдущего слова |
| e       | move_next_word_end        | В конец следующего слова   |
| W       | move_next_long_word_start | В начало следующего WORD   |
| B       | move_prev_long_word_start | В начало предыдущего WORD  |
| E       | move_next_long_word_end   | В конец следующего WORD    |

Разница между w и W w / b / e — малые слова (sub-word).

Точки, запятые и смена регистра — границы слов. camelCase считается двумя
словами (camel + Case).

W / B / E — большие слова (WORD).

Граница — только пробелы. camelCase — одно слово целиком.

```text
foo,bar baz.qux hello-world
```
