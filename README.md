# Kittygram — социальная сеть для обмена фотографиями любимых питомцев

## Описание:

Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Это полностью рабочий проект, который состоит из бэкенд-приложений на Django и фронтенд-приложений на React. Можно зарегистрироваться и авторизоваться, добавить нового котика на сайт или изменить существующего, а также просматривать записи других пользователей.

Учебный проект, созданный в рамках учебы в Яндекс.Практикуме.

## Технологии:

* Python 3.9
* Django 3.2.3
* djangorestframework 3.12.4
* djoser 2.1.0
* webcolors 1.11.1
* psycopg2-binary 2.9.3
* pytest-django 4.4.0
* pytest 6.2.4
* pytest-pythonpath 0.7.3
* PyYAML 6.0
* gunicorn 20.1.0
* python-dotenv 1.0.0

## Запуск проекта:

Клонировать репозиторий и перейти в него в командной строке:

```bash
git clone https://github.com/reginababaika/kittygram_final.git
cd kittygram_final
```

Cоздать и активировать виртуальное окружение, установить зависимости:

```bash
python3 -m venv venv && \ 
    source venv/scripts/activate && \
    python -m pip install --upgrade pip && \
    pip install -r backend/requirements.txt
```

Установите [docker compose](https://www.docker.com/) на свой компьютер.

Запустите проект через docker-compose:

```bash
docker compose -f docker-compose.yml up --build -d
```

Выполнить миграции:

```bash
docker compose -f docker-compose.yml exec backend python manage.py migrate
```

Соберите статику и скопируйте ее:

```bash
docker compose -f docker-compose.yml exec backend python manage.py collectstatic  && \
docker compose -f docker-compose.yml exec backend cp -r /app/static_backend/. /backend_static/static/
```

## .env:

В корне проекта создайте файл .env.

Скопируйте в него данные из файла .env.template:

```bash
cp .env.template .env
```

## Workflow:

Для использования Continuous Integration (CI) и Continuous Deployment (CD): в репозитории GitHub Actions `Settings/Secrets/Actions` прописать Secrets - переменные окружения для доступа к сервисам:

```
SECRET_KEY                     # стандартный ключ, который создается при старте проекта

DOCKER_USERNAME                # имя пользователя в DockerHub
DOCKER_PASSWORD                # пароль пользователя в DockerHub
HOST                           # ip_address сервера
USER                           # имя пользователя
SSH_KEY                        # приватный ssh-ключ (cat ~/.ssh/id_rsa)
PASSPHRASE                     # кодовая фраза (пароль) для ssh-ключа

TELEGRAM_TO                    # id телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN                 # токен бота (получить токен можно у @BotFather, /token, имя бота)
```

При push в ветку main автоматически отрабатывают сценарии:

* *tests* - проверка кода на соответствие стандарту PEP8 и запуск pytest. Дальнейшие шаги выполняются только если push был в ветку main;
* *build\_and\_push\_to\_docker\_hub* - сборка и доставка докер-образов на DockerHub
* *deploy* - автоматический деплой проекта на боевой сервер. Выполняется копирование файлов из DockerHub на сервер;
* *send\_message* - отправка уведомления в Telegram.

## Автор:
Регина Лубковская