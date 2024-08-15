# Интеграция внешних систем

Для корректной работы LMS Learn Python необходимо настроить поддержку внешних систем:

- GITHUB OAuth
    - Позволяет пользователям авторизоваться в системе используя учетную запись GitHub
- GITHUB API
    - Позволяет системе синхронизировать учебный процесс с прогрессом выполнения заданий и их ревью в репозиториях GitHub 
- TELEGRAM BOT API
    - Позволяет пользователям получать уведомления об изменении статусов учебных заданий


## GITHUB OAuth

> Назначение - авторизация пользователей

1. Перейдите в [настройки для разработчика](https://github.com/settings/developers) профиля GitHub
2. Создайте [новое OAuth App](https://github.com/settings/applications/new)
    - Задайте настройки:
        - *Application name* (например, `lpms-app`)
        - *Homepage URL* (например, `https://lms.python.ru/`)
        - *Application description* (опционально)
        - *Authorization callback URL*
            - должен иметь вид `http://<HOST>/users/accounts/github/login/callback/`
            - например, `http://lms.python.ru/users/accounts/github/login/callback/`
            - обратите внимание, что адрес для авторизации `<HOST>` должен соотвествовать [маске](https://docs.github.com/ru/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps#redirect-urls)
3. После сохранения:
    - запишите полученный *Client ID* 
    - создайте и запишите  *Client secret* 
    - загрузите лого приложения (опционально)

> Значения *Client ID (GITHUB_OAUTH_CLIENT_ID)*, *Client secret (GITHUB_OAUTH_SECRET)*, *Authorization callback URL (GITHUB_OAUTH_REDIRECT_URL)* будет необходимо указать в переменных окружения на последующих шагах.


## GITHUB API

> Назначение - повышение лимита суточных запросов для операций:
> 
> - Автокомплита названий репозиториев и пулл-реквестов студентов
> - Синхронизации статусов ревью для кураторов и студентов

1. Перейдите в [настройки для разработчика](https://github.com/settings/developers) профиля GitHub
2. Перейдите в раздел [Personal access tokens (classic)](https://github.com/settings/tokens)
3. Создайте новый [Personal access token](https://github.com/settings/tokens/new)
    - Задайте настройки:
        - *Note* (например, `lpms-app-token`)
        - *Expiration* (например, `30 days`)
        - *Select scopes* отмечать _не требуется_
4. После сохранения:
    - запишите полученный *GITHUB_API_TOKEN*

> Значение *GITHUB_API_TOKEN* будет необходимо указать в переменных окружения на последующих шагах.


## TELEGRAM BOT API

> Назначение - отправка уведомлений пользователям:
> 
> - Кураторы - о новых работах на проверку
> - Студенты - об обновлении статусов работ

1. Откройте клиент Telegram
2. Перейдите в диалог с ботом [@botfather](https://telegram.me/botfather)
3. Создайте бота с помощью команды `/newbot`
4. Придумайте username для бота (например, `lms-lp-bot`) и запишите его
5. Запишите полученный *TELEGRAM_BOT_TOKEN*
6. После измените профиль бота:
    - измените имя (например, `lms-lp-bot`) 
    - добавьте назначение (например, `Официальный бот LMS Learn Python`)
    - добавьте описание (например, `Бот для уведомления пользователей о изменении статусов задач и ревью`)
    - загрузите аватарку (опционально)

> Значения *TELEGRAM_BOT_USERNAME* и *TELEGRAM_BOT_TOKEN* будет необходимо указать в переменных окружения на последующих шагах.