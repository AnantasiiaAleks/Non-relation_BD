**Задание 1**

Из коллекции постов выберите документы, в которых среди топиков встречается as, идентификатор автора содержит example.ru, а score больше 100.

```
db.posts.find({"author":/.*@example.ru.*/, "topics": "as", "score":{$gt:100}})
```
{ "_id" : ObjectId("66e16254ae6fdad901b5f555"), "author" : "lprudy@example.ru", "creation_date" : ISODate("2020-06-27T00:00:00Z"), "topics" : [ "as", "very", "a", "but" ], "score" : 242, "status" : "published", "message" : "us for over with rabbit and screamed to to life her sat and i to and be and and join don fell march said majesty found if ever and their stand to than natural doing dormouse alice know size and back go twinkle alice went askance commotion caused in the she all found taking croquet dormouse but hearing the i as evidence i put the your" }

{ "_id" : ObjectId("66e16254ae6fdad901b5f55a"), "author" : "aalfred@example.ru", "creation_date" : ISODate("2021-02-03T00:00:00Z"), "topics" : [ "as", "pleasure", "hot" ], "score" : 4707, "status" : "published", "message" : "all find the a that alice therefore off yet same and and it all rather and said the said out leaves tell this nor chorus just nine blasts made s plate if to nothing her round nose to except interesting alice say become is hatter grinned this and with close that over come and that all as mushroom interrupted then first time up the it wood what procession we moment pointing thought a round you" }

{ "_id" : ObjectId("66e16254ae6fdad901b5f5a8"), "author" : "sagnesse@example.ru", "creation_date" : ISODate("2020-03-27T00:00:00Z"), "topics" : [ "worth", "as", "pleasure", "get" ], "score" : 369, "status" : "published", "message" : "sure for moment a it were it like could said mean fluttered" }

**Задание 2**

Одним запросом добавьте два документа к коллекции posts:
creation_date — текущее время, автор — skbx@example.com, topics должен быть списком из одного элемента mongodb;
creation_date — 31 декабря 2021 года, автор — skbx@example.ru.

```
db.posts.insertMany([
... {"creation_date": new Timestamp(), "author": "skdb@example.com", "topics": ["mongodb"]},
... {"creation_date": new ISODate("2021-12-31T00:00:00Z"), "author": "skdb@example.ru"}
... ])
```
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("66e74eb111c74838a41b52ec"),
		ObjectId("66e74eb111c74838a41b52ed")
	]
}

**Задание 3**

Создайте композитный индекс для коллекции users, в него войдут поля first_name и last_name. Приведите запросы: на создание индекса и на проверку, что индекс используется.

```
db.users.createIndex({first_name: 1, last_name: 1})
```
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}
```
db.users.getIndexes()
```
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "skdb.users"
	},
	{
		"v" : 1,
		"key" : {
			"first_name" : 1,
			"last_name" : 1
		},
		"name" : "first_name_1_last_name_1",
		"ns" : "skdb.users"
	}
]

**Задание 4**

Посчитайте сумму кармы по первым буквам имён пользователей для тех пользователей, у которых больше 300 визитов.
Советы и указания
Для выбора первой буквы имени используйте ключевое слово substr.

```
> db.users.aggregate(
... {$match: {"visits": {$gt: 300}}},
... {$project: {first_name: "$first_name", karma: "$karma"}},
... {$group: {_id: {$substr: ["$first_name", 0, 1]}, sum_karma: {$sum: "$karma"}}}
... )
```

{ "_id" : "Z", "sum_karma" : -82 }
{ "_id" : "T", "sum_karma" : -68 }
{ "_id" : "S", "sum_karma" : 296 }
{ "_id" : "E", "sum_karma" : 120 }
{ "_id" : "H", "sum_karma" : 79 }
{ "_id" : "R", "sum_karma" : 53 }
{ "_id" : "V", "sum_karma" : -43 }
{ "_id" : "M", "sum_karma" : 516 }
{ "_id" : "D", "sum_karma" : -39 }
{ "_id" : "L", "sum_karma" : 243 }
{ "_id" : "C", "sum_karma" : 176 }
{ "_id" : "O", "sum_karma" : 71 }
{ "_id" : "B", "sum_karma" : 323 }
{ "_id" : "P", "sum_karma" : 94 }
{ "_id" : "K", "sum_karma" : 153 }
{ "_id" : "G", "sum_karma" : 199 }
{ "_id" : "A", "sum_karma" : -28 }
{ "_id" : "J", "sum_karma" : 477 }

**Задание 5**

Создайте хранимую функцию shuffle, которая принимает один параметр — строку и возвращает строку со случайно переставленными символами. (Используйте встроенный в JavaScript метод Math.random() для сортировки символов в строке.)

db.system.js.save(
	{_id: "shuffle", value: function () {
    var a = this.split(""),
        n = a.length;
    	for(var i = n - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    	}
    	return a.join("");
		}
	}
)