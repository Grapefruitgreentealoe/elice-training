## Flask 프로젝트 1 – 익명게시판 만들기
### 목차
#### 1.프로젝트 구상하기 – 익명 게시판
#### 2.데이터베이스 구성하기(ORM)
#### 3.화면 만들기(Ajax -> dbModel)
#### 4.기능 만들기 1 – 글 쓰기(Ajax -> dbModel)
#### 5.기능 만들기 2 – 글 읽기(Ajax -> dbModel)
#### 6.기능 만들기 3 - 좋아요 누르기(Ajax -> dbModel)
API -> Rest API로

### 목표
- **프로젝트를 기획 할 수 있다.**
하나의 프로젝트를 설정하고, 분석하고 만들 수 있다.
- **프론트엔드와 백엔드 간의 통신을 만들 수 있다.**
프론트엔드와 백엔드를 구분하고, 각각 요청/응답을 처리할 수 있다.
- **Flask에서 ORM을 활용해 데이터를 다룰 수 있다.**
Flask-SQLAlchemy를 사용해서 ORM 모델을 다루고 데이터를
저장/업데이트/읽기를 할 수 있다.

### 1.프로젝트 구상하기 – 익명 게시판

#### 어떤 프로젝트를 만들까?

**글 작성**
→ Database 생성
→ API 생성
• **랜덤 아이디 생성**
→ 화면에서 랜덤 문자열 생성하기
→ Ajax로 요청 시 데이터 서버로 전달하기
• **게시된 글별로 좋아요 적용**
→ 게시글 별로 'like'를 적용하기


#### 프로젝트 구조 만들기

1. **app.py**
app.py는 서버를 실행하고, DB connection의 기본 정보들을 가지고 있습니다.
Flask 서버가 실행되기 위한 기초적인 정보 및 기능들을 선언합니다.

2. **templates 폴더**
우리가 사용할 HTML 파일들을 모아놓는 곳입니다. render_template는 이곳에 있는 HTML 파일들을 읽어서 화면에 출력해 주는 역할을 합니다.

3. **static 폴더**
이미지, CSS, JS와 같은 외부 라이브러리들을 추가해 두는 곳입니다. JinjaTemplate에서 url_for('static',filename=' ') 으로 접근 할 수 있습니다.

>    서버를 실행할 app.py
    Blueprint로 API 주소를 관리할 api.py
    모델을 선언할 models.py
    DB connection을 관리할 db_connect.py
    HTML 파일을 관리할 templates 폴더
    CSS와 JS를 저장할 static 폴더

#### API 생성하기
```python
@app.route("/", methods=["GET"])
def my_router():
    return render_template("index.html")
    # return render_template("index.html", data_list = data)
    # return jsonify(data)
```
### 2. 데이터베이스 구성하기

#### 글(POST) 모델 만들기
**필요한 정보**
- 게시글 ID
- 작성자 ID
- 게시글 내용
- 작성된 시간
- 게시글 좋아요 수

<u>Model.py</u>

```python
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

db = SQLAlchemy()

class Post(db.Model):
    __tablename__ = 'secret_post'
    id = db.Column(db.Integer, primary_key=True,
    nullable=False,autoincrement=True)
    author = db.Column(db.String(256),nullable=False)
    content = db.Column(db.Text(), nullable=False)
    like = db.Column(db.Integer, default=0)
    created_at = db.Column(db.DateTime,
    default=datetime.utcnow)
```

<br>

### 3.화면 만들기(Ajax -> dbModel)
#### 화면 작성하기
화면은 HTML, CSS(bootstrap), Javascript(JQuery Ajax)로 이루어져 있다는 것을 기억하시나요?
익명 게시판 화면에 대한 설명을 듣고 같이 만들어 보도록 하겠습니다

**주제 : 모두가 이용 할 수 있는 익명게시판**
- 배경화면 - 색상, 정렬
- 글 작성 - 글 전송, ID 생성
- 작성된 글 읽기 - 글 읽어 오기, 정렬
- 작성된 글 좋아요 기능 - 글 좋아요 Update

<br>

**[실습]화면 작성하기 및 사용할 Dummy API 만들기**
프로젝트에서 사용할 화면과 Dummy API를 만들려고 합니다.

화면은 크게 두 가지로 나뉩니다.

