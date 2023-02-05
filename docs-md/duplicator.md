## Типы дубликаторов

- Дубликатор 1 - дублирует 1 в 1 базу бабла (точнее выбранные сущности) в промежуточную базу в новой системе для быстрого доступа в работе дубликатора 2. По замыслу предназначен для каталогов, т.е. тех данных, которых не много и которые не будут быстро меняться. Сам код работы дубликатора 1 находится в коде дубликатора 2, т.е. они работают исключительно вместе в текущей реализации
- Дубликатор 2 - маппит данные из бабла (дубликатора 1) в новую базу. Используется также в основном для каталогов (для остальных всё собрано в дубликации заказа)
- Дубликатор заказа - дублирует заказ из бабла в новую систему (данный эндпоинт должен по замыслу дёргать бабл; **не забыть** в нём указать правильный адрес в api connector - move order). Каталожные данные берёт из базы дубликатора 1, остальные докачивает напрямую из бабла в процессе маппинга.
- Дубликатор файлов - дубликатор заказа в процесса маппинга ставит файлы в очередь на загрузку (спец таблица в базе дубликатора 1) затем данный дубликатор по крону запускает последовательное выкачивание этих файлов и добавляет актуальные ссылки в заказ, привязанный к файлу.
- Дубликатор заказа из новой системы в бабл - см в конце.

## Контроллеры

Везде требуется передать параметр адреса apiKey.

- GET /duplicate-all - запускает дубликатор в режиме скачивания всего каталога с бабла (на вход ничего не принимает, но внутри контроллера есть хардкод настроек)
- POST /duplicate-by-date - запускает дубликатор в режиме скачивания каталога с бабла, может принимать в body два опциональных параметра from и to - даты, в промежутке которых дублировать.
- POST /duplicate-order-old-to-new-single - запускает дубликацию одного заказа с бабла в новую систему (требует в body id заказа в бабле)
- POST /duplicate-order-old-to-new-all - запускает дубликацию всех заказов; может принимать в body два опциональных параметра from и to - даты, в промежутке которых дублировать заказы.
- GET /duplicate-files - скачивает файлы из бабла в новую систему, поставленные в очередь на загрузку дубликацией заказа (это ручной вызов, автоматический работает по крону)

## Дубликатор заказа из новой системы в бабл

POST ${BUBBLE_URL}/api/1.1/wf/receive-order-from-new-system
Эндпоинт бабла, куда новая система будет отправлять заказ, чтобы дублировать его в старой. Принимает данные в форматах бабла (только более читаемые названия полей). О каждом поле можно прочитать в эксель таблице полей бабла https://docs.google.com/spreadsheets/d/1OWV5waFglE9xDPNjXg30eN09WmDVE2FQu-H2l5XXxhw
Т.к. новая система не знает id сущностей бабла, то сопоставление обычно идёт по другим уникальным полям, например у юзеров это email, там где есть apiName - apiName и т.д. Фотографии отчётов передаются прямо в json в base64 формате.

DTO:

