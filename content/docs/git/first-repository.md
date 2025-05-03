---
title: "Создание репозитория"
description: ""
summary: ""
date: 2025-02-19T17:05:53+03:00
lastmod: 2025-02-19T17:05:53+03:00
slug: "first-repository"
draft: false
weight: 70
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Инициализация репозитория

**Локальный репозиторий** — это фактически обычная директория, помещенная под
версионный контроль. Для создания нового проекта выполните следующие шаги:

1.  Создайте новую директорию:

    ```bash {frame="none", title=""}
    mkdir git-repo
    ```

2.  Перейдите в созданную директорию:

    ```bash {frame="none", title=""}
    cd git-repo
    ```
    
3. Создайте git-репозиторий:

    ```bash {frame="none", title=""}
    git init
    ```

Можете перейти в директорию с вашим новым проектом и убедиться, что в ней
появились новые файлы. Однако не все содержимое `/git-repo` называется
репозиторием.

-  **Репозиторий** — автоматически созданная директория `.git`. Она
   находится под версионным контролем.
-  **Рабочая директория** — хранилище для файлов. Файлы в ней еще не
   включены в версионный контроль.

{{<img src="images/git-structure-1.png">}}

## Добавление файлов

Файлы в репозитории могут быть в
[нескольких состояниях]({{< relref "docs/git/introduction/#принцип-работы-git" >}}). Чтобы проверить статус репозитория и узнать, в каком состоянии находятся файлы,
введите команду:

```bash {frame="none", title=""}
$ git status

On branch main
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

Репозиторий пуст, на это указывает строка <nobr>`nothing to commit`.</nobr> Добавьте
пару файлов и еще раз проверьте статус репозитория.

```bash {frame="none", title=""}
echo 'Hello, world!' > README.md
echo 'Albert Einstein' > PEOPLE.md
```


{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Названия файлов нечувствительны к регистру в Windows и MacOS, но чувствительны
в Linux. Во избежание путаницы приучите себя соблюдать регистр.
{{< /callout >}}

```bash {frame="none", title=""}
$ git status

On branch main
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        PEOPLE.md
        README.md
nothing added to commit but untracked files present (use "git add" to track)
```

В проекте появились добавленные вами файлы со статусом `untracked`.
**Untracked (неотслеживаемые)** файлы — это файлы, которые еще не добавлены
в репозиторий, и Git не отслеживает в них изменения.

Но нельзя сразу зафиксировать файл в системе контроля версий. Сначала его
надо **индексировать**, то есть подготовить к дальнейшей работе.

```bash {frame="none", title=""}
git add README.md
```

{{< callout context="caution" title="Важно!" icon="outline/info-circle">}}
Команда `git add` не добавляет файл, она _подготавливает изменения_. Если вам надо
внести в индекс информацию о том, что файл удален, вы все равно используете
`git add`.
{{< /callout >}}

Проверьте статус репозитория.

```bash {frame="none", title=""}
$ git status

On branch main
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        PEOPLE.md
```
Теперь файл `README.md` находится в состоянии **staged (индексирован)**, а
`PEOPLE.md` остался в состоянии **untracked**. Это значит, что дальнейшие
изменения не затронут `PEOPLE.md`.

Последний шаг — это **коммит**. На этом этапе файл добавляется в
систему контроля версий, и все дальнейшие операции с ним отслеживаются.
Для описания коммита используется параметр `-m`.

```bash {frame="none", title=""}
$ git commit -m 'add README.md'

[main (root-commit) b9720a4] add README.md
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Старайтесь описывать коммиты емко и по существу, вам не раз придется искать
нужный коммит среди десятков других. Соблюдайте принятые стандарты
при работе над совместными проектами.
{{< /callout >}}

Проверьте статус репозитория.

```bash {frame="none", title=""}
$ git status

On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        PEOPLE.md
nothing added to commit but untracked files present (use "git add" to track)
```

Как видно, команда <nobr>`git status`</nobr> не показывает файлы в стадии `commited`.

Вы провели полную процедуру добавления файла и коммита. Это – основная суть
работы с Git. Теперь сделайте самостоятельно коммит файла `PEOPLE.md` и переходите
к следующему уроку.

---

## Дополнительная информация

-  [Как получить помощь?](https://git-scm.com/book/ru/v2/Введение-Как-получить-помощь%3F)
-  [Соглашение о коммитах](https://www.conventionalcommits.org/ru/v1.0.0/)
