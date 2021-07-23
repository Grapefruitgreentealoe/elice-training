##Flask Framework란 무엇인가
- 하나의 결과물을 만들기 위해서 제공하는 '틀'
- 미리 작성되어 있는 함수(라이브러리) 이상의 기능을 제공
- Flask는 Python 을 사용해서 웹 서버를 만들 수 있게 도와주는 Web Framework

#### flask framework의 장점
- 나만의 서버를 쉽게 작성할 수 있다.
- 간단한 코드로 빠르게 실행할 수 있다.
- 원하는 기능을 유연하게 확장하기 편리하다.

### Flask 웹서버 만들기

1. Flask Simple Web server 만들기
``` python
from flask import Flask #Flask 패키지에서 Flask를 import 
app = Flask(__name__)

@app.route("/")#@app.rout()는 서버에 접속할 수 있는 url 을 만들어 줍니다.
def elice():
    return "hello elice"
#@app.route("/") 아래의 def elice()라는 함수는 app.route()의 url에서 실행할 함수이다.
if name __name__ == "__main__":
    app.run()
#파일 이름이 main 일 때만 app.run()이 실행되도록 합니다.

```

## URL 연결하고 데이터를 화면에 나타내기
- app.route()와 이어져 있는 함수를 사용해서 HTML과 JSON 형식의 데이터 전달하기

####1. JSON 형식의 데이터 나타내기
``` python
from flask import Flask , jsonify #Flask 패키지에서 Flask와 jsonify를 import 
app = Flask(__name__)

@app.route("/") 
def elice_json():
    my_data = {"name":"elice"}
    return jsonify(my_data) #jsonify라는 함수는 jsonify 안에 있는 내용을 화면에 전달한다.
# @app.route("/") 아래의 def elice_json()라는 함수는 app.route()의 url에서 실행할 함수이고, {"name":"elice}라는 데이터를 화면에 전달해 준다.
if name __name__ == "__main__":
    app.run()
```


#### 2. HTML 형식의 데이터 나타내기 - 파일 준비하기
- html을 화면에 전달하기 위해서는 html 파일이 필요.
- html 파일은 templates라는 폴더 아래에 넣어주어야 한다.
- templates 폴더에 html 파일을 넣어 놓으면 Flask가 자동으로 찾아서 연결해 준다.

#### 3. templates 준비하기
- Flask를 통해 가져올 index.html을 templates 폴더 아래에 생성.

#### 4. app.py 준비하기
```python

from flask import Flask, render_template #Flask 패키지에서 Flask와 render_template를 import 한다.
app = Flask(__name__) 
#render_template라는 함수는 templates라는 폴더 안의 html 파일을 불러와 주는 역할을 한다.

@app.route("/") 
def elice_html():
    return render_template("index.html")
#templates 폴더 내의 html 파일의 이름과 render_template() 안의 이름이 같아야 화면에 잘 전달할 수 있다.
if __name__ == "__main__":
    app.run()

```

#### 5. 여러 가지 url 연결하기 

```python
from flask import *
app = Flask(__name__)

@app.route("/")
def elice():
    return jsonify("home")

@app.route("/admin")
def elice_admin():
    return jsonify("admin page")

@app.route("/student")
def elice_student():
    return jsonify("student Page")

@app.route("/student/<name>")
def elice_user(name):
    user = {"name":name}
    return jsonify(user) 

if__name__== "__main__":
    app.run()
```

---
## 실습
- JSON 데이터 보내기
서버가 요청에 응답하는 방식은 크게 두 가지라고 설명했었습니다.
한 가지는 HTML 페이지를 돌려주는 응답이고, 다른 하나는 data를 돌려주는 응답이었습니다.

## Rest API란?
- HTTP URL을 통해 데이터의 자원을 표현하고 HTTP method를 통해서 다루는 방법을 의미.
- Database, 이미지, 텍스트 등의 다양한 데이터에 적용할 수 있다.

