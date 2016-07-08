# Материал по теме PostgreSQL

#### PostgreSQL UPDATE FROM SELECT
Бывают случаи, когда данные необходимо обновить по результатам некоторого запроса, для этого можно воспользоваться примерно следующей конструкцией:

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


@todo CTE, Оконные функции, explain analyze, установка и настройка
