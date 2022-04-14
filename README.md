# Яндекс.Практикум

# курс Python-разработчик

## студент  Leonid Slavutin

![](https://avatars.githubusercontent.com/u/86873729?s=400&u=79ca75646b1a1eb2fade4f19d435a8ba65a1fe58&v=4)

### Запуск проекта:
#### 1. В dev-режиме:
Клонировать репозиторий и перейти в него в командной строке:
```sh
git clone git@github.com:jood2302/infra_sp2.git
```
Установить и активировать виртуальное окружение:
```sh
python -m venv venv
source venv/Scripts/activate
python -m pip install --upgrade pip
```
Установить зависимости из файла requirements.txt
```sh
pip install -r requirements.txt
```
Выполнить миграции:
```sh
python manage.py migrate
``` 
Импортировать данные:
```sh
python manage.py loaddata fixtures.json
```
или из CSV-файла:
```sh
python manage.py import_from_csv <csv файл> <название модели>
```
В папке с файлом manage.py выполните команду:
```sh
python manage.py runserver
```

#### 2. Запуск в контейнере docker:
Скопировать `.env.example` и назвать его `.env`:
```sh
cp .env.example .env
```
Заполнить переменные окружения в `.env`:
```sh
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД

SECRET_KEY=secret_key # секретный ключ (вставте свой)
```
Запустить `docker-compose` командой:
```
docker-compose up -d --build
```
Собрать статику и выполнить миграции внутри контейнера, создать суперпользователя:
```sh
docker-compose exec web python manage.py migrate --noinput
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```
#### При необходимости возможно импортировать тестовые данные:
```sh
docker-compose exec web python manage.py loaddata fixtures.json
```
- При импорте создается суперюзер `admin` с паролем `admin`

## Деплой на удаленном сервере
Для запуска на удаленном сервере необходимо:
- перенести файлы `docker-compose.yaml` и папку `nginx` на сервер, выполнив команду:
```sh
scp -r ./infra/* <username>@<server_ip>:/home/<username>/
```

- на github, в настройках репозитория `Secrets` --> `Actions` создать и заполнить переменные окружения:
```sh
DOCKER_USERNAME # Имя пользователя на Docker Hub;
DOCKER_PASSWORD # Пароль от Docker Hub;
DB_ENGINE # Указать, что работаем с базой данных PostgresQl;
DB_NAME # Имя базы данных;
DB_HOST # Название контейнера базы данных; 
DB_PORT # Порт для подключения к базе данных;
POSTGRES_USER # Логин для подключения к базе данных;
POSTGRES_PASSWORD # Пароль для подключение к базе данных;
SECRET_KEY # Секретный ключ приложения;
USER # Имя пользователя на сервере;
HOST # Публичный IP-адрес сервера;
PASSPHRASE # Указать в том случае, если ssh-ключ защищен фразой-паролем;
SSH_KEY # Приватный ssh-ключ;
TELEGRAM_TO # ID телеграм-аккаунта;
TELEGRAM_TOKEN # Токен телеграм-бота.
```

### После каждого пуша (`git push`) в главную ветку `main`:
- будут автоматически запускаться тесты: проверка кода на соответствие стандарту `PEP8` (с помощью пакате `flake8`) и запуск `pytest` из репозитория `yamdb_final`;
- сборка и доставка докер-образа на `Docker Hub`;
- автоматический деплой на боевой сервер;
- отправка сообщения в `Telegram` при успешном завершении деплоя.

### Ресурсы API YaMDb
- Ресурс `auth`: аутентификация.
- Ресурс `users`: пользователи.
- Ресурс `titles`: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
- Ресурс `categories`: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
- Ресурс `genres`: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
- Ресурс `reviews`: отзывы на произведения. Отзыв привязан к определённому произведению.
- Ресурс `comments`: комментарии к отзывам. Комментарий привязан к определённому отзыву.


***Над проектом работал:***
* Leonid Slavutin | Github:https://github.com/jood2302 