# Docker Compose

Откройте файл `docker-compose.yml`

``` sh title="~/lpms-app"
nano docker-compose.yml
```

Дефолтное содержание с компонентами системы по-умолчанию

``` yml title="~/lpms-app/docker-compose.yml" linenums="1"
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
      - 8082:8080
  app:
    image: ghcr.io/mmmylnikov/lpms:latest
    restart: always
    depends_on:
      - db
    command: sh -c "python manage.py migrate &&
                    python manage.py collectstatic --noinput &&
                    gunicorn config.wsgi:application --bind 0.0.0.0:8000"
    volumes:
      - lpms_static:/usr/src/app/shared/static
      - lpms_media:/usr/src/app/shared/media
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
      - 443:443
    depends_on:
      - app
    volumes:
      - lpms_static:/shared/static
      - lpms_media:/shared/media
      - lpms_nginx_conf:/etc/nginx/conf.d/
volumes:
  lpms_db:
  lpms_static:
  lpms_media:
  lpms_nginx_conf:
```

!!! info "Примечание"
    При необходимости отредактируйте команды Docker Compose,
    например, если собираетесь использовать собственную базу данных,
    или если не хотите к примеру использовать Adminer

## Volumes

Для хранения данных вне контейнеров будут созданы следующие каталоги при инициализации системы:

- `lpms_db`: каталог для хранения таблиц базы данных
- `lpms_static`: каталог для хранения статических файлов приложения
- `lpms_media`: каталог для хранения медиа файлов приложения
- `lpms_nginx_conf`: каталог для хранения конфигурации NGINX

### Как узнать абсолютный путь к каталогу

Используйте команду `docker volume inspect <VOLUME_NAME>` для получения информации о томе. Например:

``` bash
docker volume inspect lpms-app_lpms_nginx_conf
```

В выводе команды найдите строку `"Mountpoint": "/var/lib/docker/volumes/lpms-app_lpms_nginx_conf/_data"`. Это путь к вашему каталогу.

