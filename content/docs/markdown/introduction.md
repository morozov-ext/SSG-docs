---
title: "Введение в Markdown"
description: ""
summary: ""
date: 2024-10-22T15:58:35+03:00
lastmod: 2024-10-22T15:58:35+03:00
slug: "introduction"
draft: false
weight: 20
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

## Что такое Markdown? {#mark-1}

**Markdown** (Маркда́ун) — это легковесный **язык разметки**, то есть набор символов, которые стилизуют и придают структуру тексту.

Вы пишите текст в обычном текстовом редакторе по правилам **синтаксиса** Markdown, затем редакторы Markdown
преобразуют этот текст в HTML, а браузер обрабатывает HTML и выводит на экран конечный результат.

К слову, это руководство написано на Markdown.

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Языки разметки не относятся к языкам программирования, так как не выполняют программные вычисления.
{{< /callout >}}

Синтаксис Markdown в целом стандартизирован, но существуют разные **диалекты**, такие как [CommonMark](https://commonmark.org/), [GitHub Flavored Markdown](https://github.github.com/gfm/), [Multimarkdown](https://fletcherpenney.net/multimarkdown/). В них некоторые правила синтаксиса не совпадают, или появляются дополнительные функции. Например, `H~2~O` в одном случае обработается как H<sub>2</sub>O, а в другом — как H~2~O.



## Зачем использовать Markdown?

Во-первых, Markdown **очень прост в освоении**. По сравнению с такими языками разметки, как HTML, его можно выучить гораздо быстрее.

Во-вторых, Markdown **стандартизирован**. Любое устройство, которое может прочитать текстовый файл, отобразит синтаксис так, как
задумывалось. И сейчас, и в будущем.

В-третьих, Markdown **используется везде**. GitHub, Telegram, Discord, Reddit и многие другие веб-сайты и приложения работают на основе Markdown.

## Как начать изучать Markdown?

Лучше сразу на практике. Начните с [базового синтаксиса](../basic-syntax) и практикуйтесь.

Рекомендуем сначала пользоваться онлайн-редактором, например, [Dilinger](https://dillinger.io/). Результат ввода кода сразу отображается
в браузере, настраивать ничего не нужно. Если вам хочется локальный вариант, попробуйте легковесный редактор [Notepad++](https://notepad-plus-plus.org/downloads/) (доступ из России может быть затруднен) или [VS Code](https://code.visualstudio.com/).