---
title: Calldron API Reference

language_tabs:
  - shell
  - ruby
  - python
  - php

<!-- includes: -->
  <!-- - errors -->

search: true
---

# 1. Введение

**Входная точка:** https://api.calldron.ru/v1/

**Текущая актуальная версия:** v1.

**Тип: REST API.** Используется 4 вида HTTP-запросов — GET, POST, PUT и DELETE.

**Механизм авторизации:** API-Key Basic Auth (API-ключ авторизации получается в профиле пользователя https://calldron.ru/users/edit).

**Формат данных:** JSON (application/json). Необходимо указывать заголовком Content-Type.

<aside class="notice">
  Внимание! Не выкладывайте свой API-ключ авторизации в открытый доступ. Данный ключ должен надёжно храниться на вашем сервере. Вы не должны выводить его в HTML или JavaScript. В противном случае, злоумышленники смогут получить доступ к API от имени вашего пользователя.
</aside>

<aside class="notice">
  Рекомендация —  При малейших подозрения о утечке вашего API-ключа в открытый доступ или в руки посторонних лиц, вам следует сбросить API-ключ в профиле вашего личного кабинета — https://calldron.ru/users/edit.
</aside>


# 2. Базовая информация
**Примеры выполнения запросов к Calldron API при помощи различных технологий**

Здесь приведены примеры запросов к Calldron API при помощи утилиты cURL, а также языков PHP, Ruby и Python
HTTP авторизация в утилите cURL происходит указанием следующего аргумента:
curl -u username:password
В примере запроса c curl — user - API-ключ, а password параметр должен быть пуст:
curl -u <ВАШ_API_КЛЮЧ>:

>Sample request

```ruby
require "uri"
require "net/https"

# Ваш API-ключ авторизации
api_key = "gnLZYz0FexchC6RFPyP0PBb9n26cecQh"

# URL запроса
uri = URI.parse("https://api.calldron.ru/v1/")

# Создаём объекты запроса
http = Net::HTTP.new(uri.host, uri.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_PEER
request = Net::HTTP::Get.new(uri.request_uri)

# Авторизация
request.basic_auth(api_key, "")

# Заголовки запроса
request.headers["Content-Type"] = "application/json"

# Выполняем запрос
response = http.request(request)

# Ваша обработка ответа
puts response.body
```

```python
import httplib2

# Ваш API-ключ авторизации
api_key = "gnLZYz0FexchC6RFPyP0PBb9n26cecQh"

# URL запроса
url = "https://api.calldron.ru/v1/"

# Создаём объект запроса
http = httplib2.Http()

# Авторизация
http.add_credentials(api_key, '')

# Заголовки запроса
headers = {'Content-type': 'application/json'}

# Выполняем запрос
response, content = http.request(url, 'GET', headers=headers)

# Обработка ответа
print(content)
```

```shell
curl -X GET "https://api.calldron.ru/v1/" -u gnLZYz0FexchC6RFPyP0PBb9n26cecQh:  -v
```

```php
<?php
// URL запроса
$ch = curl_init("https://api.сalldron.ru/v1/");

// Ваш API-ключ авторизации
$api_key = "gnLZYz0FexchC6RFPyP0PBb9n26cecQh";

// Авторизация
curl_setopt($ch, CURLOPT_USERPWD, $api_key . ":");

// Заголовки запроса
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
	"Content-Type: application/json"
));

// Выполняем запрос
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$result = curl_exec($ch);

// Закрываем соедиениние
curl_close($ch);

/* Обработка ответа */
echo $result;
?>
```

# 3. Пагинация — разбиение списка ресурсов по страницам

Некоторые из запросов Calldron API выводят массив ресурсов. В таком случае, ответ на запрос будет разбиваться на страницы исходя из значения следующих параметров, которые также можно передать входными параметрами запроса:

* limit — ограничение на количество ресурсов в ответе. По умолчанию limit равен 20. Может принимать целочисленное значение от 1 до 100 включительно.
* offset — смещение на указанное количество ресурсов от начала. Допустим, вы указали limit = 5. Чтобы отобразить первые 5 элементов, offset должен быть равен 0. Если вы желаете отобразить вторые 5 элементов, offset должен быть равен 5. Чтобы отобразить третьи 5 элементов, offset должен быть равен 10. И так далее. По умолчанию offset равен 0. Может принимать любое положительное целочисленное значение.

Также каждый ответ на запрос списка ресурсов содержит два поля, помогающие узнать суммарное количество элементов по запросу, общее количество страниц, а также количество элементов в данном ответе:

* count — количество элементов в данном ответе.
* total — общее количество элементов, соответствующее запросу.

# 4. Методы HTTP в Calldron API
Calldron API использует 4 HTTP метода в различных случаях:
* GET — для получения каких-либо данных. Это может быть список ресурсов, единичный ресурс и т.п.
* POST — для создания нового ресурса, переключения состояния ресурса или вызова специфичных запросов.
* PUT — обновление ресурса.
* DELETE — удаление ресурса.

В подробном описание каждого запроса указан необходимый метод HTTP.

# 5. Версии Calldron API
Используемая версия API должна содержаться в URL каждого запроса сразу после домена:
https://api.calldron.ru/v1/

Текущая наиболее актуальная версия - v1. В будущем, при наличии серьёзных изменений в работе, будут создаваться новые версии. Тем не менее, текущая версия также будет функционировать. Поэтому вы можете не беспокоиться за то, что ваша интеграция с Calldron внезапно перестанет работать из-за изменений в API.

# 6. Подробное описание всех запросов к Calldron API

Ниже представлено подробное описание всех возможных запросов к Calldron API. Каждое описание содержит тип HTTP-запроса, URL, входные параметры(если они возможны), примеры ответов, ошибки(специфичные для данного запроса).

**Внимание!** Документация в процессе написания. Возможно неполное описание некоторых запросов.


## Batch — пачки прозвона / конфигурации прозвона.

###  Index — список batch’ей.

> Index HTTP Запрос:
  <br>
  GET https://api.calldron.ru/v1/batches

> Ответ:

```json
[
  {
    "id": "66",
    "name": "API",
    "description": "",
    "attempts": "3",
    "call_delay": "30",
    "callback": "79999802696",
    "from": "",
    "confirmation": "timeout",
    "status": "fresh",
    "type": "instant"
  },
   ...
]
```

### Create — создание батча

 Type может иметь значения batch, instant, callback
 Confirmation может иметь значения dtmf, timeout

> Create HTTP Запрос:
<br>
POST https://api.calldron.ru/v1/batches

```json
[
  {
  "batch": {
            "Name": "name",
            "Time_zone": "Moscow",
            "Time_from": "2000-01-01T08:00:00+02:00",
            "Time_to": "2000-01-01T19:00:00+02:00",
            "Attempts": "2",
            "call_delay": "30",
            "callback": "+7491231212",
            "From": "+7491231213",
            "intro": "Base64 encoded file (mp3, wav)",
            "intro_filename": "intro_filename",
            "type": "callback",
            "confirmation": "timeout",
            "timeout_before_intro_callback": "1"
        }
  }
]
```

> Ответ:

```json
  {
    "id": "123"
  }
```

### Update — изменение батча

 Type может иметь значения batch, instant, callback
 Confirmation может иметь значения dtmf, timeout

> Update HTTP Запрос:
<br>
PUT https://api.calldron.ru/v1/batches/:id

```json
[
  {
  "batch": {
            "Name": "name",
            "Time_zone": "Moscow",
            "Time_from": "2000-01-01T08:00:00+02:00",
            "Time_to": "2000-01-01T19:00:00+02:00",
            "Attempts": "2",
            "call_delay": "30",
            "callback": "+7491231212",
            "From": "+7491231213",
            "intro": "Base64 encoded file (mp3, wav)",
            "intro_filename": "intro_filename",
            "type": "callback",
            "confirmation": "timeout",
            "timeout_before_intro_callback": "1"
        }
  }
]
```

> Ответ:

```json
  {
    "id": "123"
  }
```

### Destroy — удаление батча

> Destroy HTTP Запрос:
<br>
DELETE https://api.calldron.ru/v1/batches/:id

> Ответ:

```json
  {
    "id": "123"
  }
```

## Calls — звонки.

###  Index — список batch’ей.

> Index HTTP Запрос:
<br>
GET https://api.calldron.ru/v1/batches

> Ответ:

```json
[
  {
    "id": "332",
    "phone_number": "+79997778855",
    "duration": "60.0",
    "status": "unknown",
    "result": null,
    "final": true,
    "dialed_at": "2016-11-22 15:36:39",
    "ended_at": "2016-11-22 15:37:39",
    "record_url": "https://api.calldron.ru/v1/batches/11/calls/332/record.mp3"
  },
  ...
]
```

### Create — создание звонка

Номер телефона должен иметь формат +7491231212

> Create HTTP Запрос:
<br>
POST https://api.calldron.ru/v1/batches/66/calls

```json
[
  {
    "call": {
    		"phone_number": "+79997778855"
    	}
  }
]
```

> Ответ:

```json
  {
  }
```


### Show — просмотр звонка

> Show HTTP Запрос:
<br>
GET https://api.calldron.ru/v1/batches/66/calls/332

> Ответ:

```json
  {
    "id": "332",
    "phone_number": "+79997778855",
    "duration": "60.0",
    "status": "unknown",
    "result": null,
    "final": true,
    "dialed_at": "2016-11-22 15:36:39",
    "ended_at": "2016-11-22 15:37:39",
    "record_url": "https://api.calldron.ru/v1/batches/11/calls/332/record.mp3"
  }
```
