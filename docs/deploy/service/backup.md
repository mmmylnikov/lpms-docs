# Резервное копирование

Перейдите в рабочий каталог

``` bash
cd ~/lpms-app
```

Создайте папку для резервного копирования дампов 

``` bash title="~/lpms-app"
mkdir backup
```

Для создания резервной копии текущего состояния базы данных выполните команды

``` bash title="~/lpms-app"
docker compose exec app sh -c "python -Xutf8 manage.py dumpdata --exclude auth.permission --exclude contenttypes > lmsdbdump.json"
docker compose cp app:/usr/src/app/lmsdbdump.json ./backup/$(date +"%Y%m%d_%H%M%S").json
```

Чтобы посмотреть список созданных дампов выполните команду

``` bash title="~/lpms-app"
ls -lh backup 
```

