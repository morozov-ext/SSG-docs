---
title: "Внесение изменений"
description: ""
summary: ""
date: 2025-02-23T12:40:35+03:00
lastmod: 2025-02-23T12:40:35+03:00
slug: "modify"
draft: false
weight: 90
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Теперь вернитесь к работе с репозиторием, который вы недавно [создали](/docs/git/first-repository/). На данный момент все измененные файлы уже внесены в коммиты, а
рабочий каталог чист.

```bash {frame="none", title=""}
$ git status

On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

## Отмена изменений

На любой стадии до выполнения коммита изменения можно **откатить**.

{{< callout context="caution" title="Важно!" icon="outline/info-circle">}}
После отката изменения уже не вернуть. Будьте внимательны.
{{< /callout >}}

### Неотслеживаемые файлы

Создайте несколько любых файлов:

```bash {frame="none", title=""}
$ touch hello
$ mkdir bonjour
$ git status

On branch main
Your branch is up to date with 'origin/main'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello
```

Файлы и пустые папки из рабочей директории, которые еще не зафиксированы
в репозитории, удаляются командой <nobr>`git clean -fd`.</nobr>

-  Параметр `f` — принудительное удаление;
-  Параметр `d` — удаление папок.

```bash {frame="none", title=""}
$ git clean -f
$ git status

On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

### Измененные файлы

Измените один из существующих файлов:

```bash {frame="none", title=""}
$ echo 'Nikola Tesla' >> PEOPLE.md
$ git status

On branch main
Your branch is up to date with 'origin/main'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   PEOPLE.md
no changes added to commit (use "git add" and/or "git commit -a")
```

Измененные файлы, которые еще не попали в индекс, откатываются
до предыдущего состояния командой `git restore <имя_файла>`.

```bash {frame="none", title=""}
$ git restore PEOPLE.md
$ git status

On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

### Индексированные файлы

Измените один из существующих файлов и добавьте его в индекс:

```bash {frame="none", title=""}
$ echo 'Nikola Tesla' >> PEOPLE.md
$ git add PEOPLE.md
$ git status

On branch main
Your branch is up to date with 'origin/main'.
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   PEOPLE.md
```

Индексированные файлы восстанавливаются командой <nobr>`git restore --staged <имя_файла>`.</nobr>

При этом изменения не откатываются, а просто убираются из индекса. Чтобы теперь отменить
изменения, выполните `git restore` из
[предыдущего шага](/docs/git/modify/#измененные-файлы).

```bash {frame="none", title=""}
$ git restore --staged PEOPLE.md
$ git status

On branch main
Your branch is up to date with 'origin/main'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   PEOPLE.md
no changes added to commit (use "git add" and/or "git commit -a")
```

```bash {frame="none", title=""}
$ git restore PEOPLE.md
$ git status

On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

## Отмена коммитов

Изменения в файлах рабочей директории — это рядовая ситуация. Пока файлы не попали в коммит,
система контроля версий еще не записала их в историю. А вот
к отмене коммитов лучше отнестись внимательнее и не «мусорить» в проекте.

### git revert

Представьте, что вы сделали коммит, а потом поняли, что эти изменения были
не нужны. А может, это было даже несколько коммитов назад. Но историю
коммитов вы не хотите перезаписывать, чтобы не ломать структуру проекта.

<img src="/images/git-branch-2.png">

Сделайте несколько коммитов, например:

```bash {frame="none", title=""}
git rm PEOPLE.md
git commit -m 'delete PEOPLE.md'
git echo 'Mercury' > PLANETS.md
git add PLANETS.md
git commit -m 'add PLANETS.md'
```

Но выяснилось, что `PEOPLE.md` удалять было не нужно. Чтобы не
проводить обратную операцию, используйте команду <nobr>`git revert <хеш_коммита>`</nobr>.

**Хеш коммита** — это уникальный идентификатор для каждого отдельного коммита.
Его можно узнать командой <nobr>`git log`.</nobr> Подробнее об этом —
в разделе [История](/docs/git/history/).

```bash {frame="none", title=""}
$ git log --oneline

fd420ae (HEAD -> main) add PLANETS.md
d8756eb delete PEOPLE.md
3810583 (origin/main) add PEOPLE.md
5811348 add README.md
```

Найдите хеш коммита, который вы хотели бы отменить. Нужны только
первые семь цифр, остальные вводить необязательно.

Теперь выполните команду <nobr>`git revert`</nobr>.

```bash {frame="none", title=""}
git revert d8756eb
```

Откроется консольный текстовый редактор [nano](https://help.ubuntu.ru/wiki/nano).
В нем вам предложат изменить название нового коммита. Оставьте фразу
по умолчанию и нажмите `CTRL+X` для выхода из nano.

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Git открывает либо nano, либо vim. Это зависит от текстового редактора по умолчанию
в вашей системе. Вы можете изменить его командой
<nobr>`git config --global core.editor <название_редактора>`</nobr>
{{< /callout >}}

<img src="/images/git-nano.png">

Готово, коммит отменен.

### git reset

Другая ситуация: вам нужно полностью удалить коммит с изменением
истории. К примеру, вы только что по ошибке сделали коммит и уверены, что никто еще
не успел продолжить работу с этой версией. Тогда допустимо использовать команду `git reset`.

<img src="/images/git-branch-3.png">

Сделайте любой коммит, например:

```bash {frame="none", title=""}
echo 'Vulkan' >> PLANETS.md
git commit PLANETS.md -m 'modify PLANETS.md'
```

Выполните команду <nobr>`git reset --hard HEAD~1`.</nobr>
- `--hard` — полное удаление. Если его не ставить, то по умолчанию
используется параметр `--mixed`, и изменения последнего коммита отправляются
в рабочую директорию.
- `HEAD~1` — это последний сделанный коммит. Если нужно удалить два коммита,
то используйте `HEAD~2`, и т. д.

```bash {frame="none", title=""}
$ git reset --hard HEAD~1
$ git log --oneline

b317d23 (HEAD -> main) Revert "delete PEOPLE.md"
fd420ae add PLANETS.md
d8756eb delete PEOPLE.md
3810583 (origin/main) add PEOPLE.md
5811348 add README.md
```

Готово, целевой коммит и все последующие удалены.

---

## Дополнительная информация

-  [Операции отмены](https://git-scm.com/book/ru/v2/Основы-Git-Операции-отмены)