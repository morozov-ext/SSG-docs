---
title: "История"
description: ""
summary: ""
date: 2025-02-23T12:40:35+03:00
lastmod: 2025-02-23T12:40:35+03:00
slug: "history"
draft: false
weight: 100
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Для понимания принципов работы Git нужно уяснить несколько моментов:
-  Ветка коммитов представляет собой **односвязный список**, т.е. каждый
   новый коммит базируется на предыдущем.
-  Git стремится двигаться только вперед. Удаление файла —
   это, как правило, новый коммит, а не изменение старого.
-  В любой момент разработчик может посмотреть изменения от самого первого
   до последнего коммита.

{{<img src="/images/git-branch-1.png">}}

Благодаря этим принципам Git является строго упорядоченной системой версий, и
работать с историей изменений в нем очень удобно.

## Анализ изменений

Предположим, что у вас есть несколько файлов, которые еще не попали в коммит, но вы уже
запутались. Где и что удалено, добавлено?

Для анализа изменений в файлах используется команда <nobr>`git diff`.</nobr>

Внесите изменения в два файла, как показано ниже. Не добавляйте их в индекс.

```bash {frame="none", title=""}
$ echo 'Venus' >> PLANETS.md
$ echo 'This is a real deal' > README.md
```

Теперь выполните команду <nobr>`git diff`</nobr> и изучите полученные данные.

```bash {frame="none", title=""}
$ git diff

diff --git a/PLANETS.md b/PLANETS.md # Показывает, что сравниваются две версии файла
index 5857748..725a5a9 100644   
--- a/PLANETS.md # Оригинальный файл
+++ b/PLANETS.md # Измененный файл
@@ -1 +1,2 @@
 Mercury # Строка без изменений
+Venus   # Добавленная строка
diff --git a/README.md b/README.md
index af5626b..d965296 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-Hello, world!       # Удаленная строка
+This is a real deal # Добавленная строка
```

Запись `@@ -1 +1,2 @@` — это индикатор
измененных строк, где `-` указывает на оригинальный файл, а `+` — на измененный.
При этом:
-  `-1` — изменения произошли в первой строке оригинального файла;
-  `+1,2` — изменения произошли в двух строках измененного файла, начиная с первой.

Чтобы сравнить _индексированные_ файлы, добавьте параметр `--staged`. Хорошей привычкой
будет проверять изменения перед коммитом.

```bash {frame="none", title=""}
git diff --staged
```

## История коммитов

Точное содержание своих коммитов, не говоря уже о чужих, легко забыть. Работа идет непрерывно, информации много. Рассмотрим основные
команды для просмотра истории.

### git log

Команда `git log` выводит список всех коммитов, из которого можно узнать:

-  хеш коммита;
-  описание коммита;
-  автора коммита;
-  дату коммита;
-  текущую ветку и позицию HEAD;
-  последнюю синхронизацию с удаленным репозиторием.

Последний коммит отображается вверху списка.

```bash {frame="none", title=""}
$ git log

Author: moroz <morozov.ext@gmail.com>
Date:   Mon Feb 24 12:50:40 2025 +0300
    Revert "delete PEOPLE.md"
    This reverts commit d8756ebeca1b77dc0ba5f9e28d0e60e0e406e3b2.
commit fd420ae3f1265fe8010da815304d1841a9402cf0
Author: moroz <morozov.ext@gmail.com>
Date:   Mon Feb 24 12:24:27 2025 +0300
    add PLANETS.md
commit d8756ebeca1b77dc0ba5f9e28d0e60e0e406e3b2
Author: moroz <morozov.ext@gmail.com>
Date:   Mon Feb 24 12:19:12 2025 +0300
    delete PEOPLE.md
commit 3810583635cdf072688b8ee541b0b1148515fc5b (origin/main)
Author: moroz <morozov.ext@gmail.com>
Date:   Sun Feb 23 13:57:46 2025 +0300
    add PEOPLE.md
commit 5811348e0b043f5d0738d17d1e2596daef9919e8
Author: moroz <morozov.ext@gmail.com>
Date:   Sun Feb 23 13:57:25 2025 +0300
    add README.md
```

Но в большинстве случаев достаточно сокращенного вывода с добавочным
параметром `--oneline`.

```bash {frame="none", title=""}
$ git log --oneline

b317d23 (HEAD -> main) Revert "delete PEOPLE.md"
fd420ae add PLANETS.md
d8756eb delete PEOPLE.md
3810583 (origin/main) add PEOPLE.md
5811348 add README.md
```

Посмотреть разницу между коммитами можно с помощью параметра `-p`.
Также вы можете ограничить количество выведенных коммитов, например, двумя,
дописав `-2`.

```bash {frame="none", title=""}
$ git log -p -2 --oneline

b317d23 (HEAD -> main) Revert "delete PEOPLE.md"
diff --git a/PEOPLE.md b/PEOPLE.md
new file mode 100644
index 0000000..2921ab7
--- /dev/null
+++ b/PEOPLE.md
@@ -0,0 +1 @@
+Albert Einstein
fd420ae add PLANETS.md
diff --git a/PLANETS.md b/PLANETS.md
new file mode 100644
index 0000000..5857748
--- /dev/null
+++ b/PLANETS.md
@@ -0,0 +1 @@
+Mercury
```

### git show

Вы научились смотреть изменения в последних коммитах с помощью <nobr>`git log`</nobr>,
но что, если нужен коммит из середины списка?

Для этого используйте команду <nobr>`git show <хеш_коммита>`.</nobr>

```bash {frame="none", title=""}
$ git show d8756eb

b317d23 (HEAD -> main) Revert "delete PEOPLE.md"
diff --git a/PEOPLE.md b/PEOPLE.md
new file mode 100644
index 0000000..2921ab7
--- /dev/null
+++ b/PEOPLE.md
@@ -0,0 +1 @@
+Albert Einstein
fd420ae add PLANETS.md
diff --git a/PLANETS.md b/PLANETS.md
new file mode 100644
index 0000000..5857748
--- /dev/null
+++ b/PLANETS.md
@@ -0,0 +1 @@
+Mercury
```

### git grep

Если вам нужно найти конкретное слово или набор слов, используйте команду
<nobr>`git grep <текст>`.</nobr> Для игнорирования регистра добавьте параметр `-i`.

```bash {frame="none", title=""}
$ git grep -i einstein

PEOPLE.md:Albert Einstein
```

### git blame

При работе в команде порой нужно узнать, кто и в каком коммите менял файл.
Для этого используйте команду <nobr>`git blame <имя_файла>`.</nobr>

```bash {frame="none", title=""}
$ git blame PLANETS.md

fd420ae3 (moroz             2025-02-24 12:24:27 +0300 1) Mercury
00000000 (Not Committed Yet 2025-02-25 16:36:40 +0300 2) Venus
```

Обратите внимание, что выводятся даже файлы, которые еще не попали в коммит.

---

## Дополнительная информация

-  [Просмотр истории коммитов](https://git-scm.com/book/ru/v2/Основы-Git-Просмотр-истории-коммитов)