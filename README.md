# API_YamDB

REST API для сервиса YaMDb — базы отзывов о фильмах, книгах и музыке.

Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории: «Книги», «Фильмы», «Музыка».
Произведению может быть присвоен жанр. Новые жанры может создавать только администратор.
Читатели оставляют к произведениям текстовые отзывы и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти).
Из множества оценок автоматически высчитывается средняя оценка произведения.

Аутентификация по JWT-токену

Поддерживает методы GET, POST, PUT, PATCH, DELETE

Предоставляет данные в формате JSON

Cоздан в команде из трёх человек с использованим Git в рамках учебного курса Яндекс.Практикум.

## Стек технологий

- проект написан на Python с использованием Django REST Framework
- библиотека Simple JWT - работа с JWT-токеном
- библиотека django-filter - фильтрация запросов
- базы данных - PostgreSQL
- система управления версиями - git
- [Docker](https://docs.docker.com/engine/install/ubuntu/) - официальная документация Docker
- [Dockerfile](https://docs.docker.com/engine/reference/builder/) - официальная документация Dockerfile
- [Docker Compose](https://docs.docker.com/compose/) - официальная документация Docker Compose

## Ресурсы API YaMDb

**AUTH**: аутентификация.

**USERS**: пользователи.

**TITLES**: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).

**CATEGORIES**: категории (типы) произведений ("Фильмы", "Книги", "Музыка").

**GENRES**: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.

**REVIEWS**: отзывы на произведения. Отзыв привязан к определённому произведению.

**COMMENTS**: комментарии к отзывам. Комментарий привязан к определённому отзыву.

## Алгоритм регистрации пользователей

Пользователь отправляет POST-запрос с параметром email на `/api/v1/auth/email/`.
YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
Пользователь отправляет POST-запрос с параметрами email и confirmation_code на `/api/v1/auth/token/`, в ответе на запрос ему приходит token (JWT-токен).
Эти операции выполняются один раз, при регистрации пользователя. В результате пользователь получает токен и может работать с API, отправляя этот токен с каждым запросом.

## Пользовательские роли

**Аноним** — может просматривать описания произведений, читать отзывы и комментарии.

**Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, дополнительно может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.

**Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять и редактировать любые отзывы и комментарии.

**Администратор (admin)** — полные права на управление проектом и всем его содержимым. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.

**Администратор Django** — те же права, что и у роли Администратор.

## Запуск проекта в Докере:

1. Сколнируйте репозиторий
2. В каталоге /infra создайте файл .env c аналогичной структурой:
 ```
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=qwerty123 # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
 ```
3. В командной строке перейдите в папку ./infra/, запустите docker-compose в фоновом режиме командой:
```
docker-compose up -d
```
4. Примените миграции:
```
docker-compose exec web python manage.py migrate

```
5. Создайте суперпользователя:
```
docker-compose exec web python manage.py createsuperuser

```
6. Соберите статику:
```
docker-compose exec web python manage.py collectstatic --no-input 

```
Теперь проект доступен по адресу http://localhost/.

Админка доступна по адресу http://localhost/admin/.

При желании можно заполнить БД тестовыми данными:

```
docker-compose exec web python3 manage.py loaddata fixtures.json
```
Как остановить контейнер:
```
docker-compose down -v
```
Как пересобрать контейнер:
```
docker-compose up -d --build
```
## Как пользоваться

После запуска проекта, подробную инструкцию можно будет посмотреть по адресу http://127.0.0.1:8000/redoc/ (http://localhost/redoc/)

## Образ Docker
Образ Docker находится в репзитории по адресу:
https://hub.docker.com/repository/docker/pavelpatsey/infra_web