# Архитектура БД

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
