---
title: "Интеграция с GitHub"
description: ""
summary: ""
date: 2025-02-12T18:17:01+03:00
lastmod: 2025-02-12T18:17:01+03:00
slug: "integration"
draft: false
weight: 80
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

## Регистрация

Рано или поздно вам понадобится разрабатывать проекты [совместно с другими разработчиками](/docs/git/introduction). Для этого вам потребуется зарегистрироваться в [GitHub](https://github.com/signup). GitHub — это облачная платформа для хостинга Git-репозиториев.

В подавляющем большинстве случаев разработчики выкладывают
свои open source проекты именно на GitHub, но есть и другие сервисы.
Например, [GitLab](https://about.gitlab.com/), [Bitbucket](https://bitbucket.org/product/), [Azure DevOps](https://azure.microsoft.com/ru-ru/products/devops).

## SSH-ключи

Когда вы связываетесь с серверами GitHub через командную строку, вы должны использовать один из двух
**протоколов безопасности**: SSH или HTTPS. HTTPS проще в настройке, но SSH обеспечивает более высокий уровень безопасности.

С помощью SSH-ключа вы сможете подключаться к GitHub без необходимости каждый раз
вводить данные для аутентификации. 

### Создание ключа

1.  Сгенерируйте SSH-ключ в вашей системе

```bash {frame="none", title=""}
ssh-keygen -t ed25519 -C "<ваш_email>"
```

Когда появится запрос `Enter a file in which to save the key`, нажмите `Enter`. 
Ключ появится в директории по умолчанию `~/.ssh/`.

Когда появится запрос `Enter passphrase (empty for no passphrase)`, придумайте
и введите пароль для SSH-ключа.

```cmd {frame="none", title=""}
> Enter passphrase (empty for no passphrase): <ваш_пароль>
> Enter same passphrase again: <ваш_пароль_повторно>
```

2.   Активируйте SSH-агент и добавьте в него SSH-ключ

**SSH-агент** — это программа, которая хранит ваши SSH-ключи и предоставляет
их GitHub по запросу.

```bash {frame="none", title=""}
# Запустить агент SSH
eval "$(ssh-agent -s)"
```

```bash {frame="none", title=""}
# Добавить SSH-ключ в агент
ssh-add ~/.ssh/id_ed25519
```

### Добавление SSH-ключа в учетную запись GitHub

1.  Скопируйте ключ в буфер обмена.

{{< tabs "ssh" >}}
{{< tab "Bash" >}}

```bash {frame="none", title=""}
clip < ~/.ssh/id_ed25519.pub
```

{{< /tab >}} 
{{< tab "PowerShell" >}}

```bash {frame="none", title=""}
cat ~/.ssh/id_ed25519.pub | clip
```
{{< /tab >}} 
{{< tab "WSL" >}}

```bash {frame="none", title=""}
clip.exe < ~/.ssh/id_ed25519.pub
```

{{< /tab >}}
{{< /tabs >}}

2.  В вашем аккаунте GitHub откройте настройки и перейдите на вкладку
[SSH and GPG keys](https://github.com/settings/keys).
3.  Нажмите кнопку **New SSH key**.
4.  Напишите любое название в поле **Title** и вставьте из буфера обмена
    SSH-ключ в поле **Key**.
5.  В поле **Key type** выберите **Authentication Key**.
6.  Нажмите **Add SSH key**.

## Создание удаленного репозитория

1.  Перейдите на страницу с вашими проектами и нажмите кнопку **New**.
2.  Введите имя проекта в поле **Repository name**.
3.  (Опционально) Введите описание проекта в поле _Description_.
<img src="/images/git-registration-1.png">
4.  Выберите тип проекта. Публичный виден всем, приватный — только вам.
<img src="/images/git-registration-2.png">
5.  (Опционально) Создайте дополнительные файлы:
    -  `README.md` — описание проекта. Вы его 
       [уже создали](/docs/git/first-repository/#добавление-файлов),
       пропустите этот шаг.
    -  `.gitignore` — список игнорируемых файлов. Не всегда имеет смысл загружать
       на GitHub все файлы из локального репозитория.
    -  `Лицензия` — позволяет или запрещает другим использовать ваш код.
<img src="/images/git-registration-3.png">
6.  Нажмите кнопку **Create repository**.

## Связывание локального и удаленного репозиториев

Теперь у вас есть два репозитория: в вашей системе и на GitHub. Осталось их
связать, чтобы в дальнейшем изменения **синхронизировались**.

{{< callout context="note" title="Обратите внимание" icon="outline/info-circle">}}
Команды выполняются из директории проекта.
{{< /callout >}}

1.  Свяжите репозитории командой <nobr>`git remote add`,</nobr> которая принимает два аргумента:
    - Название удаленного репозитория, как правило, `origin`.
    - URL-адрес удаленного репозитория. 
    
    ```bash {frame="none", title=""}
    git remote add origin git@github.com:<псевдоним_на_github>/<имя_проекта>.git
    ```

2.  (Опционально) Переименуйте основную ветку. По умолчанию основная ветка
    в Git называется `master`, а в GitHub – `main`. Неважно, какое название
    вы выберете, главное, чтобы оно совпадало.
    
    ```bash {frame="none", title=""}
    git branch -M main
    ```

3.  Запушьте (отправьте) ваш локальный репозиторий на сервер. Параметр `-u`
используется только первый раз, он связывает ветки. В дальнейшем достаточно
писать <nobr>`git push`.</nobr>
    
    ```bash {frame="none", title=""}
    git push -u origin main
    ```
    
    Если появится следующее сообщение, введите `yes`.

    ```bash {frame="none", title=""}
    The authenticity of host 'github.com (140.82.121.4)' can't be established.
    ...
    Are you sure you want to continue connecting (yes/no/[fingerprint])
    ```

Готово. Можете обновить страницу и изучить структуру вашего нового репозитория
на GitHub.

---

## Дополнительная информация
-  [Сведения о протоколе SSH](https://docs.github.com/ru/authentication/connecting-to-github-with-ssh/about-ssh)
-  [Создание нового ключа SSH](https://docs.github.com/ru/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=windows)
-  [Почему ветка master стала main?](https://habr.com/ru/news/506876/)