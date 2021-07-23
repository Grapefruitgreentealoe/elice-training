## SQL Alchemy

### 목차
#### 1. SQL Alchemy 와 ORM
#### 2. DB와 Model
#### 3. Query

### 수강 목표
##### 1.ORM을 이해할 수 있다. 
SQLAlchemy를 사용한 ORM을 이해하고 사용할 수 있습니다.
##### 2.ORM을 사용한 모델을 작성할 수 있다. 
이해한 ORM을 바탕으로 우리가 원하는 데이터베이스모델을 만들 수 있습니다.
##### 3.ORM의 쿼리를 이해할 수 있다. 
 ORM의 쿼리를 이해하고 상황과 조건에 맞게 사용할 수 있습니다.





### 1. SQL Alchemy 와 ORM

##### ORM이란
데이터 베이스에 객체를 통해 접근하는 방법은 <u>ORM(Object Relational Mapping, 객체 관계 매핑)</u>이라고 한다.
SQL질의어가 없어도 데이터베이스를 다루 수 있도록 도와줍니다.

##### 테이블에 대한 sql쿼리문과 orm
|name|age|
|-|-|
|엘리스|15
도도|16
체셔|17

|sql 쿼리문|INSERT INTO 엘리스(멤버,나이) VALUES('여왕','18')
|-|-|
ORM|memeber1= Member() <br> member1.name = '여왕' <br>member1.age='18' <br> db.session.add(member1)<br>db.session.conmmit()


##### ORM의 장점
- DB에 대한 큰 고민 없이, 데이터베이스를 코드로 다룰 수 있다.
- 테이블 구조가 변경될 때, ORM 모델만 수정하면 된다.
- 코드로 작성하기 때문에 쿼리를 직관적으로 이해할 수 있다.

##### SQL Alchemy란?
:파이썬 ORM 라이브러리 -> 파이썬 코드에서 Database와 연결하기 위해 사용할 수 있는 라이브러리

### 2. DB와 Model

##### DB 와 Model
 <u>models.py</u>

    memeber1 = Member(name='여왕', age = '18')
    db.session.add(member1)
- 위의 코드에서 Member는 파이썬 클래스이며, DB의 Member테이블과 매핑하여 사용.
- DB의 테이블과 매핑되는 클래스를 모델이라고 한다.

##### ORM Model
<u>models.py</u>

```python
class Members(db.Model): #클래스 상속
    __tablename__ = 'my_user'
    id = db.Column(db.Integer, primary_key=True, nullable=False)
    name = db.Column(db.String(20), nullable=False)
    age = db.Column(db.Integer,nullable=False)
```

- \_\_tablename__ 은 데이터베이스 테이블을 명시해줍니다.
- id, name, age 는 데이터베이스 테이블의 컬럼을 명시해 줍니다.
- 해당하는 데이터베이스를 다룰 때, Memebers클래스로 접근한다.

> members = Members()

### 3. Query
##### CRUD예제 - Create:데이터 저장

```python
class Members(db.Model): #클래스 상속
    __tablename__ = 'my_user'
    id = db.Column(db.Integer, primary_key=True, nullable=False)
    name = db.Column(db.String(20), nullable=False)
    age = db.Column(db.Integer,nullable=False)
```
    members = Members()
    ->
    members.id = 1
    members.name = 'elice'
    members.age = 20
    ->
    db.session.add(members)
    db.session.commit()

##### CRUD예제 - Read:데이터 읽기
    members = db.session.query(Members).filter(Members.name == 
    'elice').all() #List
    #db.session.query(Members).first() ->1개의 Model만 
__db.session.query(Members).filter(~~~) == Members.query.filter__
##### CRUD예제 - Update : 데이터 수정
    db.session.query(Members).filter(Members.name == 'elice').all() 
    ->
    members[0].name = 'oliver'
    ->
    db.session.commit()
##### CRUD예제 - Delete:데이터 삭제 
    me = db.session.query(Members).filter(Members.name == 'elice)
    .first()
    ->
    db.session.delete(me)
    ->
    db.session.commit()

#### Query 사용법
##### Query 사용법 - equl

```python
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name == 'Elice')
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - not equl

```python
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name != 'Elice')
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - like

```python
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name.like('Elice')
    return " ".join(i.name for i in member_list)
```
> __('%Elice%') ->문자열 내용 포함__
__(f%{name}%) ->변수 내용 포함__

##### Query 사용법 - in

```python
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name.in_
    (['Elice','Dodo'])
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - not in

```python
@app.route('/')
def list():
    member_list = Member.query.filter(~Member.name.in_
    (['Elice','Dodo'])
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - is null

```python
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name == None)
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - is not null

```python
@app.route('/')
def list():
    member_list = Member.query.filter(Member.name != None)
    return " ".join(i.name for i in member_list)
```
##### Query 사용법 - and

```python
from sqlalchemy import and_
@app.route('/')
def list():
    member_list = Member.query.filter(and_(Member.name == 'Elice', Memeber.age == '15'))
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - or
```python
from sqlalchemy import or_
@app.route('/')
def list():
    member_list = Member.query.filter(or_(Member.name == 'Elice', Memeber.age == '15'))
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - order by

```python
@app.route('/')
def list():
    member_list = Member.query.order_by(Member.age.desc())
    return " ".join(i.name for i in member_list)
```
##### Query 사용법 - limit

```python
@app.route('/')
def list(limit_num = 5):
    if limit_num is None:
        limit_num = 5
    member_list = Memeber.query.order_by(Member.age.desc()).limit(limit_num)
    return " ".join(i.name for i in member_list)
```

##### Query 사용법 - count

```python
@app.route('/')
def list(limit_num = 5):
    member_list = Memeber.query.order_by(Member.age.desc().count()
    return str(mnember_list)
```

__Query 문을 행 결과로 반환된 tuple 수를 반환__









