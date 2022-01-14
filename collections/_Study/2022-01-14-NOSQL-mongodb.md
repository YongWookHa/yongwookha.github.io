---
layout: post
title: MongoDB, Not Only SQL
subtitle: PyMongo, CRUD
tags: [DEVELOP, STUDY]
cover-img: assets/img/mongodb.png
comments: true
---

RDBMS가 꽉 잡고 있는 DB시장에서 최근 그 존재감을 점점 더 키워가고 있는 NOSQL. 빅데이터에서 자주 쓰일 뿐만 아니라 MLOps에서 자주 언급되는 녀석인데, 대학생때 프로젝트에서 써본 이후로 한번도 제대로 들여다보지 않았던 것 같다. 이번 기회에 한번 mongoDB에 대해 다시 한번 알아보고 정리하고자 한다. 특히 PyMongo 위주로.  

# 🍃 MongoDB

MongoDB는 NOSQL DBMS 중에서 가장 높은 인지도를 보유하고 있다. 

> MongoDB is a document database designed for ease for application development and scailing.
> MongoDB는 개발과 확장을 쉽게 하기 위해 만들어진 document 기반의 DB다.

Mongo DB는 스키마가 엄격하지 않아 데이터 구조가 자유롭고 속도가 빠르며 수평 확장성이 뛰어나다. 지금은 장점으로 내세우고 있지만, 한때는 이런 특징들이 오히려 단점으로 생각되며 많은 사람들이 싫어?하기도 했었다.

- 생각보다 Schema는 필요한 것이었다. Schema를 안쓰는게 오히려 데이터의 Consistency를 훼손함
- 생각보다 Non-relational 데이터를 다룰 일이 많이 없고 MongoDB로 Relational 데이터를 다룰 때 너무 불편함
- 생각보다 확장성이 필요한 경우가 잘 없다. 수백GB 단위의 데이터도 single node에서 문제 없는 경우가 있다.

하지만 빅데이터의 세상이 도래하고 자유로운 데이터 구조, 쏟아지는 Non-relational 데이터가 single node에서 감당하기 어려워지면서 

