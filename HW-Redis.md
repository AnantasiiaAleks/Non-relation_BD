**Задание 1**
Напишите последовательность команд для Redis:
Создайте ключ index со значением “index precalculated content”.
Проверьте, есть ли ключ index в БД.
Узнайте, сколько ещё времени будет существовать ключ index.
Отмените запланированное удаление ключа index.

`set index "index precalculated content" ex 360`
OK

`exists index`
(integer) 1

`ttl index`
(integer) 336

`persist index`
(integer) 1


**Задание 2**
Напишите последовательность команд для Redis:
Создайте в Redis структуру данных с ключом ratings для хранения следующих значений рейтингов технологий: mysql — 10, postgresql — 20, mongodb — 30, redis — 40.
По этому же ключу увеличьте значение рейтинга mysql на 15.
Удалите из структуры элемент с максимальным значением.
Выведите место в рейтинге для mysql.

`zadd ratings 10 mysql`
(integer) 1
`zadd ratings 20 postgresql`
(integer) 1
`zadd ratings 30 mongodb`
(integer) 1
`zadd ratings 40 redis`
(integer) 1

`zincrby ratings 15 mysql`
"25"

`zpopmax ratings`
"redis"
"40"

`zscore ratings mysql`
"25"

`zrange ratings 0 5 withscores`
1) "postgresql"
2) "20"
3) "mysql"
4) "25"
5) "mongodb"
6) "30"

`zrange ratings 0 5`
1) "postgresql"
2) "mysql"
3) "mongodb"

**Задание 3**
Напишите две команды для СУБД Redis:
Подпишитесь на все события, опубликованные на каналах, начинающихся с events.
Опубликуйте сообщение на канале events101 с текстом “Hello there”.

`psubscribe events*`

`publish events101 "Hello there"`

**Задание 4**
Сохраните в Redis функцию, которая принимает ключ и значение и сохраняет под указанным ключом квадратный корень от значения.

`script load "redis.call('set', KEYS[1], ARGV[1]^2)"`
"50db1877696acc7867436309aac1547f87944c15"
`evalsha 50db1877696acc7867436309aac1547f87944c15 1 1 5`
(nil)
`get 1`
"25"
`evalsha 50db1877696acc7867436309aac1547f87944c15 1 2 3`
(nil)
`get 2`
"9"
`evalsha 50db1877696acc7867436309aac1547f87944c15 1 3 8`
(nil)
`get 3`
"64"

