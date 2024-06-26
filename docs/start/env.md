# Переменные окружения

## Основные

| Название переменной | Назначение / Валидные значения |
|---------------------|--------------------------------|
| `DEBUG`             |`True` или `False`              |
| `ALLOWED_HOSTS` | Хосты/доменные имена через запятую, которые являются допустимыми для данного сайта. Например: `127.0.0.1,localhost` |
| `CSRF_TRUSTED_ORIGINS` | Список доверенных испочников для небезопасных запросов Например: `https://lpms.mmmylnikov.ru` |
| `DJANGO_SECRET_KEY` | [Секретный ключ](https://docs.djangoproject.com/en/5.0/ref/settings/#secret-key) для вашей конкретной установки приложения. |
| `INTERNAL_IPS` | [Список IP адресов](https://django-debug-toolbar.readthedocs.io/en/latest/installation.html#configure-internal-ips) для вывода отладочной инфоормации|


## База данных

### Если используете SQLite

| Название переменной | Назначение / Валидные значения |
|---------------------|--------------------------------|
| `DATABASE_ENGINE` | `sqlite3` |

### Если используете PostgreSQL

| Название переменной | Назначение / Валидные значения |
|---------------------|--------------------------------|
| `DATABASE_ENGINE` | `postgresql` |
| `DATABASE_NAME` | Название базы данных |
| `DATABASE_USERNAME` | Имя пользователя базы данных  |
| `DATABASE_PASSWORD` | Пароль пользователя базы данных |
| `DATABASE_HOST` | По умолчанию `localhost`. При необходимости измените хост, если отличается |
| `DATABASE_PORT` | По умолчанию `5432`. При необходимости измените порт, если отличается |
| `DATABASE_OPTIONS` | При необходимости добавьте опции базы данных в формате json |



## Авторизация через GitHub OAuth

| Название переменной | Назначение / Валидные значения |
|---------------------|--------------------------------|
| `GITHUB_OAUTH_CLIENT_ID` | `Client ID` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers) |
| `GITHUB_OAUTH_SECRET` | `Client secrets` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers) |
| `GITHUB_OAUTH_REDIRECT_URL` | `Authorization callback URL` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers) |


## Уведомления через Telegram bot

| Название переменной | Назначение / Валидные значения |
|---------------------|--------------------------------|
| `TELEGRAM_BOT_TOKEN` | [`Token`](https://core.telegram.org/bots/features#creating-a-new-bot) вашего Телеграм бота. Позволяет отправлять уведомления студентам и кураторам о изменении статусов Тасков и Ревью. |


## Автокомплит через GitHub API (*опционально*)

| Название переменной | Назначение / Валидные значения |
|---------------------|--------------------------------|
| `GITHUB_API_TOKEN` | [`Personal access tokens (classic)`](https://github.com/settings/tokens) вашего аккаунта GitHub. Позволяет увеличить часовой лимит запросов от приложения к GitHub c 60 до 5000. Используется в задачах автокомплита пулл-реквестов и последующей сверки статуса ревью. |

