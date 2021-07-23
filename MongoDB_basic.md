## MongoDB 기초

###목차

#### 1.MongoDB 체험하기

#### 2.데이터베이스계의 이단아 NoSQL

#### 3.MongoDB 소개 및 분산컴퓨팅

#### 4.JS 친화적 MongoDB

#### 5.MongoDB 활용

<br>
### 1.MongoDB 체험하기 
```
use myDatabase #db 접속
db.initialCollection.insertOne({hello: "world"}) #정보 저장
db.initialCollection.find() #정보 확인
```
### 2.데이터베이스계의 이단아 NoSQL
>질의 명령어가 SQL이 아님  
정보의 형식을 미리 정하지 않는다  
_id값이 자동 저장 되었다.

NoSQL DBMS : Not Only SQL Database Management System

### 3.MongoDB 소개 및 분산 컴퓨팅

- 현대적 설계로 고성능을 내면서 동시에 많은 기능과 안정화가 이루어진 다목적 DB
- 과거에는 하나의 DBMS로 처리하는 상황이 빈번했다
- 기존 RDBMS는 하나의 인스턴스에서 작동하는 것이 기본값
- 요즘은 처리할 데이터양이 많아서 분산컴퓨팅이 빈번하다
- 현대적 DBMS는 분산컴퓨팅을 하는 것이 기본값

##### 분산 컴퓨팅의 방식

    복제 : 복사를 하여 안정성을 높이기 위한 방식. 원본서버가 망가져도 정상 서비스가능
    샤딩 : 성능을 향상하기위한 방식. 나누어 저장하여 읽기, 쓰기 성능 향상 가능

- MongoDB는 분산컴퓨팅을 하는 것을 가정하고 쓰기 기본 설정이 되어있다!
- Write-Concern, Read-Concern 설정을 알아야 제대로 사용 가능하다!

### 4. JS 친화적 MongoDB

##### 입문자를 위한 JS 채택

    2008년 속도를 획기적으로 향상시킨 V8 엔진 등장
    2008년 Chrome의 부상
    2009년 백엔드 개발이 가능한 NodeJS 등장!

<br>

    JSON과 유사한 BSON구조로 정보를 저장
    스키마가 없는 방식

##### MongoDB가 JS를 사용해서 얻은 특징

    1.웹 개발자에게 쉬운 입문이 가능.
    2.BSON 자료형 사용
    3.내부 명령어를 JS 형식으로 사용.

### 5.MongoDB 활용
