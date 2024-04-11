# Сборка из docker compose

По умолчанию в образ системы включены:

- приложение LMPS. Доступно на порту `80`
- база данных PostgreSQL. Доступна на порту `5432`
- панель администрирования БД (Adminer). Доступен на порту `8080`
- веб-сервер Nginx


## Перед установкой

### Клонируйте репозиторий

``` sh
git clone https://github.com/mmmylnikov/lpms.git
```

### Настройте окружение

#### .env

``` sh
nano .env
```


``` ini title=".env"
# основные:
DEBUG=  # (1)
DJANGO_SECRET_KEY=  # (2)
DJANGO_ALLOWED_HOSTS=  # (3)
# база данных
DATABASE_ENGINE=postgresql
DATABASE_NAME=lmpsdb
DATABASE_USERNAME=lpmsuser
DATABASE_PASSWORD=lpmspassword
DATABASE_HOST=db  # (4)
DATABASE_PORT=5432  # (5)
DATABASE_OPTIONS={}  # (6)
# авторизация OAuth:
GITHUB_OAUTH_CLIENT_ID=  # (7)
GITHUB_OAUTH_SECRET=  # (8)
GITHUB_OAUTH_REDIRECT_URL=http://localhost:8000/oauth/accounts/github/login/callback/  # (9)
# уведомления
TELEGRAM_BOT_TOKEN=  # (10)
# автокомплит (опционально)
GITHUB_API_TOKEN=  # (11)

```

1. Укажите `True` - для режима отладки, иначе оставьте пустым 
2. [Секретный ключ](https://docs.djangoproject.com/en/5.0/ref/settings/#secret-key) для вашей конкретной установки приложения.
3. Хосты/доменные имена через запятую, которые являются допустимыми для данного сайта. Например: `127.0.0.1,localhost` 
4. При необходимости измените хост, если будете использовать бд не из образа
5. При необходимости измените порт, если будете использовать бд не из образа
6. При необходимости добавьте опции базы данных в формате json
7. `Client ID` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers)
8. `Client secrets` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers) |
9. `Authorization callback URL` для вашего приложения на GitHub [OAuth Apps](https://github.com/settings/developers)
10. [`Token`](https://core.telegram.org/bots/features#creating-a-new-bot) вашего Телеграм бота. Позволяет отправлять уведомления студентам и кураторам о изменении статусов Тасков и Ревью. 
11. ОПЦИОНАЛЬНО: [`Personal access tokens (classic)`](https://github.com/settings/tokens) вашего аккаунта GitHub. Позволяет увеличить часовой лимит запросов от приложения к GitHub c 60 до 5000. Используется в задачах автокомплита пулл-реквестов и последующей сверки статуса ревью.

#### docker-compose.yml

``` sh
nano docker-compose.yml
```

!!! warning "Обратите внимание"
    В файле `docker-compose.yml` выделена строка "30".
    В ней осуществляется загрузка тестовой базы материалов обучения.
    В настоящий момент тестовая база данных не поставляется с файлами системы. 
    Ее можно получить по запросу. Напишите [мне](https://t.me/mmmylnikov), я вам постараюсь помочь.
    Если у вас нет тестовой базы материалов - **удалите эту строку из файла**.


``` yml title="docker-compose.yml" linenums="1" hl_lines="30"
version: '3.9'
services:
  db:
    image: postgres
    ports:
      - 5432:5432
    restart: always
    shm_size: 128mb
    volumes:
      - lpms_db:/var/lib/postgresql/data
    env_file:
      - path: ./.env
        required: true
    environment:
      POSTGRES_USER: ${DATABASE_USERNAME}
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD}
      POSTGRES_DB: ${DATABASE_NAME}
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
  app:
    build: ./lpms
    restart: always
    depends_on:
      - db
    command: sh -c "python manage.py migrate &&
                    python manage.py collectstatic --noinput &&
                    python manage.py loaddata db_test.json &&
                    gunicorn config.wsgi:application --bind 0.0.0.0:8000"
    volumes:
      - lpms_static:/usr/src/app/shared/static
      - lpms_nginx_conf:/usr/src/app/shared/nginx
    expose:
      - 8000
    env_file:
      - path: ./.env
        required: true
  nginx:
    image: nginx:latest
    restart: always
    ports:
      - 80:80
    depends_on:
      - app
    volumes:
      - lpms_static:/shared/static
      - lpms_nginx_conf:/etc/nginx/conf.d/
volumes:
  lpms_db:
  lpms_static:
  lpms_nginx_conf:
```

## Запустите систему

``` bash 
docker compose up
```

!!! success "Поздравляю, система готова к работе!"

    Перейдите в панель администратора, чтобы добавить пользователей и 
    наполнить вашу систему учебными материалами:
    `http://localhost/admin/`
****
