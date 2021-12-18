# Документация к API YaMDb (v1)

## Описание проекта:

Проект YaMDb собирает отзывы пользователей на различные произведения
сохранённые в базе данных (БД) проекта. Произведения делятся на категории:
«Книги», «Фильмы», «Музыка» и т.д. Список категорий может быть расширен
администратором (например, можно добавить категорию «Изобразительное искусство»
или «Ювелирка»).

Система не хранит в своей БД YaMDb исходный контент, нельзя посмотреть фильм
или послушать музыку.

Новые произведения могут вносить только администраторы, для рядовых
пользователей данный функционал недоступен. Произведению может быть присвоен
жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»).
Новые жанры также может создавать только администратор.

## Запуск проекта:

1. Скопируйте проект

```bash
git clone https://github.com/Ahttps://github.com/AsmodaiP/infra_sp2  
```

2. Создайте env-файл с переменными окружения и наполнить его по шаблону:

```bash
 SECRET_KEY=p&l%385148kslhtyn^##a1)ilz@4zqj=rq&agdol^##zgl9(vs
 DB_ENGINE=django.db.backends.postgresql
 DB_NAME=postgres
 POSTGRES_USER=postgres
 POSTGRES_PASSWORD=postgres
 DB_HOST=db
 DB_PORT=5432
 DEBUG=True
 ALLOWED_HOSTS=*
```

3. Установить Docker ([инструкция](https://docs.docker.com/engine/install/))
4. Установить Docker Compose ([инструкция](https://docs.docker.com/compose/install/))
5. Перейдите в директорию infra и запустите Docker-compose

```bash
cd infra/
 docker-compose up 
```

6. Выполните миграции, соберите статику создайте суперпользователя и загрузите тетовые данные:

   ```bash
   docker-compose exec web python manage.py migrate --noinput
   docker-compose exec web python manage.py createsuperuser
   docker-compose exec web python manage.py collectstatic --no-input
   docker-compose exec web python manage.py loaddata fixtures.json

   ```

## Работа с эндпоинтами:

Краткое описание основных возможностей, за более подробной информацией
обратитесь к [/redoc/](http://127.0.0.1:8000/redoc/)

### Авторизация с применением JWT токена

#### Регистрация нового пользователя

Для получения JWT-токена необходимо отправить **JSON** запрос, содержащий
имя пользователя и имя почтового ящика.

**JSON** запрос:

```JSON
{
  "email": "string",
  "username": "string"
}
```

POST: [/api/v1/auth/signup/](http://127.0.0.1:8000/api/v1/auth/signup/)

В письме придёт код подтверждения. Дальше необходимо необходимо отправить
**JSON** запрос, содержащий код подтверждения:

```JSON
{
  "username": "string",
  "confirmation_code": "string"
}
```

POST: [/api/v1/auth/token/](http://127.0.0.1:8000/api/v1/auth/token/)

В ответ вы получите токен для доступа к сервису.

```JSON
{
  "token": "string"
}
```

Вам будут доступны:

- Категории:
  [/api/v1/categories/](http://127.0.0.1:8000/api/v1/categories/)
- Жанры:
  [/api/v1/genres/](http://127.0.0.1:8000/api/v1/genres/)
- Произведения:
  [/api/v1/titles/](http://127.0.0.1:8000/api/v1/titles/)
- Отзывы:
  [/api/v1/titles/{title_id}/reviews/](http://127.0.0.1:8000/api/v1/titles/1/reviews/)
- Комментарии:
  [/api/v1/titles/{title_id}/reviews/{review_id}/comments/](http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/)

Авторами проекта являются: [Дмитрий Перегуда](https://github.com/AsmodaiP), [Сергей Жаров](https://github.com/zhss1983), [Иван Осянин](https://github.com/IvanOsyaninhttps://github.com/IvanOsyanin).

Были использованы следующие технологии: Django 3.05, Django Rest Framework 3.12.4, gunicorn 20.0.4, python 3.8.
