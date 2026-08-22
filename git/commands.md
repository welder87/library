# Actions

## Просмотр истории коммитов

```bash
git log -- filepath/
```

```bash
git log --oneline --graph --decorate
```

```bash
git log -p
```

```bash
git log --pretty=format: --name-only branch_name..HEAD | sort | grep -v '^$' | uniq -c | sort -nr
```

```bash
git log --oneline --name-only
```

```bash
git add -i
```

```bash
git add -p
```

## Откат изменений

```bash
git restore .
```

```bash
git restore --staged --worktree .

# Восстановить только проиндексированные файлы
git restore --staged .
```

```bash
# Создать локальную ветку и переключиться на нее
git checkout -b feature-branch origin/feature-branch

# Или с помощью switch (более современный способ)
git switch -c feature-branch origin/feature-branch
```

```bash
git fetch --prune
```

```bash
git stash
```

## Создание ветки с отслеживанием удаленной

```bash
git push -u origin <branch_name>
# или
git push --set-upstream origin <branch_name>
```

```bash
git switch --track origin/<branch_name>
```

```bash
git switch -c <branch_name1> --track origin/<branch_name2>
```

```bash
git branch -vv
```

## Временно перейти в состояние старого коммита, чтобы посмотреть, как всё работало тогда

```bash
git switch --detach <commit_hash>
```

```bash
git switch <branch_name>
```

## Отмена последнего коммита

```bash
git reset --soft HEAD~1
```

```bash
git reset HEAD~1
# или
git reset --mixed HEAD~1
```

```bash
git reset --hard HEAD~1
```

```bash
git revert HEAD
```

## Поиск разницы между ветками

```bash
git log branch1 ^branch2
git log branch1 --not branch2
```

```bash
git log branch2 ^branch1
git log branch2 --not branch1
```

```bash
git log branch1...branch2
```

```bash
git rev-list --count feature ^main
git rev-list --count develop --not master
```

```bash
git rev-list feature ^main
git rev-list develop --not master
```

## Применение коммитов одной ветки к другой

```bash
git cherry-pick <commit_hash>
```

```bash
git cherry-pick <commit_hash_1> <commit_hash_2> <commit_hash_3>
```

```bash
git cherry-pick <commit_hash_start>^..<commit_hash_end>
```

## Изменить наменование удаленной ветки git

```bash
git push origin --delete old-branch-name
```

```bash
git branch -m old-branch-name new-branch-name
```

```bash
git push origin new-branch-name
```

```bash
git branch --set-upstream-to=origin/new-branch-name
```

## Восстановить удаленную ветку

Ветка удалена и в локальном, и в удалённом репозитории.

```bash
git reflog --no-abbrev
```

```bash
git checkout [sha]
```

```bash
git checkout -b [branchname]
```

## Перебазирование (Rebase)

| Команда  | Сокр. | Что делает                              |
| -------- | ----- | --------------------------------------- |
| `pick`   | `p`   | Использовать коммит как есть            |
| `reword` | `r`   | Изменить сообщение коммита              |
| `edit`   | `e`   | Остановиться для правки                 |
| `squash` | `s`   | Объединить с предыдущим (с сообщением)  |
| `fixup`  | `f`   | Объединить с предыдущим (без сообщения) |
| `drop`   | `d`   | Удалить коммит                          |
| `exec`   | `x`   | Выполнить команду                       |
| `break`  | `b`   | Остановиться для отладки                |
| `merge`  | `m`   | Создать merge-коммит                    |

Показывает коммиты, которые есть в текущей ветке, но отсутствуют в указанной
ветке (или upstream).

```bash
git cherry -v <branch_name>
```

Показывает количество коммитов, которые есть в текущей ветке, но отсутствуют в
указанной ветке (или upstream).

```bash
git cherry -v <branch_name> | wc -l
```

Перебазирование с N коммитами в интерактивном режиме.

```bash
git rebase -i HEAD~N
```

Перезапись истории в удаленной ветке.

```bash
git push origin branch_name --force
# или более безопасно:
git push origin branch_name --force-with-lease
```

- `--force` и `--force-with-lease` перезаписывают историю на удалённом
  репозитории. Нужно убедиться, что это не нарушит работу других участников
  проекта. `--force-with-lease` cамо по себе, без уточнения деталей, защитит все
  удалённые ссылки, которые будут обновляться, требуя, чтобы их текущее значение
  совпадало с удалённой веткой отслеживания, которую мы для них создали.
- Если ветка защищена (например, в GitHub/GitLab), может потребоваться снять
  защиту перед отправкой.

Полная отмена перебазирования.

```bash
git reset ORIG_HEAD --hard
```

Режим, в котором Git автоматически расставляет команды squash/fixup,
ориентируясь на специальные ключевые слова в сообщениях коммитов.

```bash
git rebase -i --autosquash
```

Чтобы autosquash сработал, вы должны создать «целевые» коммиты со специальным
сообщением:

| Шаблон сообщения    | Куда попадёт коммит | Команда в rebase                |
| ------------------- | ------------------- | ------------------------------- |
| fixup! <сообщение>  | перед целевым       | fixup (сообщение отбрасывается) |
| squash! <сообщение> | перед целевым       | squash (сообщения объединяются) |
| amend! <сообщение>  | перед целевым       | squash + можно отредактировать  |
| revert! <сообщение> | перед целевым       | удаляет целевой коммит          |

