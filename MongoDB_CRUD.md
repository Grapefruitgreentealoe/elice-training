## MongoDB 기초 _ CRUD
#### 1.MongoDB의 구조 
#### 2.BSON 데이터 타입 
#### 3.도큐먼트 생성 
#### 4.도큐먼트 검색 기초 
#### 5.도큐먼트 수정 
#### 6.도큐먼트 삭제

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
collection = db.get_collection("testCollection")#콜렉션
collection.insert_one({ "hello": "world" })#띄어쓰기 주의. 도큐먼트 삽입
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
    [{'_id':}]




