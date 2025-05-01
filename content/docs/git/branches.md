---
title: "Локальные ветки"
description: ""
summary: ""
date: 2025-02-23T12:40:35+03:00
lastmod: 2025-02-23T12:40:35+03:00
slug: "branches"
draft: false
weight: 110
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

В предыдущем уроке вы [выяснили](/docs/git/history), что Git стремится двигаться только вперед
и представляет собой односвязный список.

Но это упрощенная модель с одной **веткой** main.
В реальных проектах используется ряд односвязных списков, связанных
между собой.

<img src="/images/git-branch-4.png">

Ветка представляет собой последовательность коммитов в
хронологическом порядке. Но точнее будет сказать, что ветка — это _указатель_
на один из коммитов. С каждым новым коммитом ветка автоматически сдвигается,
а коммит создает ссылку на родительский.

Ветки помогают отделить основной проект от параллельно разрабатываемых задач.
К примеру, над open source проектами вместе работают многие людей, и
разработчики создают отдельные ветки для избежания путаницы. Когда разработчик
заканчивает работу над своей задачей, он создает **merge request** и сливает свою
ветку с основной.

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Ветка `main`/`master` — это не какая-то особенная ветка. Она обладает теми же
свойствами, как и все остальные, да и называться может как угодно. Просто
она создается первой, и поэтому считается главной.
{{< /callout >}}

## Создание веток

Есть два варианта, как начать работать с новой веткой:

1.  Создайте ветку командой <nobr>`git branch <имя_ветки>`</nobr>, а затем переключитесь на нее
командой <nobr>`git checkout <имя_ветки>`</nobr>.
2.  Создайте и сразу переключитесь на новую ветку командой
    <nobr>`git checkout -b <имя_ветки>`.</nobr>

```bash {frame="none", title=""}
$ git checkout -b feature

Switched to a new branch 'feature'
```

Командой `git branch` также можно посмотреть список имеющихся веток.
Для этого введите ее без аргументов.

```bash {frame="none", title=""}
$ git branch

* feature # Символ "*" указывает на активную ветку
  main
```

Теперь вы можете спокойно работать с файлами, основную ветку это не затронет.
Сделайте любой коммит, например:

```bash {frame="none", title=""}
$ echo 'Awesome feature #1' > FEATURES.md
$ git add FEATERUS.md
$ git commit -m 'add FEATURE.md'
```

Изучите с помощью `git log`, что происходит с ветками и коммитами.
Добавьте параметр `--all`, чтобы видеть историю всех веток, а не только
активной.

```bash {frame="none", title=""}
$ git log --all --oneline

85c5af2 (HEAD -> feature) add FEATURE.md
487b3bd (origin/main, main) update README.md PLANETS.md
b317d23 Revert "delete PEOPLE.md"
fd420ae add PLANETS.md
d8756eb delete PEOPLE.md
3810583 add PEOPLE.md
5811348 add README.md
```

`HEAD` — это указатель на последний коммит и на активную ветку. Сейчас
он указывает на ветку `feature`. Если вы сделаете новый коммит и/или ветку,
`HEAD` передвинется на шаг вперед. А если переключитесь на предыдущий
коммит командой `git checkout 487b3bd`, то на шаг назад.


## Слияние веток

### Fast-forward

На данный момент ветка `feature` находится на один шаг впереди от ветки `main`.
При этом в ветке `main` изменений не было. Это очень простая ситуация, так как одна ветка буквально продолжает другую.

Схематично это выглядит так:

<img src="/images/git-branch-5.png">

Перед началом слияния убедитесь, что вы находитесь в той ветке, _куда_ хотите
влить другую. Перейдите в ветку `main` и выполните из нее команду
`git merge <имя_ветки>`.

```bash {frame="none", title=""}
$ git merge feature

Updating 487b3bd..85c5af2
Fast-forward
 FEATURES.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 FEATURES.md
```

Обратите внимание на запись **fast-forward**. Она означает, что Git просто
перенес указатель ветки вперед, так как ветка `feature` была прямым
потомком.

### Трехстороннее слияние

Усложним задачу. Сделайте два новых коммита в ветке `feature`,
а затем еще один в ветке `main`, как показано ниже:

```bash {frame="none", title=""}
$ git checkout feature
$ git echo 'Awesome feature #2' >> FEATURES.md
$ git commit -a -m 'update FEATURES.md'
$ sed -i 's/Venus/Mars/' PLANETS.md
$ git commit -a -m 'update PLANETS.md'
$ git checkout main
$ sed -i 's/Venus/Jupiter/' PLANETS.md
$ git commit -a -m 'update PLANETS.md'
```

Теперь ситуация выглядит так:

<img src="/images/git-branch-6.png">

Слияние методом fast-forward невозможно, так как в ветке `main` появился
еще один коммит. В этом случае Git автоматически выполнит **трехстороннее
слияние** на основе последних коммитов обеих веток и общего родительского
коммита.

Удостоверьтесь, что находитесь в ветке `main`, а затем выполните команду слияния.

```bash {frame="none", title=""}
$ git merge feature

Auto-merging PLANETS.md
CONFLICT (content): Merge conflict in PLANETS.md
Automatic merge failed; fix conflicts and then commit the result.'
```