- HTTP URI(Uniform Resource Identifier)를 통해 자원을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것.
 
- 다양한 클라이언트가 생겨남에 따라서 REST API가 필요합니다.(다양한 곳에서 통신해야 하기 때문)
- REST API는 메시지가 의도하는 바를 URL에서 나타내므로, 쉽게 기능을 파악할 수 있습니다.
- HTTP 표준 프로토콜에 따르는 플랫폼에서 사용 가능합니다.
- 서버와 클라이언트의 구분을 명확하게 할 수 있습니다.
- REST API의 표준이 존재하지 않는 단점이 있습니다.

## HTTP Method 사용하기

#### HTTP Method의 개념 -GET/ POST
 - GET 과 POST는 HTTP Method 중 하나이다.
 - GET은 데이터를 URL 뒤에? 와 함께 사용하고
 - POST는 특정 양식에 데이터를 넣어 전송하는 방법이다.

 #### GET 방식의 예시
 > http://사이트의_주소? 데이터=123

> GET의 예시-> url 뒤에 물음표를 붙여서 데이터를 서버에 전송 
 네이버 영화 페이지->https://movie.naver.com/movie/bi/mi/basic.nhn 
 네이버 영화의 해리 포터와 마법사의 돌 페이지-> https://movie.naver.com/movie/bi/mi/basic.nhn? code=30688

#### POST 방식의 예시
> POST의 예시 -> 일정한 양식에 담아 데이터를 숨겨서 서버에 전송
네이버 로그인 페이지 url -> https://nid.naver.com/nidlogin.login
네이버 로그인 데이터 전송 url -> https://nid.naver.com/nidlogin.login

#### 로그인 예제
```python
user_id = request.form['user_id']
user_pw = request.form['user_pw']
user = {'user_id': 'elice', 'user_pw': '1234'}
if user is not None:
    if user_id == user['user_id'] and user_pw == user['user_pw']:
        session['login'] = user_id
        return jsonify({"result":"success"})
        else:
            return jsonify({"result":"fail"})
```
> - request로 받아온 로그인 정보(user_id, user_pw)를 변수에 저장합니다.
> - 데이터베이스에 user_id와 같은 데이터가 있는지 찾아옵니다.
> - 입력된 user_pw와 저장된 패스워드가 같은 크지 체크합니다.

#### 로그아웃 예제

> session['login'] = None

- 로그아웃을 할 때는 정보를 받을 필요가 없습니다.
- 현재 저장된 session의 데이터를 비워주면 기능이 완료됩니다.


### 5. 로깅

#### 로깅이란
    로킹은 프로그램이 작동할 때 발생하는 이벤트를 추적하는 행위를 말한다.
    프로그램의 문제들을 파악하고 유지 보수하는 데 사용. 로킹을 통해 발생한 에러를 추적할 수 있다.

#### 로깅-level
DEBUG < INFO < WARNING < ERROR < CRITICAL
기본 로거 레벨 세팅은 warning이기 때문에 설정 없이 Info, debug를 출력할 수 없습니다.

> DEBUG: 상세한 정보 
INFO: 일반적인 정보 
WARNING: 예상치 못하거나 가까운 미래에 발생할 문제 
ERROR: 에러 로그. 심각한 문제 
CRITICAL:  프로그램 자체가 실행되지 않을 수 있는 문제

```python
import logging
if __name__ == '__main__':
    logger.info("hello elice!")
```

```python
import logging
if __name__ == '__main__':
    logger = logging.getLogger()
    logger.setlevel(logging.DEBUG)
    logger.info("hello elice")
```

#### flask에서 로깅

flask에서 기본적으로 logging을 제공한다.
```python
import logging
if __name__ == '__main__':
    app.logger.info("test") 
    app.logger.debug("debug test") 
    app.logger.error("error test") 
    app.run()
```
