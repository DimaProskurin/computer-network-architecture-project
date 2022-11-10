[![CI](https://github.com/DimaProskurin/computer-network-architecture-project/actions/workflows/main.yml/badge.svg)](https://github.com/DimaProskurin/computer-network-architecture-project/actions/workflows/main.yml) [![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)


# Календарь

Данный проект реализован в качестве зачетного проекта по курсу "Архитектура компьютерных сетей" в МФТИ

Проскурин Дмитрий (гр. М05-116б)

## Как запустить
```shell
./start.sh
./stop.sh
```

## Прогон тестов
```shell
./run_tests.sh
```


## Использование и API спецификация

**! Поскольку для авторизации пользователя используются cookies, то чтобы не пробрасывать токен авторизации и айди сессии в запросе, предлагается использовать утилиту Postman, которая делает это автоматически, для нижеперечисленных запросов** 

### Создание пользователя
  * endpoint: `/api/create/user`
  * POST HTTP
  * параметры пользователя должны быть указаны в теле запроса в формате JSON
```shell
curl --request POST 'http://51.250.5.79:8000/api/create/user' \
--data-raw '{
    "first_name": "John",
    "last_name": "Doe",
    "email": "johndoe@gmail.com",
    "username": "johndoe",
    "password": "mysecretpassword"
}'
```

### Авторизация
  * endpoint: `/accounts/login`
  * POST HTTP
  * логи и пароль пользователя должны быть указаны в теле запроса в формате JSON
```shell
curl --request POST 'http://51.250.5.79:8000/accounts/login' \
--data-raw '{
    "username": "johndoe",
    "password": "mysecretpassword"
}'
```

### Выход из-под пользователя
  * endpoint: `/accounts/logout`
  * GET HTTP
```shell
curl --request GET 'http://51.250.5.79:8000/accounts/logout'
```

### Создание события
  * endpoint: `api/create/event`
  * POST HTTP
  * параметры события должны быть указаны в теле запроса в формате JSON
```shell
curl --request POST '51.250.5.79:8000/api/create/event' \
--data-raw '{
    "title": "Пример события",
    "description": "Это поле является опциональным",
    "start": "2022-11-10T08:00:00",
    "end": "2022-11-10T10:00:00",
    "is_recurring": "True",
    "repeats": ["daily"],
    "invited_emails": ["guest@gmail.com"]
}'
```

### Информация о пользователе
  * endpoint `api/info/user/<int:user_id>`
  * GET HTTP
```shell
curl --request GET '51.250.5.79:8000/api/info/user/1'
```

### Показать детальную информацию о событии
  * endpoint: `api/info/event/<int:event_id>`
  * GET HTTP
  * Если событие частное для данного пользователя, то будет показана только частичная информация о событии
```shell
curl --request GET '51.250.5.79:8000/api/info/event/2'
```

### Принять или отклонить приглашение на событие
  * endpoint `api/update/invite/<int:invite_id>`
  * PUT HTTP
  * Новый статус для приглашения (ACCEPTED / REJECTED) нужно указать в качестве параметра `status` в запросе
```shell
curl --request PUT '51.250.5.79:8000/api/update/invite/2?status=REJECTED'
```

### Посмотреть список приглашений
  * endpoint `api/info/invites`
  * GET HTTP
  * Фильтр-параметр `status` определяет тип приглашений, которые необходимо показать. По умолчанию возвращаются все приглашения
```shell
curl --request GET '51.250.5.79:8000/api/info/invites'
curl --request GET '51.250.5.79:8000/api/info/invites?status=PENDING'
```

### Показать все события (по типу расписания) пользователя для указанного промежутка времени
  * endpoint `api/info/user/<int:user_id>/events`
  * GET HTTP
  * Параметры `from` и `till` должны быть указаны
```shell
curl --request GET '51.250.5.79:8000/api/info/user/1/events?from=2022-11-09T00:00:00&till=2022-11-13T00:00:00'
```

### Найти первый свободный временной промежуток для группы людей для создания события
  * endpoint `api/timetable/free_time_slot`
  * GET HTTP
  * Параметры `user_ids` и `duration` должны быть указаны 
```shell
curl --request GET '51.250.5.79:8000/api/timetable/free_time_slot?user_ids=1,2,3&duration=1:00:00'
```
