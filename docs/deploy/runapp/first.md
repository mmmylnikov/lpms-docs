# Первый запуск системы

Находясь в рабочем каталоге, запустите следующие команды:

``` bash title="~/lpms-app"
docker compose up 
```

Будут скачаны образы и запущены новые контейнеры

``` bash title="~/lpms-app"
mylnikov@MacBook-Air-Maksim lpms-app % docker compose up
[+] Running 12/12
 ✔ app 11 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                      2.6s 
   ✔ c6a83fedfae6 Already exists                                           0.0s 
   ✔ d7c82518a7cd Already exists                                           0.0s 
   ✔ 14d88fabbbe9 Already exists                                           0.0s 
   ✔ 5a3d34767148 Already exists                                           0.0s 
   ✔ a241ded2a651 Already exists                                           0.0s 
   ✔ 7e700ed7c05b Already exists                                           0.0s 
   ✔ f16ca674ba65 Already exists                                           0.0s 
   ✔ 3cc9bb7ceabb Already exists                                           0.0s 
   ✔ 018f2e6389ae Already exists                                           0.0s 
   ✔ 85974b77f050 Already exists                                           0.0s 
   ✔ 0334476208b0 Already exists                                           0.0s 
[+] Running 9/9
 ✔ Network lpms-app_default           Create...                            0.2s 
 ✔ Volume "lpms-app_lpms_static"      C...                                 0.0s 
 ✔ Volume "lpms-app_lpms_media"       Cr...                                0.0s 
 ✔ Volume "lpms-app_lpms_nginx_conf"  Created                              0.0s 
 ✔ Volume "lpms-app_lpms_db"          Creat...                             0.0s 
 ✔ Container lpms-app-adminer-1       Cr...                                0.3s 
 ✔ Container lpms-app-db-1            Created                              0.3s 
 ✔ Container lpms-app-app-1           Create...                            1.1s 
 ✔ Container lpms-app-nginx-1         Crea...                              0.2s 
 ...
```

Попробуйте открыть в браузере `http://<HOST>:8082/`. Вы должны увидеть панель администрирования базы данных Adminer.

!!! info "Примечание" 
    Если используете брандмауер, проверьте, чтобы были открыты внешние подключения к соответствующим портам

Однако система пока что не готова к работе. Адрес `http://<HOST>/` пока что не доступен, а в консоли будут следующие сообщения:

``` bash title="~/lpms-app"
...
nginx-1    | 2024/08/09 17:19:49 [emerg] 1#1: cannot load certificate "/etc/nginx/conf.d/cert1.pem": BIO_new_file() failed (SSL: error:80000002:system library::No such file or directory:calling fopen(/etc/nginx/conf.d/cert1.pem, r) error:10000080:BIO routines::no such file)
nginx-1    | nginx: [emerg] cannot load certificate "/etc/nginx/conf.d/cert1.pem": BIO_new_file() failed (SSL: error:80000002:system library::No such file or directory:calling fopen(/etc/nginx/conf.d/cert1.pem, r) error:10000080:BIO routines::no such file)
nginx-1 exited with code 1
...
```

Остановите запущенные контейнеры нажав на клавиатуре `Ctrl + C`.

Теперь необходимо настроить конфигурацию NGINX