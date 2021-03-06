# Материал по теме PostgreSQL

### Коротко о фишках PostgreSQL
1) Транзакционный DDL.
Когда ты большие миграции заливаешь на проде, в которых помимо данных добавляются или удаляются колонки, то если где-то в mysql возникнет ошибка в запросе, то получается что половина миграции применена, а половина нет. База поломана, админы 2 дня и 2 ночи восстанавливают все из бекапов. С PG такого нету.
Это первый реальный кейс, кто сталкивался с этим, знают какая это боль. В mysql плодят костыли, навроде постепенной накатки миграций по 1 запросу и т.д.

2) Частичные индексы, индексы по выражению. Если у вас в таблице куча записей, например 20млн., из которых надо быстро выбирать скажем по самым свежим 15к записям, то можно создать индекс только по ним например. Это будет занимать намного меньше места.
То есть не возникает проблемы как в mysql что база уже не влезает на диск и надо ее раскидывать по нескольким сервакам.

3) Работа с JSON индексированная, ну тут вроде все очевидно. Если надо сваливать куда-то неструктурированные данные и выбирать по ним быстро, то все работает транзакционно, и не надо никакой монги сбоку прикручивать.

4) FTS поиск более продвинутый чем в mysql, во многих случаях можно обойтись без sphinx/elasticsearch, что опять же сильно экономит время и ресурсы.

5) Foreign Data Wrappers - из PG можно прямо запросами ходить в Memcached, Redis, Mysql, и другие базы, если нужно какие-то запросы хитрые выбирать. На стороне кода все это не нужно реализовывать.


#### PostgreSQL UPDATE FROM SELECT
Когда необходимо обновить данные по результатам некоторого подзапроса, для этого можно воспользоваться следующей конструкцией:

```postgresql
UPDATE dummy
SET customer = subquery.customer,
    address = subquery.address,
    partn = subquery.partn
FROM (
    SELECT address_id, customer, address, partn
    FROM  /* big hairy SQL */
) AS subquery
WHERE dummy.address_id = subquery.address_id;
```


#### PostgreSQL INSERT FROM SELECT
Когда необходимо вставить данные в таблицу по результатам некоторого подзапроса, воспользуйтесь следующей конструкцией:

```postgresql
INSERT INTO current_invoices_items (item_id, invoice_id) SELECT id AS item_id, invoice_id AS invoice_id FROM items
```

#### PostgreSQL обновить счетчкик поля
У PostgreSQL счетчик автоинкремента существует отдельно от таблицы(в 10 версии можно использовать Identity columns) и при определенных условиях может отставать от значения в поле, чтобы задать счетчик равным максимальному значению поля (в данном примере id) выполните:
```SELECT setval(pg_get_serial_sequence('TABLE_NAME', 'id'), MAX(id)) FROM TABLE_NAME;```

#### PostgreSQL отключить проверку индексов
Полезно при выполнениии SQL импортов

```
SET session_replication_role = replica;
-- SQL MAGIC
SET session_replication_role = DEFAULT;
```

@todo CTE, Оконные функции, explain analyze
