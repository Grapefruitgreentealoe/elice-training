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
from flask import Flask #Flask 패키지에서 Flask 를 import 
app = Flask(__name__)

@app.route("/")#@app.rout()는 서버에 접속할 수 있는 url 을 만들어 줍니다.
def elice():
    return "hello elice"
#@app.route("/")아래의 def elice()라는 함수는 app.route()의 url에서 실행할 함수이다.
if name __name__ == "__main__":
    app.run()
#파일 이름이 main일 때만 app.run()이 실행되도록 합니다.

```

