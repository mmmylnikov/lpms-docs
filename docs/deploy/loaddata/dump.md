# Загрузка дампа

Вместе с системой поставляется дамп базы данных, который содержит:

- Брендированную тему оформления панели администрирования
- Содержание курсов Learn Python и Learn Python Advanced
- Уроки и практические задания для курсов
- Программы обучения

## Скопируйте файлы дампа в каталог `lpms-app`

1) Перейдите в каталог `lpms-app`

``` bash
cd ~/lpms-app
```

2) Поместите полученный дамп `lms-dump.tar.gz` в каталог `lpms-app`

3) Разархивируйте архив `lms-dump.tar.gz` в каталог `lpms-app`

``` bash
tar -xvzf lms-dump.tar.gz
```

## Загрузка данных в систему

1) Запустите контейнер `lpms_app-app-1` в фоновом режиме:

``` bash title="~/lpms-app"
docker-compose up -d app
```

2.1) Загрузите тему интерфейса администрирования в каталог `lpms_nginx_conf`

``` bash title="~/lpms-app"
docker compose cp lms-dump/admin-theme/theme.json app:/usr/src/app/shared/nginx/theme.json
```

2.2) Загрузите курсы и треки в каталог `lpms_nginx_conf`

``` bash title="~/lpms-app"
docker compose cp lms-dump/course/course.json app:/usr/src/app/shared/nginx/course.json
```

3) Остановите контейнер `lpms_app-app-1`

``` bash title="~/lpms-app"
docker compose stop app db
```

4) Запустите контейнер `lpms_app-app-1` в интерактивном режиме:

``` bash title="~/lpms-app"
docker compose run app sh
```

5.1) Загрузите дамп темы оформления в базу данных

``` bash title="/usr/src/app"
python manage.py loaddata shared/nginx/theme.json
```

Если загрузка прошла успешно, то вы увидите сообщение

``` bash title="/usr/src/app"   
/usr/src/app # python manage.py loaddata shared/nginx/theme.json
Installed 2 object(s) from 1 fixture(s)
```

5.2) Загрузите дамп курсов и треков в базу данных

``` bash title="/usr/src/app"
python manage.py loaddata shared/nginx/course.json
```

Если загрузка прошла успешно, то вы увидите сообщение

``` bash title="/usr/src/app"   
/usr/src/app # python manage.py loaddata shared/nginx/course.json
Installed 11 object(s) from 1 fixture(s)
```


6) Завершите работу контейнера `lpms_app-app-1`

``` bash title="/usr/src/app"
exit
```

7) Завершите работу контейнера `lpms_app-db-1`

``` bash title="~/lpms-app"
docker compose stop db
```

## Загрузка дампа Учебных материалов

Загрузка уроков, заданий и программ обучения будет рассмотрено в следующих шагах через интерфейс администрирования.