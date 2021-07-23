##Flask service

####1.Blueprint와 Jinja Template
####2. 게시판을 위한 CRUD 설계 및 제작
####3.Authentication 이란?
####4. 로그인 기능 구현 

###수강 목표
##### Flask의 기본기능을 이해한다.
Blueprint와 Jinja Template를 사용해 Flask 서버를 다룰 수 있다.
##### Data를 다룰 수 있다.
CRUD의 개념을 알고, Flask 서버에서 CRUD의 기능을 작성할 수 있습니다.
#####세션, 쿠키, 로킹에 대해서 이해한다.
세션과 쿠키를 이해할 수 있고, 로그인에 적용할 수 있습니다. 또한, 서버의 로킹에 대해 이해하고 사용할 수 있습니다

###1.Blueprint와 Jinja Template

####blueprint란?
Blueprint를 사용해서 길어진 코드를 모듈화해 주어 수정 개발과 유지 보수에 용이하게 코드를 관리할 수 있습니다

#####blueprint를 사용하지 않았을 때
<u>Flask.py</u>
```python
from flask import Flask, jsonify
app = Flask(__name__)

@app.route("/", methods=["GET"])
def home_route():
    return jsonify("home")

@app.route("/", methods=['GET'])
def first_route():
    return jsonify("first page")

@app.route("/second",methods=["GET"])
def second_route():
    return jsonify("second page")

@app.route("third",methods=["GET"]
def third_route():
    return jsonify("third page")

if __name__ == :"__main__":
    app.run(debug=True)
```

##### blueprint를 사용할 때
<u>app.py</u>
```python
from flask import Flask
from first_api import bp

app= Falsk(__name__)
app.register_blueprint(bp)

if __name__ == '__main__'
    app.run(debug = True)
```

<u>first_api.py</u>
```python
from flask import Blueprint, jsonify
bp = Blueprint('bp',__name__)

@bp.route("/first",methods=['GET'])
def first_route():
    return jsonify('first page')

@bp.route("/second",methods=['GET'])
def second_route():
    return jsonify("second apge")
```

#### Jinja2란?
Jinja2는 Python에서 가장 많이 사용되는 템플릿입니다.
 서버에서 받아온 데이터를 효과적으로 보여주고 비교적 간략한 표현으로 데이터를 가공할 수 있습니다.

#### Jinja2 Template에서 데이터 넘겨주기 - 단일 변수

<u>app.py</u>
```python
@app.route("/")
def elice():
    return render_template(
        'index.html',
        data = 'elice'
    )
```
<u>index.html</u>
```html
<html>
    <head>
        <title>jinja example</title>
    </head>
    <body>
        {{ data }}
    </body>
    </html>
```

#### Jinja2 Template에서 데이터 넘겨주기 - list

<u>app.py</u>
```python
@app.route("/")
def elice():
    my_list=[1,2,3,4]
    return render_template(
        'index.html',
        data = my_list
    )
```
<u>index.html</u>
```html
<html>
    <head>
        <title>jinja example</title>
    </head>
    <body>
        {{ data }}
        {{ for d in data }}
            {{ d }}
        {% endfor %}
    </body>
    </html>
```

#### Jinja2 Template에서 데이터 넘겨주기 - dictionary

<u>app.py</u>
```python
@app.route("/")
def elice():
    my_data=['name' : 'elice']
    return render_template(
        'index.html',
        data = my_data
    )
```
<u>index.html</u>
```html
<html>
    <head>
        <title>jinja example</title>
    </head>
    <body>
        {{ data.get('name') }}
    </body>
    </html>
```

###2. 게시판을 위한 CRUD 설계 및 제작

#### API 동작 원리

클라이언트 --(http request)--> api <--(http response)-- server


#### CRUD란?
> Create  -> 데이터의 생성
Read  -> 데이터 조회
Update -> 데이터 수정
Delete -> 데이터 삭제
===> API 설계

#### 게시판 만들기

 |CRUD|HTTP Method|DB 명령어|
|----|-----------|------|
|Create|POST|INSERT|
|Read|GET|SELECT|
|Update|PUT, PATCH|UPDATE|
|Delete|DELETE|DELETE|

### 3.Authentication 이란?

#### Authentication vs Authorization

Authentication 이란 사용자가 누구인지 확인하는 절차를 말합니다. 
→ 회원가입하고 로그인하는 과정

### 4. 로그인 기능 구현

#### 쿠키
클라이언트에 저장되는 키. 값이 들어있는 데이터
- 사용자가 따로 요청하지 않아도, Request 시에 자동으로 서버에 전송.

#### 세션
쿠키를 기반으로 하지만 서버 측에서 관리하는 데이터
클라이언트에 고유 ID를 부여하고 클라이언트에 알맞은 서비스를 제공합니다. 서버에서 관리하기 때문에 보안이 쿠키보다 우수합니다.

#### 로그인 예제
```python
user_id = request.form['user_id']
user_pw = request.form['user_pw']
user = {'user_id':'elice', 'user_pw':'1234'}

if user is not None:
    if user_id == user['user_id'] and user_pw == user['user_pw']:
        session['login'] = user.id
        
```