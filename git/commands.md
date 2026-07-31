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

```bash
git cherry -v <branch_name>
```

```bash
git cherry -v <branch_name> | wc -l
```

```bash
git rebase -i HEAD~N # перебазирование с N коммитами
```

```bash
git push origin branch_name --force
# или более безопасно:
git push origin branch_name --force-with-lease
```

```bash
git reset ORIG_HEAD --hard
```

- `--force` и `--force-with-lease` перезаписывают историю на удалённом репозитории. Нужно убедиться, что это не нарушит работу других участников проекта. `--force-with-lease` cамо по себе, без уточнения деталей, защитит все удалённые ссылки, которые будут обновляться, требуя, чтобы их текущее значение совпадало с удалённой веткой отслеживания, которую мы для них создали.
- Если ветка защищена (например, в GitHub/GitLab), может потребоваться снять защиту перед отправкой.

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

- `--force` и `--force-with-lease` перезаписывают историю на удалённом репозитории. Нужно убедиться, что это не нарушит работу других участников проекта. `--force-with-lease` cамо по себе, без уточнения деталей, защитит все удалённые ссылки, которые будут обновляться, требуя, чтобы их текущее значение совпадало с удалённой веткой отслеживания, которую мы для них создали.
- Если ветка защищена (например, в GitHub/GitLab), может потребоваться снять защиту перед отправкой.

### Альтернативный способ (без reset --hard)

Если нужно сохранить последующие коммиты как "отменённые", можно создать обратный коммит (revert):

sh

Copy

Download

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


git bisect
 Git сам поможет найти, в каком коммите всё сломалось.
 git bisect start
git bisect bad
git bisect good <commit_hash>
 6. git reflog
 Отображает историю перемещений HEAD, что позволяет восстановить потерянные коммиты:
 git reflog
 Незаменимая команда при восстановлении случайно удалённых изменений.
 7. git reset —hard <commit_hash>
 Сбрасывает текущую ветку к указанному коммиту, удаляя все последующие изменения:
 git reset —hard a1b2c3d
 Используй с осторожностью, так как это удаляет все несохранённые изменения.
