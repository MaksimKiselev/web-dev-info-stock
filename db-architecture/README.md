# Архитектура БД

### Вычисление пересекающихся интервалов в линейных, и замкнутых пространствах имен

`$start` - начало интервала
`$end` - конец интервала

Выборка записей пересекающих заданный интервал в линейных пространствах имен:
```sql
SELECT * FROM table WHERE
(start >= $start AND start < $end)  /* смещение вперед */
OR
(end <= $end AND end > $start)      /* смещение назад */
OR
(start >= $start AND end <= $end)   /* поглощение и совпадение */
```

Выборка записей не пересекающих заданный интервал в линейных пространствах имен:
```sql
SELECT * FROM table WHERE start < $end AND end > $start
```

Замкнутое пространство имен - это такое пространство имен, которое, при исчерпании имён первого порядка, не переходит на новый порядок а возвращается к своему началу.

Выборка записей пересекающих заданный интервал в замкнутых пространствах имен:
```sql
SELECT * FROM table WHERE
greatest(start, $start) <= least(end, $end)
OR ((end > $start OR $end > start) and sign(start - end) <> sign($start - $end))
OR (end < start and $end < $start)
```

За основу был взян пост: https://habrahabr.ru/post/209138/

------------------------------------------------------------------
@todo Написать про блокировки
lynicidn @lynicidn 16:23
if (isset($this->versionField)) {
пфф над уии костыль
который может уии?
вы крутые
ревизия и есть смысл lock
только есть optimistick (в момент)
а есть pissimistick (до)
а есть select for update
но не для всех "Ar::getDb"


@todo написать про иерархии, nested sets, Adjacency list, closure table


https://habrahabr.ru/post/263629/
https://habrahabr.ru/post/193166/

http://gbif.blogspot.ru/2012/06/taxonomic-trees-in-postgresql.html
https://www.postgresql.org/docs/9.1/static/ltree.html