MySQL과 같은 RDBMS에 익숙한 우리에게, 다음 그림은 Mongo DB의 구조를 쉽게 이해할 수 있도록 잘 설명한다.  
![](https://www.dropbox.com/s/7khpatrk1357zvg/mongodb-layer.jpg?raw=1)

MongoDB팀은 DBMS 뿐만 아니라 DB 내용을 GUI로 탐색하거나 Visualize할 수 있는 GUI 서비스인 [Charts](https://docs.mongodb.com/charts/)와 [Compass](https://docs.mongodb.com/compass/current/)도 제공하고 있다.

# 📗 Mongo DB Reference

MongoDB를 조작하는 전용 shell인 mongosh shell의 [methods](https://docs.mongodb.com/mongodb-shell/reference/methods/)와 [data type](https://docs.mongodb.com/mongodb-shell/reference/data-types/) 그리고 [option](https://docs.mongodb.com/mongodb-shell/reference/options/)은 공식 문서에서 확인 가능하다. 어차피 다 외우지 못하니, 있다는걸 어렴풋이 알고 있기만 하면 필요할 때 찾아서 이용하면 된다.


Python base의 어플리케이션을 개발하는 경우 Official MongoDB driver인 [PyMongo](https://pymongo.readthedocs.io/en/stable/index.html)를 이용할 수 있다. 이번 포스트에서는 PyMongo의 사용법을 간단히 정리해본다. 본 포스트에서 정리하는 각 method들은 다양한 parameters를 가지고 있다. 전부 다루지 못하므로, 필요에 의해 사용할 때 Official documents를 꼭 확인하자.

_참조 1: [official tutorial](https://pymongo.readthedocs.io/en/stable/tutorial.html)_  
_참조 2: [official document - pymongo.collection](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection)_  

---

<br/> 

# 🐍 PyMongo

## 📥 Install & Connect  

```bash
# install
> pip install pymongo
```

```python
from pymongo import MongoClient

host = 'mongodb://localhost:27017/'
client = MongoClient(host)

db = client.database_name  # client.database-name는 당연히 안된다
db = client['database-name']  # db 이름이 'database-name'일때는 dictionary 처럼

collection = db.collection_name
collection = db.['collection-name']  # 마찬가지
```

## 📄 Documents

RDBMS에서 `row`에 해당하는 Documents는 `JSON-style`이다. Python에서는 Document를 Dictionary로 표현할 수 있다. Dictionary 안에는 `list`, `datetime` 등의 object도 들어갈 수 있다.

```python
import datetime

post_document = {"author": "Mike",
                 "text": "My first blog post!",
                 "tags": ["mongodb", "python", "pymongo"],
                 "date": datetime.datetime.utcnow()}

```

이러한 Document 데이터 단위를 collection에 저장한다. 본 포스트에서는 pymongo를 이용하여 collection에 document를 CRUD하는 방법에 대해 다룬다.

> _**C**reate, **R**ead, **U**pdate, **D**elete_

---

## Create

Collection에 document를 넣는 방법은 `insert_one`과 `insert_many`가 있다. 직관적으로 하나 혹은 다수의 document를 입력하는데 각각 이용되며 method의 결과는 `pymongo.results`의 `Result` class들 중 하나의 instance로 return된다. 

Insert에 앞서 Collection을 굳이 따로 만들지 않아도 된다. 첫번째 document가 insert되는 시점에 아래의 `collection = db.<collection_name>` 라인에서 설정한 `<collection_name>`이름의 collection이 자동으로 생성된다.

```python
collection = db.collection_name

# 하나만 넣을 때
result = collection.insert_one(post_document)
result.iserted_id  # ObjectId('...')
```
[insert_one](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.insert_one)는 [InsertOneResult](https://pymongo.readthedocs.io/en/stable/api/pymongo/results.html#pymongo.results.InsertOneResult)의 instance를 return 한다.

```python
# 여럿을 넣을 때
documents = [{"author": "Mike",
              "text": "Another post!",
              "tags": ["bulk", "insert"],
              "date": datetime.datetime(2009, 11, 12, 11, 14)},
             {"author": "Eliot",
              "title": "MongoDB is fun",
              "text": "and pretty easy too!",
              "date": datetime.datetime(2009, 11, 10, 10, 45)}]
result = collection.insert_many(documents)
result.inserted_ids  # [ObjectId('...'), ObjectId('...')]
```

Document를 insert할 때, `_id`를 별도 입력하지 않는다면 mongoDB가 자동으로 `_id`를 생성해준다. `_id`는 `int`나 `string`이 **아니라** [`ObjectId`](https://pymongo.readthedocs.io/en/stable/api/bson/objectid.html#bson.objectid.ObjectId) instance이다.

마찬가지로 [insert_many](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.insert_many)의 return은 [InsertManyResult](https://pymongo.readthedocs.io/en/stable/api/pymongo/results.html#pymongo.results.InsertManyResult)의 instance다

---

## Read

Pymongo에서는 **R**ead를 위해 `find` method를 이용한다.

- find  
    조건(filter) 검색에서 match되는 모든 결과를 이용하려면 [`find()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection)를 사용한다. 결과는 [`Cursor`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection) instance.  
    ```python
    for match_result in collection.find({"hello": "world"}):
        # do something with match_result
    ```  
- find_one  
    [`find_one()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.find_one)는 가장 먼저 match되는 하나의 document를 리턴한다. filter 결과가 하나일거라는 확신이 있을 때 사용이 권장된다.

`find*` prefix의 다른 method들은 **U**pdate에 쓰인다.

---

## Update

update는 기존 문서의 field를 "수정"하고, replace는 기존 문서를 "대체"한다. 

- [`find_one_and_replace()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.find_one_and_update)  
    `find`로 찾은 문서 하나를 replace한다. method의 return 값은 document. matched document가 없으면 `none` return.
    ```python
    for doc in collection.find({}):
        print(doc)
    # {'x': 1, '_id': 0}
    # {'x': 1, '_id': 1}
    # {'x': 1, '_id': 2}

    collection.find_one_and_replace({'x': 1}, {'y': 1})  # {'x': 1, '_id': 0}
    
    for doc in collection.find({}):
        print(doc)
    # {'y': 1, '_id': 0}
    # {'x': 1, '_id': 1}
    # {'x': 1, '_id': 2}
    ```
- [`replace_one()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.replace_one)  
    find_one_and_replace와 거의 동일하지만, matched document가 없는 경우, 새로운 document를 생성하는 `upsert` 옵션이 있다. Document를 client에 load하지 않기 때문에 약간 더 빠르다. return 값은 [`UpdateResult`](https://pymongo.readthedocs.io/en/stable/api/pymongo/results.html#pymongo.results.UpdateResult).
    ```python
    for doc in collection.find({}):
        print(doc)
    # {'x': 1, '_id': 0}
    # {'x': 1, '_id': 1}
    # {'x': 1, '_id': 2}
    
    res = collection.replace_one({'x': 1}, {'y': 1})
    res.matched_count  # 1
    res.modified_count  # 1
    
    for doc in collection.find({}):
        print(doc)
    # {'y': 1, '_id': 0}
    # {'x': 1, '_id': 1}
    # {'x': 1, '_id': 2}

    res = collection.replace_one({'x': 2}, {'x': 1}, upsert=True)
    res.matched_count  # 0
    res.modified_count  # 0

    for doc in collection.find({}):
        print(doc)
    # {'y': 1, '_id': 0}
    # {'x': 1, '_id': 1}
    # {'x': 1, '_id': 2}
    # {'x': 2, '_id': ObjectId('...')} <- 새로 생성
    ```
- [`find_one_and_update()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.find_one_and_update)  
    matched document가 없으면 `none` return.   
    ```python
    collection.find_one_and_update({'_id': 665},
                                {'$inc': {'count': 1}, '$set': {'done': True}})
    # {'_id': 665, 'done': False, 'count': 25}}  # update 이전의 document임에 유의
    ```
- [`update_one()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.update_one)  
    method return 값은 [`UpdateResult`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection).
    ```python
    res = collection.update_one({'x': 1}, {'$inc': {'x': 3}})
    res.matched_count  # 1
    res.modified_count  # 1
    ```
- [`update_many()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.update_many)  
    matched documents 모두 update. 사용법은 `update_one()`과 유사

---

## Delete

`find_one_and_delete()`, `delete_one()`, `delete_many()`를 이용해서 document를 삭제할 수 있다. `find_one_and_delete()`는 삭제된 document를 return하고, `delete_one()`와 `delete_many()`는 [DeleteResult](https://pymongo.readthedocs.io/en/stable/api/pymongo/results.html#pymongo.results.DeleteResult) instance를 return.

```python
collection.find_one_and_delete({'x': 1})  # {'x': 1, '_id': ObjectId('...')}
collection.delete_one({'x': 1})  # DeleteResult('...')  
collection.delete_many({'x': 1})  # DeleteResult('...')
```

---

## Counting

collection에 `count_documents()` method를 이용해서 특정 조건에 대한 documents 개수를 알아낼 수 있다.

```python
collection.count_documents({})
collection.count_documents({"author": "Mike"})
```

---

## Queries

위에 언급된 method들의 첫번째 parameter로 들어가는 filter를 작성할 때는 mongoDB에서 제공하는 별도의 [Query](https://pymongo.readthedocs.io/en/stable/tutorial.html#range-queries)를 이용할 수 있다. filter에 [Special operator](https://docs.mongodb.com/manual/reference/operator/)들을 이용해서 범위 검색등에 이용하거나 [`sort()`](https://pymongo.readthedocs.io/en/stable/api/pymongo/cursor.html#pymongo.cursor.Cursor.sort)를 통해 결과를 정렬할 수 있다.

```python
d = datetime.datetime(2009, 11, 12, 12)
for matched_document in collection.find({"date": {"$lt": d}}).sort("author"):
    # do something with matched_document
```

보다 활용성을 높이기 위해서는 Query 사용법 숙지가 가장 중요할 것 같다.

---

## indexing

query 속도를 높이기 위해 default로 제공되는 `_id` 이외에 query에 사용할 수 있는 [unique index](https://docs.mongodb.com/manual/core/index-unique/)를 만들 수 있다. 

```python
result = db.profiles.create_index([('user_id', pymongo.ASCENDING)],
                                   unique=True)
sorted(list(db.profiles.index_information()))  #  ['_id_', 'user_id_1']
```

이렇게 만든 index는 중복될 수 없다. 중복되는 index를 가진 document를 insert 시도하면 `DuplicateKeyError`를 반환한다.


