Цель: практика использования Docker Compose.

Задание:  аналогично предыдущей задаче, запусти необходимую для работы приложения СУБД, используя файл docker-compose.yaml.

Файл можно создать в папке проекта.

Вместо консольного запуска можно использовать возможности IDE, но рекомендуется попробовать оба варианта.


статья: https://jtprog.ru/docker-base/

# Версия Docker API
version: '3.7'
# Сервисы которые мы будем запускать
services:
# Первый сервис - db
db:
# Образ на основе которого он будет запускаться
image: postgres:12-alpine
# volumes - магическая вещь, которая создает некоторое устройство в
# рамках Docker и монтирует его в директорию /var/lib/postgresql/data
volumes:
- postgres_data:/var/lib/postgresql/data/
# Переменные окружения
environment:
POSTGRES_USER: movie
POSTGRES_PASSWORD: 123456
POSTGRES_DB: movie
# Говорим открыть снаружи порт 5432
expose:
- 5432

# Второй сервис - app
app:
# Говорим что его надо будет собрать - в качестве контекста
# передаем текущую директорию - в ней лежит Dockerfile
build: .
# Монтируем локальную директорию ./src в директорию
# внутри контейнера /opt/app
volumes:
- ./src:/opt/app
# Говорим пробросить порт 8000 хоста в порт 8000 контейнера
ports:
- 8000:8000
# Зависит от сервиса db - запускать после него
depends_on:
- db
# Просто говорим создать volume с именем postgres_data
volumes:
postgres_data:

доп материал:
https://www.youtube.com/playlist?list=PLD5U-C5KK50XMCBkY0U-NLzglcRHzOwAg
