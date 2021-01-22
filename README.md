# Infra_sp2

Данный проект демонстрирует возможность автоматичекого развертывания другого проекта ([api_yamdb](https://github.com/SergeyMMedvedev/api_yamdb)) с помощью Docker. 

Docker — программное обеспечение для автоматизации развёртывания и управления приложениями в средах с поддержкой контейнеризации.



## Начало

Для начала необоходимо установить Docker на свой компьютер и клонировать проект Infra_sp2.

Скачать Docker на сайте https://www.docker.com/

Клонировать проект:
```
Git clone https://github.com/SergeyMMedvedev/infra_sp2.git
```

### Установка

В корне проекта находятся два файла:
* docker-compose.yaml
* Dockerfile

docker-compose.yaml - файл, предназначенный для управления взаимодействием контейнеров. В нем содержится инструкция по разворачиванию всего проекта, в том числе описание контейнеров, которые будут развернуты. 

Образ для одного из контейнеров (db) - postgres:12.4. Включает в себя все необходимое для работы базы данных проекта.

Образ для второго контейнера (web) будте создан по инструкции, указанной в файле Dockerfile.
Эта инструкция создает образ на основе базового слоя python:3.8.5. и включает установку всех необходимых зависимостей для работы проекта api_yamdb.
При запуске контейнера выполнится команда, запускающая wsgi-сервер Gunicorn.

Для запуска сборки необходимо перейтив корневую директорию проекта и выполнить 
```
$ docker-compose up
```

Проект будет развернуты два контейнера (db и web).
Посмотреть информацию о состоянии которых можно с помощью команды:

```
$ docker container ls
```

Далее необходимо скопировать CONTAINER ID контейнера web (полное название infra_sp2_web)
и выполинть команду для входа в контейнер:
 
```
$ docker exec -it <CONTAINER ID> bash
```

Будет осуществлен вход в изолированный контейнер со своей операционной системой, интерпритатором и файлами проекта api_yamdb.

Теперь необходимо выполнить миграции:
```
$ python manage.py migrate
```

Для наполнения базы данных тестовыми данными необходимо выполнить:
```
$ python3 manage.py shell
>>> from django.contrib.contenttypes.models import ContentType
>>> ContentType.objects.all().delete()
>>> quit()

$ python manage.py dumpdata > fixtures.json
```

Для создания суперпользователя:
```
$ python manage.py createsuperuser
```

## Проверка работоспособности

Теперь можно обращаться к API проекта api_yamdb:

* http://127.0.0.1:8000/api/v1/auth/token/
* http://127.0.0.1:8000/api/v1/users/
* http://127.0.0.1:8000/api/v1/categories/
* http://127.0.0.1:8000/api/v1/genres/
* http://127.0.0.1:8000/api/v1/titles/
* http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/
* http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/
* http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/

Подробнее о методах и структурах запросов в см. в проекте api_yamdb.

Для изменения содержания базы данных монжо воспользоваться админкой Django:
* http://localhost:8000/admin/


## Автор

* **Сергей Медведев** -  [SergeyMMedvedev](https://github.com/SergeyMMedvedev)
