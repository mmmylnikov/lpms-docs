# Переменные окружения

Откройте файл `.env`

``` sh title="~/lpms-app"
nano .env
```

Добавьте необходимые переменные окружения

``` ini title="~/lpms-app/.env"
# основные:
DEBUG=  # (1)
DJANGO_SECRET_KEY=  # (2)
DJANGO_ALLOWED_HOSTS=  # (3)
CSRF_TRUSTED_ORIGINS=  # (12)

# база данных (если используете PostgreSQL из Docker Compose)
DATABASE_ENGINE=postgresql
DATABASE_NAME=lmpsdb
DATABASE_USERNAME=lpmsuser
DATABASE_PASSWORD=lpmspassword
DATABASE_HOST=db  # (4)
DATABASE_PORT=5432  # (5)
DATABASE_OPTIONS={}  # (6)

# база данных (если используете SQLite)
# DATABASE_ENGINE=sqlite3
# другие настройки "DATABASE_<ARG>" не требуются

# авторизация OAuth:
GITHUB_OAUTH_CLIENT_ID=  # (7)
GITHUB_OAUTH_SECRET=  # (8)
GITHUB_OAUTH_REDIRECT_URL=  # (9)

# автокомплит и проверка работ
GITHUB_API_TOKEN=  # (11)

# уведомления
TELEGRAM_BOT_TOKEN=  # (10)
TELEGRAM_BOT_USERNAME=  # (13)
```

1. Укажите `True` - для режима отладки, иначе оставьте пустым 
2. Придумайте [cекретный ключ](https://docs.djangoproject.com/en/5.0/ref/settings/#secret-key) для вашей конкретной установки приложения.
3. Хосты/доменные имена через запятую, которые являются допустимыми для данного сайта. Например: `127.0.0.1,localhost,lms.python.ru,auth.python.ru` 
4. При необходимости измените хост, если будете использовать БД не из стека
5. При необходимости измените порт, если будете использовать БД не из стека
6. При необходимости добавьте опции базы данных в формате json
7. `Client ID` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers)
8. `Client secrets` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers) |
9. `Authorization callback URL` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers)
10. [`Token`](https://core.telegram.org/bots/features#creating-a-new-bot) вашего Телеграм бота. Позволяет отправлять уведомления студентам и кураторам о изменении статусов Тасков и Ревью. 
11. [`Personal access tokens (classic)`](https://github.com/settings/tokens) вашего аккаунта GitHub. Позволяет увеличить часовой лимит запросов от приложения к GitHub c 60 до 5000. Используется в задачах автокомплита пулл-реквестов и последующей сверки статуса ревью.
12. Список [доверенных хостов](https://docs.djangoproject.com/en/5.0/ref/settings/#csrf-trusted-origins) для данного сайта. Например: `http://lms.python.ru,http://auth.python.ru,https://lms.python.ru,https://auth.python.ru`
13. `@username` для вашего Телеграм бота. Указывайте его без @. Например: `lms_lp_bot`


### Примечания

Как получить отдельные переменные можно прочитать здесь:

- <a href="../external/#github-oauth">GITHUB_OAUTH</a>
- <a href="../external/#github-api">GITHUB_API_TOKEN</a>
- <a href="../external/#telegram-bot-api">TELEGRAM_BOT_TOKEN</a>