## Откат удалённой ветки до определённого коммита

```bash
git log --oneline
```

```bash
git branch backup-branch
```

```bash
git checkout имя-ветки
```

```bash
git reset --hard хеш-коммита
```

```bash
git push origin имя-ветки --force
# или
git push origin имя-ветки --force-with-lease
```

- `--force` и `--force-with-lease` перезаписывают историю на удалённом
  репозитории. Нужно убедиться, что это не нарушит работу других участников
  проекта. `--force-with-lease` cамо по себе, без уточнения деталей, защитит все
  удалённые ссылки, которые будут обновляться, требуя, чтобы их текущее значение
  совпадало с удалённой веткой отслеживания, которую мы для них создали.
- Если ветка защищена (например, в GitHub/GitLab), может потребоваться снять
  защиту перед отправкой.

### Альтернативный способ (без reset --hard)

Если нужно сохранить последующие коммиты как "отменённые", можно создать
обратный коммит (revert):

```bash
git revert хеш-нежелательных-коммитов
git push origin имя-ветки
```

Это безопаснее, но не удаляет коммиты из истории.

## Поиск коммитов в Git

```bash
# Базовый поиск
git log --grep="текст_поиска"
# С учетом регистра
git log -i --grep="текст"  # --regexp-ignore-case
# Регулярные выражения
git log --grep="^fix:"  # коммиты, начинающиеся с "fix:"
# Несколько условий (ИЛИ)
git log --grep="bug" --grep="fix"
```

```bash
# Поиск строки в истории (-S)
git log -S "функция_имя"
# Поиск по регулярному выражению (-G)
git log -G "регулярное_выражение"
# С путем к файлу
git log -S "текст" -- путь/к/файлу
git log -p -S "TODO" -- src/
```

```bash
git log --author="ИмяАвтора"
git log --author="email@example.com"
# Частичное совпадение
git log --author="John"
```

```bash
# Относительное время
git log --since="2 weeks ago"
git log --since="1 month ago"
git log --since="yesterday"
# Абсолютные даты
git log --since="2024-01-01"
git log --until="2024-12-31"
# Диапазон
git log --since="2024-01-01" --until="2024-06-30"
# По дню
git log --since="9am" --until="6pm"
```

```bash
# Частичный хеш (первые 4+ символов)
git show abc123
git log --oneline abc123..
# По тегу
git show v1.0.0
```

```bash
git log feature --grep="bug"
git log main..feature --grep="fix"
```

## Вернуть слияние

### Способ 1

Полностью откатывает слияние к состоянию до merge. Все изменения в рабочей
директории, сделанные после конфликта, будут потеряны.

```bash
git merge --abort
git merge <branch>
```

### Способ 2

Cбросить неверные правки в конфликтуемых файлах, перезапустить резолв конкретных
файлов.

```bash
git checkout --conflict=merge <файлы>   # восстановить маркеры конфликта
git mergetool                           # заново разрешить
```

git bisect Git сам поможет найти, в каком коммите всё сломалось. git bisect
start git bisect bad git bisect good <commit_hash> 6. git reflog Отображает
историю перемещений HEAD, что позволяет восстановить потерянные коммиты: git
reflog Незаменимая команда при восстановлении случайно удалённых изменений. 7.
git reset —hard <commit_hash> Сбрасывает текущую ветку к указанному коммиту,
удаляя все последующие изменения: git reset —hard a1b2c3d Используй с
осторожностью, так как это удаляет все несохранённые изменения.

## Stash

### Сохранение

`git stash (или git stash push)` — спрятать изменения в отслеживаемых файлах.
Неотслеживаемые (untracked) файлы не прячутся.

`git stash -u (--include-untracked)` — спрятать вместе с неотслеживаемыми
файлами.

`git stash -a (--all)` — то же + игнорируемые файлы (из .gitignore).

`git stash -m "текст"` — с сообщением, чтобы потом легко найти в списке.

`git stash push` -- file.xx — спрятать только конкретные файлы/папки.

`git stash -p` — интерактивно выбрать, какие куски изменений прятать.

`git stash --keep-index` — спрятать всё, но оставить в рабочей папке то, что уже
в staging area.

### Просмотр

`git stash list` — список стэшей (stash@{0}, stash@{1}, …).

`git stash show` — краткая статистика последнего стэша.

`git stash show -p stash@{1}` — полный diff конкретного стэша.

### Восстановление

`git stash pop` — применить последний стэш и удалить его из списка.

`git stash apply` — применить, но оставить в списке (удобно, если хотите
применить в несколько веток).

`git stash apply stash@{2}` — применить конкретный стэш.

`git stash apply --index` — восстановить ещё и разделение на staged/unstaged,
какое было до стэша.

### Удаление и прочее

`git stash drop stash@{1}` — удалить конкретный стэш.

`git stash clear` — удалить все стэши (без подтверждения, осторожно).

`git stash branch fix-branch` — создать новую ветку от коммита, где был сделан
стэш, и применить его туда — спасает, когда после переключения веток стэш
конфликтует.
