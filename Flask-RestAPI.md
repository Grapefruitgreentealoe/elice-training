## Rest API

### 목차
#### 1.REST API
#### 2.flask-restful
#### 3.Ajax란–동기/비동기
#### 4.테스팅

<br>

### 수강목표
####Flask를 사용한 REST API를 만들 수 있다. 
개념을 간단히 배웠던 REST API에 대해서 다시 짚어보고, REST API를 구현할 수 있습니다. 
####비동기통신을 이해하고 사용할 수 있다. 
**Ajax**를 사용한 비동기통신을 이해하고 사용할 수 있습니다. 
####API 테스팅 툴을 이해하고 사용할 수 있다. 
테스팅 코드를 작성해서 내가 만든 API를 테스트할 수 있습니다

<br>

### 1.REST API
REST는 자원을 이름으로 구분하여 **해당 자원의 상태를 주고받는 모든 것**을 의미합니다. 한마디로 정리하면, 자원(resource)의 표현(representation)에 의한 상태 전달입니다.

#### REST 의 구성과 특징
- **자원** – URI에 표현이 되어야 함→무엇을 서버에 요청할 것인지 
- **행위** – HTTP Method → 어떻게(어떤 방법을) 요청할 것인지 
- **표현** – Representations → API만 보고 무엇을 요청할 것인지 알 수 있도록 

REST 서버는 API 제공, 클라이언트는 사용자 인증 등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로 간 의존성이 줄어들게 됩니다. 

REST는 상태 정보를 따로 저장하고 관리하지 않습니다. 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 됩니다. 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해집니다.

#### REST API 디자인 가이드
1. URI 자원을 표현해야함
: REST API 로 요청할 때에, URI 에서 어떤 데이터를 요청하는지 표현이 되어야 함.
2. 자원에 대한 행위는 HTTP Method로 나타내야 함

#### REST API 디자인
1. 리소스 요청은 주로 동사보다는 명사를 사용
2. 슬래시(/)는 계층 관계를 나타냄
3. 파일 확장자는 URI에 포함하기 않음
3. 긴 URI 에서는 밑줄보다는 하이픈을 사용.

#### HTTP 응답 상태 코드
|상태코드|내용|
|-|-|
|200|정상적으로 수행|
|201|성공적으로리소스생성|
|400|클라이언트의 요청이 부적절할 때|
|401|인증되지 않은 상태에서 리소스 요청|
|404|응답하고 싶지 않은 리소스, 혹은 없는 리소스를 요청|
|500|서버에 문제가 생겼을 때|

### 2.flask-restful

#### flask-restful
:Flask는 return 값을 jsonify로 주어서 **RESTful API** 를 만들 수 있다.
하지만, 좀 더 RESTful 에 맞게 서버를 만들 수 있는  라이브러리가 있다.
\<rest>
```python
@app.route('/first',methods=['GET'])
def elice_route():
    return jsonify('elice GET')

@app.route('/first',methods=['POST'])
def elice_route():
    return jsonify('elice POST')
```

\<restful>
```python
Class First(Resource):
    def get(self):
        return 'eliceGET', 200
    
    def post(self):
        return 'elicePOST', 200
```
<details>
<summary>퀴즈</summary>


Q1.RESTful한 API를 만드는 법으로 옳은 것을 고르세요.

- URI에 자원과 id 외 정보가 들어가지 않게 한다.
- 모든 요청(request)를 POST로 받는다.
- 응답에 대한 메타데이터를 Body에 포함 한다.
- API의 Endpoint가 여러 개 이다.
<details>
<summary>해설</summary>
<div markdown="1">       

모든 요청을 POST로 받는게 아닌 GET, POST, PUT, DELETE 메소드를 활용해야 합니다.
응답에 대한 메타데이터는 Body에 포함 시키지 않고 최대한 HTTP 헤더에 포함되어야 합니다.
각 API에서 resource는 URL로 표시가 되며, 유일한 식별자로써 표현되야 하기 때문에 Endpoint가 하나여야 합니다.
</div>
</details>

<br>

Q2. REST API 설계 규칙으로 맞지 않는 것은?
- URI 마지막 문자로 슬래시('/')를 포함하지 않는다.
- 슬래시 구분자('/')는 계층 관계를 나타내는데 사용한다.
- 파일확장자는 URI에 포함하지 않는다.
- URI 경로에는 대문자가 적합하다.

