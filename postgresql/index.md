# Материал по теме PostgreSQL

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


@todo CTE, Оконные функции, explain analyze, установка и настройка
