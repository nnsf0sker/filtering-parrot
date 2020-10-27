# Filterrot

Это тестовое задание на позицию стажёра-бекендера в Avito, реализованное [Нечаевым Никитой Сергеевичем](github.com/nnsf0sker) в апреле 2020. Условия задачи находятся ниже в пункте "Задача", а здесь будет описано, как пользовать решением и что означают файлы в 
репозитории. Данная реализация получила название "Filterrot" от выражения "filterring parrot", лаконично описывающего принцип действия сервиса - повторять одну и ту же информацию, но только тем, кто спрашивает не слишком часто.

## От автора решения

Решение реализовано на Python 3.8 с использованием Falcon в качестве основного web-фреймворка. Pytest применялся для написания 
тестов, а flake8 помогал соблюдать кодстайл.

### Запуск решения

Для работы решения на компьютере должен быть установлен Docker.

    docker-compose up
    
Сервис ничего не делает, кроме фильтрации чрезмерно частых запросов (не более 100 в минуту). Причём запросы во время 
бана учитываются), т. е. входящий запрос будет обработан только в том случае, если за последние 2 минуты с данной 
подсети пришло не более 100 запросов.


### Тестирование

    pytest test.py
    
### Проверка код-стайла

    flake8 .

## Задача

Необходимо создать HTTP-сервис, способный ограничивать количество запросов (rate limit) из одной подсети IPv4. Если ограничения отсутствуют, то нужно выдавать одинаковый статический контент.

### Требования:
- язык: Go или Python
- код должен быть выложен на GitHub
- ответ должен соответствовать спецификации RFC 6585
- IP должен извлекаться из заголовка X-Forwarded-For
- подсеть: /24 (маска 255.255.255.0)
- лимит: 100 запросов в минуту
- время ожидания после ограничения: 2 минуты

**Пример**: после 20 запросов с IP 123.45.67.89 и 80 запросов с IP 123.45.67.1 сервис возвращает 429 ошибку на любой запрос с подсети 123.45.67.0/24 в течение двух последующих минут.

### Усложнения (плюс в карму за каждый пункт):
- покрытие тестами
- контейнеризация, возможность запустить с помощью `docker-compose up`
- размер префикса подсети, лимит и время ожидания можно задавать при старте сервиса
- отдельный handler для сброса лимита по префиксу