```json
{
  "apiKey": "xxx",
  "cityApiName": "Moscow",
  "clientsAddress": "test address",
  "enterpriseOrderNumber": "0001",
  "brandApiName": "hansa",
  "statusApiName": "cancelled",
  "voximplantNumber": 10000,
  "clientsPhone": "89990001122",
  "notice": "text",
  "clientsName": "Bob",
  "orderNumber": 99991,
  "legalEntityInn": "test",
  "salesmanBonusClicker": 1,
  "salesmanBonusEnterprise": 2,
  "isSaasOrder": false,
  "handymanEmail": "test@mail.ru",
  "offeredServiceCompaniesEmailList": ["test@mail.ru"],
  "serviceCompanyEmail": "test@mail.ru",
  "relatedOrderNumber": 12345,
  "scOfferAcceptedPersonsEmail": "test@mail.ru",
  "scOfferAcceptedDate": "2020-04-21T20:05:49.418Z",
  "timeslot1DateRange": ["2020-04-21T20:05:49.418Z", "2020-04-21T20:05:49.419Z"],
  "timeslot2DateRange": ["2020-04-21T20:05:49.418Z", "2020-04-21T20:05:49.419Z"],
  "timeslot3DateRange": ["2020-04-21T20:05:49.418Z", "2020-04-21T20:05:49.419Z"],
  "timeslot4DateRange": ["2020-04-21T20:05:49.418Z", "2020-04-21T20:05:49.419Z"],
  "timeslot5DateRange": ["2020-04-21T20:05:49.418Z", "2020-04-21T20:05:49.419Z"],
  "installationDateFrom": "2020-04-21T20:05:49.418Z",
  "installationDateTo": "2020-04-21T20:05:49.419Z",
  "timeslot1Visibility": true,
  "timeslot2Visibility": true,
  "timeslot3Visibility": true,
  "timeslot4Visibility": true,
  "timeslot5Visibility": true,
  "deliveryDateFrom": "2020-04-21T20:05:49.418Z",
  "deliveryDateTo": "2020-04-21T20:05:49.419Z",
  "salesmanCode": "1234",
  "departmentCode": "1234",
  "dateNextCall": "2020-04-21T20:05:49.419Z",
  "cancelReasonApiName": "test",
  "cancelComment": "test",
  "managersEmails": ["test@mail.ru", "test2@mail.ru"],
  "deliveryContacts": "test",
  "reviewBall": 5,
  "reviewText": "Тестовый отзыв",
  "comments": [
    {
      "text": "comment 1",
      "authorsEmail": "test@mail.ru"
    },
    {
      "text": "comment 2",
      "authorsEmail": "test@mail.ru"
    }
  ],
  "history": [
    {
      "text": "history 1",
      "createdDate": "2020-04-21T20:05:49.418Z",
      "authorsEmail": "test@mail.ru"
    },
    {
      "text": "history 2",
      "createdDate": "2021-04-21T20:05:49.418Z",
      "authorsEmail": "test@mail.ru"
    }
  ],
  "services": [
    {
      "clientsTotalPrice": 123,
      "isPaid": false,
      "scTotalRevenue": 123,
      "clickerTotalRevenue": 123,
      "enterpriseTotalRevenue": 123,
      "amount": 2,
      "description": "test",
      "included": "test",
      "notIncluded": "test",
      "name": "test",
      "clientsSinglePrice": 123,
      "scSingleRevenue": 123,
      "clickerSingleRevenue": 123,
      "enterpriseSingleRevenue": 123,
      "paidAtRetailer": 123,
      "paidByCard": 123,
      "paidWithCash": 123,
      "paidWithCashOwnRegister": 123
    }
  ],
  "goodsToInstall": [
    {
      "productId": "test1",
      "price": 100,
      "title": "Name 1",
      "amount": 1,
      "url": "http://test1"
    },
    {
      "productId": "test2",
      "price": 200,
      "title": "Name 2",
      "amount": 2,
      "url": "http://test2"
    }
  ],
  "tasks": [
    {
      "state": "finished",
      "typeApiName": "new_order",
      "deadlineDate": "2022-04-21T20:05:49.418Z",
      "closedDate": "2021-04-21T20:05:49.418Z",
      "description": "test",
      "closedUserEmail": "test@mail.ru",
      "recipientUserEmail": "test@mail.ru",
      "createdUserEmail": "test@mail.ru"
    },
    {
      "state": "active",
      "typeApiName": "new_order",
      "deadlineDate": "2023-04-22T20:05:49.418Z",
      "description": "test2",
      "closedUserEmail": "test@mail.ru",
      "recipientUserEmail": "test@mail.ru",
      "createdUserEmail": "test@mail.ru"
    },
    {
      "state": "planned",
      "typeApiName": "new_order",
      "deadlineDate": "2023-04-22T20:05:49.418Z",
      "description": "test3",
      "closedUserEmail": "test@mail.ru",
      "recipientUserEmail": "test@mail.ru",
      "createdUserEmail": "test@mail.ru"
    }
  ],
  "reports": [
    {
      "type": "work",
      "foto": "data:image/png;base64,..."
    }
  ]
}
```

Следующие данные заказа не будут восстановлены на бабле:

search_text
recent_order_history_custom_order_history
last_modified_date
sales_check_list_file
date_for_first_call_date
duplicator_field_text
date_for_group_by_date
recent_reminder_date
last_non_mc_comment_custom_comment
last_non_mc_comment_date_date
time_setting_list_custom_time