Однако Git выдал ошибку `merge conflict`. Почему это произошло?

### Конфликт слияния

Ошибка возникла из-за разницы в файлах, которые вы изменили
[ранее](/docs/git/branches/#трехстороннее-слияние).

```bash {frame="none", title=""}
$ git checkout feature

sed -i 's/Venus/Mars/' PLANETS.md

$ git checkout main

sed -i 's/Venus/Jupiter/' PLANETS.md
```

Один из файлов, которые вы пытаетесь объединить, содержит противоречащие записи
в разных ветках. Обнаружив **конфликт слияния**, Git остановит операцию и
попросит вас это исправить.

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Вы не сможете перейти в другую ветку, пока не разрешите конфликт слияния.
{{< /callout >}}

Чтобы просмотреть текущие конфликты, воспользуйтесь командой <nobr>`git status`</nobr>.

```bash {frame="none", title=""}
$ git status

On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)
Changes to be committed:
        modified:   FEATURES.md
Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   PLANETS.md
```

Откройте в директории вашего проекта проблемный файл `PLANETS.md`. Вы увидите,
что Git разметил его **маркерами конфликта**. Ваша задача — оставить в файле
только один вариант (или изменить оба) и удалить маркеры конфликта.

```bash {frame="none", title=""}
Mercury
<<<<<<< HEAD
Jupiter
=======
Mars
>>>>>>> feature
```

К примеру, отредактируйте файл так, как показано ниже, и сохраните его:

```bash {frame="none", title=""}
Mercury
Jupiter
Mars
```

Добавьте файл в индекс командой <nobr>`git add`</nobr>, и Git пометит конфликт как разрешенный.
Теперь создайте коммит слияния командой <nobr>`git commit`</nobr>. Откроется редактор, где
вам предложат изменить название коммита или оставить название по умолчанию. После этого шага 
операция трехстороннего слияния завершится.

```bash {frame="none", title=""}
$ git commit

[main 9d61b3e] Merge branch 'feature'
```

## Перебазирование

Есть еще один способ перенести изменения из одной ветки в другую:
**перебазирование**. Отличие от слияния в следующем:

-  **Слияние** — на общий родительский коммит наслаиваются
   изменения из последних коммитов обеих веток.
-  **Перебазирование** — коммиты с изменениями из одной ветки
   последовательно переносятся на другую ветку.

<img src="/images/git-branch-7.png">

Принципиальная разница заключается в истории коммитов. Слияние запоминает всю историю
изменений в ветках и сам коммит слияния. Перебазирование перемещает коммиты из
одной ветки в другую, создавая линейную историю.

{{< callout context="caution" title="Важно!" icon="outline/info-circle">}}
Не используйте `git rebase` для коммитов, уже отправленных в публичный
репозиторий. Помните, что этой операцией вы создаете новые коммиты. Другим
людям придется заново выполнять слияние, что приведет к путанице.
{{< /callout >}}

Вернитесь к состоянию репозитория непосредственно перед слиянием с помощью команды
<nobr>`git reset`</nobr> и вместо слияния выполните перебазирование.

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Слияние делается из той ветки, куда вы вливаете изменения, а перебазирование — 
наоборот.
{{< /callout >}}

```bash {frame="none", title=""}
$ git reset --hard HEAD~
```

```bash {frame="none", title=""}
$ git checkout feature
$ git rebase main

Auto-merging PLANETS.md
CONFLICT (content): Merge conflict in PLANETS.md
error: could not apply 05994f2... update PLANETS.md
```

Как и в случае со слиянием, возникает конфликт, который надо
[разрешить вручную](/docs/git/branches/#конфликт-слияния).

После этого выполните два шага:

1. Завершите процедуру перебазирования.

```bash {frame="none", title=""}
$ git add PLANETS.md
$ git rebase --continue

[detached HEAD 78ed0d2] update PLANETS.md
 1 file changed, 1 insertion(+)
Successfully rebased and updated refs/heads/feature.
```

2. Переключитесь на ветку `main` и выполните слияние методом fast-forward.

```bash {frame="none", title=""}
$ git checkout main
$ git merge feature

Updating e8d83d8..78ed0d2
Fast-forward
 FEATURES.md | 1 +
 PLANETS.md  | 1 +
 2 files changed, 2 insertions(+)
```

Готово. Результат абсолютно идентичен обычному слиянию,
разница только в истории коммитов.

## Удаление веток

После завершения слияния обе ветки находятся в одинаковом состоянии, так что
ненужную ветку можно удалить. Это делается командой <nobr>`git branch -d <имя_ветки>`.</nobr>

```bash {frame="none", title=""}
$ git branch -d feature

Deleted branch feature (was 05994f2).
```

---

## Дополнительная информация

-  [Основы ветвления и слияния](https://git-scm.com/book/ru/v2/Ветвление-в-Git-Основы-ветвления-и-слияния)
-  [Стратегии ветвления](https://javarush.com/groups/posts/2693-komandnaja-rabota-bez-putanicih-razbiraem-strategii-vetvlenija-v-gite)