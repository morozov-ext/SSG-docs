---
title: "Удаленные ветки"
description: ""
summary: ""
date: 2025-02-23T12:40:35+03:00
lastmod: 2025-02-23T12:40:35+03:00
slug: "remote-branches"
draft: false
weight: 120
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

## Отправка изменений

Локальные ветки не синхронизируются автоматически с удаленными. Если вы сейчас
используете команду <nobr>`git push`</nobr>, то отправите в удаленный репозиторий только
ветку `main`, которую связали 
[ранее](/docs/git/integration/#связывание-локального-и-удаленного-репозиториев).

Отправьте в удаленный репозиторий ветку `feature`.
Правда, вы ее недавно
[удалили](http://localhost:1313/docs/git/branches/#удаление-веток), так что
создайте заново.

```bash {frame="none", title=""}
$ git checkout -b feature
$ git push origin feature

Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'feature' on GitHub by visiting:
remote:      https://github.com/morozov-ext/learning-repo/pull/new/feature
remote:
To github.com:morozov-ext/learning-repo.git
 * [new branch]      feature -> feature
```

А теперь введите команду <nobr>`git branch -vv`</nobr>, которая покажет локальные ветки
и связанные с ней удаленные.

```bash {frame="none", title=""}
$ git branch -vv
  feature 78ed0d2 update PLANETS.md
* main    78ed0d2 [origin/main] update PLANETS.md
```

Обратите внимание на запись `origin/main`. Это — **ветка слежения**, то есть
локальная ветка, которая передает информацию об удаленной. Если
другой разработчик внесет изменения в эту ветку на GitHub, вы сможете
узнать об этом с помощью ветки слежения и скопировать себе актуальную версию.

Однако напротив ветки `feature` такая запись отсутствует, потому что мы
отправили ее в репозиторий, но не настроили на слежение. Удостоверьтесь,
что находитесь в нужной ветке и выполните команду
<nobr>`git branch -u <удаленный_репозиторий>/<имя_ветки>`.</nobr>

```bash {frame="none", title=""}
$ git branch -u origin/feature

Branch 'feature' set up to track remote branch 'feature' from 'origin'.
```

Но проще делать это еще на этапе `git push`, добавив параметр `-u`.

```bash {frame="none", title=""}
$ git push -u origin feature
```

Повторите команду `git branch -vv` и убедитесь, что ветка слежения
`origin/feature` появилась.

```bash {frame="none", title=""}
$ git branch -vv

* feature 78ed0d2 [origin/feature] update PLANETS.md
  main    78ed0d2 [origin/main] update PLANETS.md
```

{{< callout context="caution" title="Важно!" icon="outline/info-circle">}}
Информация из `git branch -vv` может быть устаревшей. Эта команда не
обращается к серверам, а использует локальный кэш. Кэш обновляется
только при командах `git push`, `git pull` и `git fetch`.
{{< /callout >}}

## Получение изменений

В совместных проектах вам часто придется обновлять свою локальную копию
репозитория на основе изменений от других разработчиков. Это можно
сделать двумя способами.

### git fetch

Команда <nobr>`git fetch <удаленный_репозиторий>`</nobr> загружает из удаленного репозитория все изменения, но
не заменяет автоматически локальные файлы. Это удобно, так как вы можете
сначала изучить эти изменения, а уже потом применить их командой
<nobr>`git merge <удаленный_репозиторий>/<имя_ветки>`.</nobr>

```bash {frame="none", title=""}
$ git fetch origin
$ git merge origin/feature
```

### git pull

Команда <nobr>`git pull <удаленный_репозиторий> <имя_ветки>`</nobr> совмещает <nobr>`git fetch`</nobr> и <nobr>`git merge`.</nobr> Все
изменения применяются автоматически.

```bash {frame="none", title=""}
$ git pull origin feature

From github.com:morozov-ext/learning-repo
 * branch            feature    -> FETCH_HEAD
Already up to date.
```

## Удаление изменений

Чтобы удалить ветку в в удаленном репозитории, используйте команду
<nobr>`git push <удаленный_репозиторий> --delete <имя_ветки>`.</nobr>

```bash {frame="none", title=""}
$ git push origin --delete feature

To github.com:morozov-ext/learning-repo
 - [deleted]         feature
```

## Дополнительная информация

-  [Удаленные ветки](https://git-scm.com/book/ru/v2/Ветвление-в-Git-Удалённые-ветки)