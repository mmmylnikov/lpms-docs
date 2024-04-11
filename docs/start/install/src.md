# Установка из иcходников

> Протестировано на Python 3.12

## Перед установкой

Клонируйте репозиторий

``` sh
git clone https://github.com/mmmylnikov/lpms.git
cd lpms
```

Создайте виртуальное окружение

``` sh
python3.12 -m venv venv
source venv/bin/activate
```

Установите необходимые [зависимости](../start/requirements.md)

``` sh
pip install -r lpms/requirements.txt
```

## Настройка окружения

Установите [переменные окружения](../start/env.md)

### Основные

``` sh
nano .env
```

``` ini title=".env"
DEBUG=  # (1)
DJANGO_SECRET_KEY=  # (2)
DJANGO_ALLOWED_HOSTS=  # (3)
...
```

1. Укажите `True` - для режима отладки, иначе оставьте пустым 
2. [Секретный ключ](https://docs.djangoproject.com/en/5.0/ref/settings/#secret-key) для вашей конкретной установки приложения.
3. Хосты/доменные имена через запятую, которые являются допустимыми для данного сайта. Например: `127.0.0.1,localhost` 

### Если используете SQLite

``` ini title=".env"
...
DATABASE_ENGINE=sqlite3
...
```

> Данные будут храниться в каталоге `lpms/shared/db.sqlite3`

### Если используете PostgreSQL

Подготовьте базу данных

``` bash 
sudo -u postgres psql
CREATE DATABASE lmpsdb;
CREATE USER lpmsuser WITH PASSWORD 'lpmspassword';
ALTER ROLE lpmsuser SET client_encoding TO 'utf8';
ALTER ROLE lpmsuser SET default_transaction_isolation TO 'read committed';
ALTER ROLE lpmsuser SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE lmpsdb TO lpmsuser;
\q
```

Добавьте настройки базы данных в переменные окружения

``` ini title=".env"
...
DATABASE_ENGINE=postgresql
DATABASE_NAME=lmpsdb
DATABASE_USERNAME=lpmsuser
DATABASE_PASSWORD=lpmspassword
DATABASE_HOST=localhost  # (1)
DATABASE_PORT=5432  # (2)
DATABASE_OPTIONS={}  # (3)
...
```

1. При необходимости измените хост, если отличается
2. При необходимости измените порт, если отличается
3. При необходимости добавьте опции базы данных в формате json

### GitHub

Добавьте настройки аккаунта GitHub для Авторизации пользователей (обязательно) и Автокомплита полей (опционально)

``` ini title=".env"
...
# обязательно:
GITHUB_OAUTH_CLIENT_ID=  # (1)
GITHUB_OAUTH_SECRET=  # (2)
GITHUB_OAUTH_REDIRECT_URL=http://localhost:8000/oauth/accounts/github/login/callback/  # (3)
# опционально:
GITHUB_API_TOKEN=  # (4)
...
```

1. `Client ID` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers)
2. `Client secrets` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers) |
3. `Authorization callback URL` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers)
4. ОПЦИОНАЛЬНО: [`Personal access tokens (classic)`](https://github.com/settings/tokens) вашего аккаунта GitHub. Позволяет увеличить часовой лимит запросов от приложения к GitHub c 60 до 5000. Используется в задачах автокомплита пулл-реквестов и последующей сверки статуса ревью.

### Telegram бот

Добавьте настройки Telegram бота для оправки пользователям уведомлений

``` ini title=".env"
...
TELEGRAM_BOT_TOKEN=  # (1)
```

1. [`Token`](https://core.telegram.org/bots/features#creating-a-new-bot) вашего Телеграм бота. Позволяет отправлять уведомления студентам и кураторам о изменении статусов Тасков и Ревью. 

## Перед запуском

Выполните миграции

``` bash 
cd lpms/
python manage.py migrate
```

Выполните сбор статических файлов

``` bash 
python manage.py collectstatic
```

> Статические данные по-умолчанию сохраняются в каталог `lpms/shared/static/`


Создайте учетную запись администратора

``` bash 
python manage.py createsuperuser
```

Загрузите тестовую базу материалов обучения

!!! warning "Обратите внимание"

    В настоящий момент тестовая база данных не поставляется с файлами системы. 
    Ее можно получить по запросу. Напишите [мне](https://t.me/mmmylnikov), я вам постараюсь помочь.

``` bash 
python python manage.py loaddata db_test.json
```

## Запустите приложение

``` bash 
gunicorn config.wsgi:application --bind 0.0.0.0:8000
```

!!! success "Поздравляю, система готова к работе!"

    Перейдите в панель администратора, чтобы добавить пользователей и 
    наполнить вашу систему учебными материалами:
    `http://localhost:8000/admin/`
****
