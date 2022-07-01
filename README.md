# Учебный проект api_yamdb завернутый в докер

## Как запустить проект:

1. Клонировать репозиторий
2. В каталоге /infra создать файл .env c с подобной структурой:
 ```
 DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=qwerty123 # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
 ```
3. В командной строке перейти в папку infra, запустите docker-compose в фоновом режимекомандой:
```
docker-compose up -d
```
4. Применить миграции:
```
docker-compose exec web python manage.py migrate

```
5. Создать суперпользователя:
```
sudo docker-compose exec web python manage.py createsuperuser

```
6. Собрать статику:
```
docker-compose exec web python manage.py collectstatic --no-input 

```
7. Теперь проект доступен по адресу http://localhost/.
8. админка доступна по адресу http://localhost/admin/.