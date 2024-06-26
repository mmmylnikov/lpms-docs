# 2. Описание системы

Система состоит из следующих основных функциональных блоков:

1. Регистрация, аутентификация и авторизация
2. Функционал для Куратора
3. Функционал для Обучающегося
4. Функционал оплаты курса
6. Функционал интеграции с Github
7. Функционал интеграции с Telegram
8. Функционал интеграции с YouTube
9. Функционал интеграции с Google Slides


## 2.1. Типы пользователей

Система предусматривает два типа пользователей системы: 

- Куратор
- Обучающийся

Куратор: 

- наполняет курс материалом, 
- организует работу обучающихся, 
- проверяет домашние задания, 
- организует встречи.

Обучающийся: 

- получает доступ к учебным материалам, 
- управляет статусом выполнения домашнего задания.


## 2.2. Регистрация пользователей 

### Регистрация **Куратора**

В первой версии системы допускается заведение Куратора в Систему при помощи команды на сервере, это допускается. Этой команде заведения автора должны быть переданы на вход следующие данные:

* gh_username — обязательное поле (должен совпадать с login в API GitHUB)
* tg_username — обязательное поле (должен совпадать с login в API Telegram)
* пароль — обязательное поле

Процесс заведения Куратора через команду на сервере должен быть описан в
документации Системы.

### Регистрация **Обучающегося**

Процесс регистрации Обучающихся, должен быть реализован в интерфейсах Системы. 
Система должна иметь возможность использоваться GitHub OAuth для автоматического создания учетной записи с заполненными полями

* gh_username — обязательное поле (должен совпадать с login в API GitHUB)
* имя и фамилия — обязательное поле (если указаны в API GitHub)

Интерфейс системы должен иметь возможность заполнить другие обязательные поля:

* tg_username — обязательное поле (должен совпадать с login в API Telegram)


## 2.3. Аутентификация пользователей

Аутентификация Куратора и Обучающегося осуществляется с помощью GitHub OAuth


## 2.4. Функционал для Куратора

Куратор после аутентификации получает доступ к 
своему функционалу в Системе. Этот функционал состоит из
следующих блоков:

1. Редактирование данных профиля
2. Заведение и редактирование Курсов
3. Заведение и редактирование Учебных материалов
4. Заведение и редактирование Обучения
5. Учебный процесс
6. Аналитика


### 2.4.1. Редактирование профиля

В этом разделе у Куратора есть возможность редактирования данных
своего профиля —  логин в телеграм, фамилия и имя.


### 2.4.2. Заведение и редактирование Курсов

Учебные материалы Курса относятся к Треку (направлению подготовки)

На Курсе учебные материалы по Трекам группируются по Неделям.


### 2.4.3. Заведение и редактирование Учебных материалов

Учебные материалы могут быть представлены:
- текстом (краткое описание задания)
- видеоматериал
- слайды
- исходники


### 2.4.4. Заведение и редактирование Обучения

В зависимости от Набора (Потока) должна иметься возможность менять наполнение Недель, и иметься возможность быстрого копирования наполнения Недель в новый Набор (Поток) Курса.

Обучающиеся отдельного Потока отдельного Курса группируются в Группы, которые курирует отдельный Куратор

### 2.4.5. Учебный процесс

Куратор видит в Системе задания которые необходимо проверить и имеет быстрый доступ к материалам Обучающегося

### 2.4.6. Аналитика

Куратор видит следующую аналитику:

1. Количество студентов в потоке и группе
2. Количество выполненных заданий
3. Количество проверенных заданий


## 2.5. Функционал для Обучающегося

Обучающийся после аутентификации получает доступ к 
своему функционалу в Системе. Этот функционал состоит из
следующих блоков:

1. Редактирование данных профиля
2. Просмотр Курсов
3. Просмотр Учебных материалов
4. Выполнение Обучения
5. Учебный процесс
6. Аналитика


### 2.4.1. Редактирование профиля

В этом разделе у Обучающегося есть возможность редактирования данных
своего профиля —  логин в телеграм, фамилия и имя.


### 2.4.2. Просмотр Курсов

Обучающийся видит Курсы к которым у него есть доступ


### 2.4.3. Просмотр Учебных материалов

По мере обучения у Обучающегося открывается доступ к материалам новых недель


### 2.4.4. Выполнение Обучения

Обучающийся имеет возвожность прикрепить ссылку на материалы своего домашнего задания через интерфес системы и тем самым уведомить Куратора.

По мере проверки у Обучающегося менятся статус выполнения домашего задания: 

- доступно для выполнения
- на проверке
- требует исправления
- выполнено

### 2.4.6. Аналитика

Обучающийся видит следующую аналитику:

1. Количество выполненных заданий
2. Количество проверенных заданий

## 2.6. Функционал оплаты курса

Чтобы иметь доступ к материалам курса Обучающийся должен иметь возможность произвести оплату до начала программы текущего набора (потока).

Куратор должен иметь возможность зачислить Обучающегося на курс, после успешной оплаты.

Возможность возврата оплаты (частичной оплаты) за курс функционалом системы не предусматривается. 

## 2.7. Функционал интеграции с GitHub

Посредством API GitHub Система должна осуществлять

1. Аутентификцию пользователей
2. Быстрый доступ для Обучающегося:

    - к репозиториям с учебными материалами
    - к личным репозиториям с домашними заданиями
    - к отправленным пулл реквестам по выполненным домашним заданиям
    - к статусам проверки пулл реквества Куратором

3. Быстрый доступ для Куратора:

    - к репозиториям с учебными материалами
    - к личным репозиториям Обучающихся с домашними заданиями
    - к отправленным Обучающимися пулл реквестам по выполненным домашним заданиям
    - к статусам проверки пулл реквества Куратором


## 2.8. Функционал интеграции с YouTube

Пользователи Системы должны иметь возможность доступа к учебным видеоматериалам загруженным на YouTube без непосредственного перехода на сторонний сервис

## 2.9. Функционал интеграции с Google Slides

Пользователи Системы должны иметь возможность доступа к учебным материалам загруженным на Google Slides без непосредственного перехода на сторонний сервис


## 2.8. Функционал интеграции с Telegram

Автоматически Обучающимся должны приходить уведомления в Telegram:

- об успешной оплате курса
- об успешном зачислении на курс
- о новых материалах по программе обучения в Системе
- об проверенном Куратором задании

Автоматически Кураторам должны приходить уведомления в Telegram:

- об новых оплатах курса
- о выполненных домашних заданиях Обучающихся в отмеченных Системе. 