<details>
<summary>해설</summary>
<div markdown="1">       

RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문에 URI 경로에는 소문자가 적합합니다.
</div>
</details>
</details>


<details>
<summary>실습</summary>
</details>

### 3.Ajax란–동기/비동기
#### 동기? 비동기?
:동기 – 앞의 작업이 끝나지 않는다면 다음 작업을 할 수 없다.
비동기 – 앞의 작업 상태와 상관없이 다음 동작을 수행할 수 있다

#### 비동기 처리
- Fetch(url에 요청. request, response 사용. Explorer 지원 x),
- Axios(Vue, React에서 많이 사용. axios.get(), axios.post),
- ajax(제이쿼리 사용.)

#### ajax 의 형태

```javascript
$.ajax({
    url: 'url 주소'
    type: '메서드',
    data : {},
    success : function(res){}
    console.log(res)
    }

})
```
- url은 Ajax로 요청을 보낼 주소를 적는 곳
- type은 Ajax로 보낼 방법을 적는 곳
- data는 Ajax 통신으로 보낼 데이터를 적는
곳
- success는 통신이 성공한 후 실행되는
함수를 적는 곳

#### Ajax 호출 예시
```javascript
function ajaxTest(){
$.ajax({
url: 'http://openapi.seoul.go.kr:8088/5a4947666d62613936345971506a6d/json/SearchParkInfoService/1/10',
type: 'get',
data: {},
success : function(res){
console.log(res)
}
})
}
///HTML
<button onclick = ajaxTest()> 호출 </button> 
```


#### Ajax - Flask
<u>html</u>
```html
$.ajax({
url: 'ajax_test',
type: 'GET',
data: {},
success : function(res){
console.log(res)
//{ “name” : “elice”}
}
})
```
<flask>
```python
from flask import Flask, jsonify
app = Flask(__name__)
@app.route("/ajax_test")
def elice_jsonify():
dict_data = {
“name” : “elice”
}
return jsonify(dict_data)
```

<details>
<summary>실습코드 = Ajax 구현</summary>

    from flask import Flask, render_template, jsonify, request

    app = Flask(__name__)

    board = []

    @app.route('/')
    def index():
        return render_template('index.html', rows=board)


    @app.route('/ajax', methods=['POST'])
    def ajax():
        # json 형태로 데이터를 저장하세요.
        data = request.get_json()
        board.append(data)
        # 지시사항을 참고하여 데이터를 반환하세요.
        return jsonify(result="success", result2= data)

</details>

