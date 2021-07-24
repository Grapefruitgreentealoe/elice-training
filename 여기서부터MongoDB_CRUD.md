## MongoDB 기초 _ CRUD
#### 1.MongoDB의 구조 
#### 2.BSON 데이터 타입 
#### 3.도큐먼트 생성 
#### 4.도큐먼트 검색 기초 
#### 5.도큐먼트 수정 
#### 6.도큐먼트 삭제
<br>
### 1.MongoDB의 구조 
>데이터베이스
컬렉션
도큐먼트

__Json과 유사한 bson구조로 정보를 저장__

#### Pymongo소개

```python
import pymongo
connection = pymongo.MongoClient("mongodb://localhost:27017/")
#localhost : 내 pc 내부
#27017 : MongoDB 기본 포트
```

__pymnongo는 mongoDB를 사용할 수 있게 도와주는 파이썬 모듈__

##### Pymongo로 DB접속
```python
import pymongo
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection.get_database("testDB")
```
접속할 데이터베이스로 접근. 만약 데이터베이스가 없으면 **자동으로 생성한 후** 접속

##### 컬렉션에 도큐먼트 삽입하기
```python
import pymongo
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection.get_database("testDB") #데이터베이스
collection = db.get_collection("testCollection")#
collection.insert_one({ "hello": "world" })#띄어쓰기 주의. 도큐먼트 삽입
#만약 컬렉션이 없다면 자동으로 생송.
```
##### 데이터베이스, 컬렉션, 도큐먼트 확인
```python
print(connection.list_database_names())
print(db.list_collection_names())
pprint(list(collection.find()))
```

##### 결과 출력
    ['admin','config','local', 'testDB']
    ['testCollection']
    [{'_id': ObjectId('...'), "hello":"world"}]




### 2.BSON 데이터 타입 

#### BSON 이란
    BSON = Binary JSON의 의미

    JSON의 일부로써 MongoDB 도큐먼트로 데이터를 저장하기 위한 형식

#### BSON에 들어가는 자료형
|||
|-|-
Double/Integer|
String|
Object|
Array|
Boolean|
Date|
Object|
Date| ISODate("2017-10-24T05:02:46.395Z")
ObjectId|ObjectId("542c2b97 bac059 5474 108b48")
#### ObjectId의 구성
ObjectId: MongoDB에서 각 Document의 primary key의 값으로 사용된다.

**유닉스시간, 기기id, 프로세스id, 카운터**로 구성된다.
<br>

#### 정리하기

<u>BSON 구조 예시</u>
```python
{_id:ObjectId("542c2b97 bac059 5474 108b48"),
name:{first: "sue", last: "Turing" },
age: 26,
is_alive:true,
groups: ["news", "sports"],
viewTime: ISODate("2017-10-24T05:02:46.395Z")}
```

#### Pymongo에서 사용하기
<u>Python으로 표현한 자료형</u>

```python
from bson import ObjectId
from datetime import datetime

collection.insert_one({
    "_id":ObjectId("542c2b97bac0595474108b48"),
    "name":{"first": "sue", "last": "Turing" },
    "age": 26,
    is_alive":True,
    "groups": ["news", "sports"],
    "viewTime": datetime(2017, 10, 24, 5, 2, 46)})
```
<br>

### 3.도큐먼트 생성

#### 도큐먼트를 보기 좋게 출력하기
```python
from pprint import pprint
pprint({ BSON document })
```
#### 컬렉션에 도큐먼트 삽입하기
```python
import pymongo
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["testDB"]
collection = db["tedstCollection"]
collection.insert_one({})
# 이전에 잠깐 배웠을 때 insert_one으로 도큐먼트를 생성했었다.
```

<u>명령어</u>

```python
from pprint import pprint
result = collection.insert_one({ document })
print(result.inserted_id)
pprint(result.inserted_id)
```
<u>결과</u>

    1sdf01239023sdjnfsndlf234
    ObjectId('1sdf01239023sdjnfsndlf234')

<u>명령어</u>

```python
from pprint import pprint
result = collection.insert_many({ document1 },{ document2 },{ document3 }, ...)
print(result.inserted_ids)

```
<u>결과</u>

    [
    ObjectId('1sdf01239023sdjnfsndlf234'),

    ObjectId('11232kdf01239023sdjnfsndl'), ...
    ]

**=> 다수의 도큐먼트 객체를 넘긴다.**
<br>

#### 웹서버에서 도큐먼트 생성 예시
    -웹 서버
    1. 주어진 정보로 게시글을 insert_one 으로 저장
    2. 반환된 ObjectId로 해당 게시글 접속 URL을 생성
    3. URL 반환

<br>

### 4.도큐먼트 검색 기초 
##### 컬렉션에서 도큐먼트 조회
```python
import pymongo
from pprint import pprint
connection = pymongo.MongoClient
("mongodb://localhost:27017/")
db = connection["testDB"]
collection = db["testCollection"]
collection.insert_one({ "hello": "wolrd" })
print(list(collection.find()))
```

```python
result = collection.find(
    {query},
    {projection}
)
print(result)
print(list(result))
```
    <pymongo.cursor.Cursor object at 0x12931fsd0423423>
    [{ document },{ document }]

**find 명령어는 컬렉션 내에 query 조건에 맞는 다수의 도큐먼트를 검색한다.**
<br>

#### 커서란?
:커서란 **쿼리 결과에 대한 포인터**
도큐먼트의 위치정보만을 반환하여 작업을 효율적으로 만들어준다.

#### 커서에서 도큐먼트 불러오는 부분
```python
result = collection.find(
    { query }
)
print(list(result))
for document in result:
    print(document)
```
<결과>

    #list로 내용물을 한번에 불러올 수 있다.
    [{ document }, { document }, ...]
    #for문으로 내용물을 하나씩 처리할 수 있다.
    { document }
    { document }

#### 쿼리란?
:쿼리란 원하는 정보를 걸러내기 위한 깔때기이다.

#### 기본 Query
    {"field" : value, "field" : value,...}
**Query는 그 field에 맞는 value값으로 필터링한다!**
<br>

#### find문 예시
```python
user.find(
    {"username" : "karoid"}
) 
```
<결과>

    { "_id" : ObjectId("59ef45b4ddf91a3b998ee9ed"), "username" : "karoid", "password" : "1111" }

<br>

#### Projection이란?
    {"field" : boolean, "field" : boolean, ...}
**Projection은 그 field 를 보여줄지 말지를 알려준다.**

