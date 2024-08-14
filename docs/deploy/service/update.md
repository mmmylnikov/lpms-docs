# Установка обновлений

Перейдите в рабочий каталог

``` bash
cd ~/lpms-app
```

Остановите запущенные сервисы

``` bash title="~/lpms-app"
docker compose down
```

Удалите контейнер `app`

``` bash title="~/lpms-app"
docker compose stop app
docker compose rm app
```

Скачайте образ заново

``` bash title="~/lpms-app"
docker compose pull app
```

Запустите обновленные сервисы

``` bash title="~/lpms-app"
docker compose up
```

