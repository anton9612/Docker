1. Скачать репозиторий
2. Создать .env файл
3. Открыть Docker Desktop
4. В папке проекта выполнить команду:
   docker compose up --build -d
    Docker Compose соберёт образ приложения и запустит два контейнера:
      web — Django-приложение;
      db — PostgreSQL.
    После запуска сайт будет доступен
5. Для запуска в Production используется дополнительный файл docker-compose.prod.yml
   Для запуска выполните команду в папке проекта:
     docker compose -f docker-compose.yml -f docker-compose.prod.yml up --build -d

Для миграции данных были использованы команды Django dumpdata и loaddata
В проекте переносились записи модели Recipe.
1. Был открыт Powershall в папке проекта и выполнен:
   docker compose run --rm -e DB_ENGINE=sqlite -v "${PWD}/db.sqlite3:/app/db.sqlite3:ro" web python manage.py dumpdata recipe_app.Recipe --indent 2 --output recipes_data/recipes_fixture.json
   В данной команде была подключена исходная БД к контейнеру, переключен Django на SQLITE и экспортированы модели Recipe в JSON-файл
2. После экспорта была выполнена команда:
   docker compose exec web python manage.py loaddata recipes_data/recipes_fixture.json
   Django загрузила записи из JSON-файла в PostgreSQL