И данные услуги в заказе:

services_custom_price
json_text
serv_price_text_text
html_for_bill_text
service_catalog_id_text

# Последовательность при переносе системы

#### Залить дубликатор онлайн с финальными настройками

- Установить все переменные среды
- Установить переменную BUBBLE_TOTAL_ITEMS_LIMIT_DEFAULT (в коде) как Number.POSITIVE_INFINITY;
- В контроллере duplicateByDateController передать нужные сущность joinsForUpdate, entitiesForPostMapping (список будет позже, когда будет реализация заказа, но если проще нужны все сущности, кроме тех, что относятся к заказу), dropOldTables должен быть false. Список entitiesForUpdate (каталоложные сущности) должен быть следующим (нужны все сущности, кроме тех, что относятся к заказу):
  BUBBLE_ENTITY.city,
  BUBBLE_ENTITY.user,
  BUBBLE_ENTITY.retailerSalesman,
  BUBBLE_ENTITY.price,
  BUBBLE_ENTITY.serviceCompany,
  BUBBLE_ENTITY.enterprise,
  BUBBLE_ENTITY.legalEntity,
  BUBBLE_ENTITY.brand,
  BUBBLE_ENTITY.enterpriseDepartment,
  BUBBLE_ENTITY.serviceCategoryGroup,
  BUBBLE_ENTITY.serviceCategory,
  BUBBLE_ENTITY.service,

#### Задублировать каталоги

Ориентировочное время около часа-двух. Эндпоинт POST /duplicate-by-date, тело:

```json
{
  "from": "date string",
  "to": "date string"
}
```

From не требуется, to требуется (указать текущуюю дату, сохранить её где-нибудь), для того, чтобы потом при завершении переезда ввести вновь данную дату, только уже во from, и задублировать созданные с момента первой дубликации сущности.

#### В микросервисах активировать поправки работы на две системы

Виджеты должны будут уметь проверять к какой системе относится заказ и далее паботать с ней

- Паблик апи (getServicesToShop, ...) ???
- Вокс (сейчас ходит напрямую в бабл за данными) ???
- Виджет модальный ???
- Виджет iframe ???
- SMM сервис (не забыть также реализовать отправку ИЗ новой системы данных в смм)

#### Ручной перенос заказов в двух системах

Доступны два эндпоинта, один в бабле, другой в дубликаторе, чтобы через них переносить заказ в другую систему.  
В бабле активировать кнопку переноса (на странице зказа есть специальный попап, его вызывает скрытая кнопка) и прописать адрес этого апи дубликатора

#### Финальный переезд, и перенос всех заказов

Ориентировочное время - несколько ночей? [в процессе]

- Выполлнить первый шаг с датой "to", только теперь передать её во "from" (хотя лучше конечно, чтобы изначально не было расхождения в каталогах)

Дальше запустить дубликатор заказов POST /duplicate-order-old-to-new-all. DTO:

```json
{
  "from": "date string",
  "to": "date string",
  "includeStatuses": ["status_api_name"],
  "duplicateFiles": true
}
```

- Передать в "to" текущую дату, т.к. к моменту завершения дублирования в старую систему могут придти новые заказы и надо будет задублировать заказы ещё раз, только теперь дату "to" передать во "from". Также надо передать "duplicateFiles": true, чтобы по завершении дублирования информации заказов также запустить выкачивание файлов, иначе все файлы из заказов будут добавлены в очередь и их скачивание будет запущено по крону в 23 часа. Ещё надо передать массив "includeStatuses", в который включить все статусы активных заказов [offered_for_consultation, offered_for_assembly, offered_for_call, waiting_for_call, waiting_for_consultation, new_time_is_set, new_time_is_not_set, consultation, waiting, service_company_assigned, work_started, blank]
- Повторить предыдущий шаг только уже с датой "from" как описано.
- Затем в свободное время (другие ночи) докопировать остальные, неактивные заказы [cancelled, received_review, finished_successfully, finished_unsuccessfully]

#### Перенести микросервисы

Если после тестов переезд прошел успешно, но окончательно перенести микросервисы (убрать все проверки принадлежности заказа к старой системе):

- Вокс
- Паблик апи
- Виджет модальный
- Виджет iframe