- Post를 작성하는 공간
- 작성된 Post들을 보여주는 공간

화면은 Bootstrap과 jQuery를 사용합니다.
Bootstrap과 jQuery를 사용하기 위해서는 import를 해주어야 합니다.

Bootstrap, jQuery는 모두 static 폴더 안에 저장이 되어, 아래처럼 사용 가능합니다.

```html
<script src="{{ url_for('static', filename='bootstrap.bundle.min.js') }}" > </script>
    <script src="{{ url_for('static', filename='jquery.js') }}" > </script>
    <link href="{{url_for('static',filename='bootstrap.min.css')}
```

### 4.기능 만들기 1 – 글 쓰기(Ajax -> dbModel)

글쓰기 카드
- jQuery를 사용해서 글 작성 공간 값 가져오기
```$("#content").val()```
- Javascript의 Math.random() 함수를 이용해서 문자
랜덤 생성하기
```Math.random().toString(20).substr(2, 10)```
- Ajax를 사용해서 Flask 서버의 API와 연동하기
→Flask API 생성하기(API url : /create/post )
→Flask API와 Ajax 연결하기


#### 데이터베이스 저장하기 예제
```
content = request.form['content']
post = Post()
post.content = content
db.session.add(post)
db.session.commit()
```

- request로 받아온 데이터를 변수(content)에 저장합니다.
- 저장할 모델을 선언 (Post()) 합니다.
- 모델 안에 데이터 넣어줍니다.
- 세션에 추가 / 저장합니다.

### 5.기능 만들기 2 – 글 읽기(Ajax -> dbModel)

**글 읽기 카드**
- flask의 jinja2 template 사용하기
```
{% for data in data_list %}
    {{ data }}
{% endfor %}
```
- flask API의 render_template에서
데이터베이스에 있는 값 넘겨주기
```
render_template("index.html",data_list=data)
```
#### 데이터 읽어오기 예제
```python
data = db.session.query(모델이름).filter(조건).all() # 데이터값 읽어 오기 -> list
render_template('html이름', data_list = data)
```
```
{{ data_list}}
{% for d in data_list %}
    {{ d }}
{% endfor %}
```

- query() 안에 내가 원하는 데이터를 가지고 있는 모델을 작성합니다.
- 가져온 data를 render_template()에 전달합니다.
- html에서 jinja template 문법으로 데이터 읽어옵니다.

[실습]글 읽기 API 작성하기
```python
from flask import Blueprint, render_template, jsonify, request
from models import Post
from db_connect import db

board = Blueprint('board',__name__)

# 작성한 게시글을 볼 수 있도록 함수를 완성하세요.
@board.route("/")
def home():
    data = Post.query.order_by(Post.created_at.desc()).all()
    return render_template('index.html', data = data)
    
    
@board.route("/post",methods=["POST"])
def create_post():
    content = request.form['content']
    author = request.form['author']
    post = Post(author,content)
    db.session.add(post)
    db.session.commit()
    return jsonify({'result':'success'})
    
    
@board.route("/like",methods=["PATCH"])
def update_like():
    return jsonfiy("good")
```

### 6.기능 만들기 3 - 좋아요 누르기(Ajax -> dbModel)
**글 읽기 카드 - 좋아요**
- 좋아요 버튼에 함수 만들어주기
```updatePostLike(id) → 해당 게시글의 id```
- Database Update 기능 만들기
```db.session.commit()```

#### 데이터베이스 업데이트 예제
```
data = db.session.query(모델 이름).filter(조건).first()
data.like = data.like+1 # 데이터값 수정
db.session.commit()
```
- query() 안에 내가 원하는 데이터를 가지고 있는 모델을 작성합니다.
- filter() 안에 읽어올 데이터의 조건들을 작성합니다.
- data를 불러왔다면, 가져온 데이터에 있는 값을 수정합니다.
- db.session에 이미 데이터를 불러왔기 때문에 add() 없이 commit() 합니다.

**[실습]좋아요 기능 구현하기**
```
function postLike(id) {
            $.ajax({
                url:'/like',
                type:'patch',
                data:{
                    'id':id},
                success:function(res){
                    let result = res['result']
                    if(result == "success"){
                        window.location.reload()}
                    else{
                    alert("저장 오류!")
                    }
                        
                }
                }
                
            })
        }
```
