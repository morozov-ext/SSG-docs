---
title: "Установка"
description: ""
summary: ""
date: 2025-02-10T17:52:52+03:00
lastmod: 2025-02-10T17:52:52+03:00
slug: "installation"
draft: false
weight: 60
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Windows

Перейдите на [страницу загрузки Git](https://git-scm.com/downloads/win) на
официальном сайте. Вы можете скачать
[последнюю поддерживаемую сборку](https://github.com/git-for-windows/git/releases/download/v2.47.1.windows.2/Git-2.47.1.2-64-bit.exe),
любую другую подходящую версию или использовать инструмент [WinGet](https://learn.microsoft.com/en-us/windows/package-manager/winget/).

Запустите установщик и следуйте инструкциям. Если хотите изменить компоненты
по умолчанию, ознакомьтесь с
[описанием настроек](https://selectel.ru/blog/tutorials/how-to-install-git-to-windows/#get-install-windows).

Альтернативный вариант — установите Git через
[WSL (Windows Subsystem for Linux)](https://learn.microsoft.com/ru-ru/windows/wsl/install).
Это простой способ запустить среду Linux прямо в Windows без виртуальных машин.
Заодно вы сразу начнете осваивать Linux.

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
В пакет установки входит **Git Bash**. Это приложение командной строки для
ввода команд Git и Bash. Используйте его в этих уроках, если работаете в Windows.
{{< /callout >}}

## Linux и MacOS

Установите Git через пакетные менеджеры:

{{< tabs "git-install" >}}
{{< tab "Ubuntu" >}}
```bash
sudo apt update # Проверяем актуальность версии
sudo apt install git-all
```
{{< /tab >}}
{{< tab "MacOS" >}}
```bash
brew install git
```
{{< /tab >}}
{{< /tabs >}}

## Первоначальная настройка

После установки Git нужно задать имя пользователя и адрес электронной почты.
Эта информация будет включена в каждый последующий коммит.

```bash
# Выполняется из любой директории
git config --global user.name "<имя фамилия>"
git config --global user.email "<ваш email>"
```

-  Если хотите поменять имя и адрес, выполните команды еще раз.
-  Чтобы посмотреть текущие настройки, используйте команду <nobr>`git config --list`.</nobr>
-  Настройки хранятся в вашей системе в директории `~/.gitconfig` или `~/.config/git/config`.

## Редактор кода

Если вы еще не пользовались специализированными редакторами кода, то
сейчас самое время начать. Они облегчают работу за счет
таких функций, как:

-  подсветка синтаксиса;
-  автоматическая расстановка отступов;
-  автозаполнение;
-  быстрое переключение между файлами;
-  запуск, компиляция и отладка кода.

Хорошие бесплатные редакторы для новичков —
[Visual Studio Code](https://code.visualstudio.com)
или [Notepad++](https://notepad-plus-plus.org/downloads/),
хотя можете выбрать любой другой.

---

## Дополнительные материалы

-  [Установка Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
-  [Что такое WSL?](https://learn.microsoft.com/ru-ru/windows/wsl/about)