<details>
<summary>실습코드 = UPDATE & DELETE 구현</summary>
    
    from flask import Flask, render_template, jsonify, request

    app = Flask(__name__)

    board = [{"id": 1, "name": "elice", "context": "test"}]

    @app.route('/')
    def index():
        return render_template('index.html', rows = board)

    @app.route('/board', methods=['POST'])
    def create():
        data = request.get_json()
        board.append(data)
        return jsonify({"result":"success"})

    # 1번을 하세요
    @app.route('/board', methods=['DELETE'])
    def delete():
        del board[-1]

        return jsonify({"result":"success"})
        

    # 2번을 하세요
    @app.route('/board', methods=['PATCH'])
    def put():
        data = request.get_json()
        board[-1] = data

        return jsonify({"result":"success"})
    ```
</details>

### 4.flask 테스팅

#### 테스팅
: 테스트 코드는 내가 작성한 기능들이 의도대로 잘 수행하는지
기능 확인을 위한 테스트를 위해 작성합니다.

#### 테스트의 장점
- **테스트 환경 세팅 자동화**
특수한 상황에 맞는 파라미터도 사전에 정의할 수 있기 때문에, 매번
파라미터 값을 수동으로 바꿔가면서 테스트하지 않아도 됩니다.
- **통합 테스트 시간을 줄임**
통합 테스트에는 테스트코드로 검증하기 어려운 부분( 클라이언트
인터페이스 / 배치 스크립트와의 호환성 등)에만 집중할 수 있습니다.
- **외부와 의존성 있는 로직을 테스트하기 편리**
외부와 통신하는 로직을 Mock으로 처리해두면, 외부에서 발생할
수 있는 여러 환경을 내가 가짜로 구성할 수 있습니다.
- **전체 테스트 자동화**
모든 세부 기능을 통합 테스트에서 다 확인하기는 어렵다. 전체
테스트가 자동화되면, 수정된 코드가 다른 기능에 미친 영향을 쉽게
확인할 수 있습니다.

#### Get 테스팅
<u>flask</u>
```python
@app.route('/mysum', methods=['GET'])
def add():
data = request.args
num1 = int(data.get(“a”))
num2 = int(data.get(“b”))
return jsonify(
{'result': num1 + num2 }
)
```
<u>test</u>
```python
def test_get():
response = app.test_client().get(
'/mysum?a=10&b=20',
)
data = json.loads(
response.get_data()
)
assert response.status_code == 200
assert data['result'] == 200
```

#### POST 테스팅
<u>flask</u>
```python
@app.route('/add', methods=['POST'])
def add():
data = request.get_json()
return jsonify(
{'sum': data['a'] + data['b']}
)
```
<u>test</u>
```python
def test_add():
response = app.test_client().post(
'/add',
data=json.dumps({'a': 10, 'b': 2}),
content_type='application/json',
)
data = json.loads(
response.get_data(as_text=True)
)
assert response.status_code == 200
assert data['sum'] == 12
```

<details>
<summary>flask test 하기</summary>
    
지시사항
- app.py에 구현되어있는 API를 확인하여, GET 메서드의 역할과 POST 메서드의 역할을 구분하세요.
- test_app.py에 HTTP Method에 따른 테스트 함수 두개를 생성하려고 합니다.
    
    - def test_home_post()

    - def test_home_get()

- 위의 두 함수를 선언하세요.
- test_home_post() 함수를 완성하세요.
    해당 함수에서는 유저 이름을 add_name변수에 ‘oliver’로 저장 한 후에, post 방식으로 / url에 전송 하는 함수입니다.
    post() 내부에 들어갈 data의 형태는 json.dumps({'name': add_name,'age':20}) 이고, content_type은 application/json의 형태입니다.
    응답을 받는 변수는 response입니다.
- test_home_post() 함수에서 응답을 확인하기 위해서 data라는 변수를 선언 한 후 응답 받은 response를 json.loads에 담아주세요.
- assert구문을 활용해서, response.status_code가 200인지, data안의 result에 add_name에 전송 한 이름이 돌아오는지 검사하는 기능을 작성하세요.
- test_home_get() 함수는 API에서 보듯이, 현재 student 의 수를 반환하는 함수입니다. 테스트 흐름을 잘 생각해서 마지막 assert 구문에 올 변수와 결과값을 작성하세요.
<u>app.py</u>

```python
from flask import Flask, render_template, jsonify, request

app = Flask(__name__)

students = [
    {'name':'elice','age':'16'}
]

@app.route("/",methods=["GET","POST"])
def home():
    if request.method == 'POST':
        data = request.get_json()
        name = data['name']
        age = data['age']
        students.append({'name':name,'age':age})
        return jsonify({"result":name})       
    else:
        return jsonify({"count":len(students)})
    

if __name__ == '__main__':
    app.run(debug=True)
```

app_test.py

```python
from app import app
from flask import json


# test_home_post() 함수 작성하세요.
def test_home_post():
    # add_name을 생성하세요.
    add_name = 'oliver'
    # response를 선언하고 post 함수 기능을 작성하세요.
    response= app.test_client().post(
        '/',
        data= json.dumps({"name":add_name, "age":20}),
        content_type= 'application/json'
        )
    # data 변수를 선언하고 json.loads를 사용해 response 객체를 읽어오세요.
    data = json.loads(response.get_data(as_text=True))
    # assert를 사용해 status 값과 결과값 검사하세요.
    assert response.status_code == 200
    assert data['result'] == add_name


def test_home_get():
    
    response = app.test_client().get(
        '/',
    )
    data = json.loads(response.get_data())
    
    assert response.status_code == 200
    # API를 보고, 테스트 흐름에 맞는 결과값 작성하세요.
    assert data['count'] == 2
```

</details>
