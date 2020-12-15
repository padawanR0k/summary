# 1. REST(Representational State Transfer) API 

HTTP/1.0과 1.1의 스펙 작성에 참여하였고 아파치 HTTP 서버 프로젝트의 공동설립자인 로이 필딩 (Roy Fielding)의 2000년 논문에서 처음 소개되었다. 웹의 장점을 최대한 활용할 수 있는 아키텍쳐로 소개된것이 REST이다.

> 좋은걸 만들어놨는데 왜 못 쓰고 잇냐? 이렇게 써라~ 
>
> 해서 만든게 REST

REST의 기본 원칙을 성실히 지킨 서비스 디자인을 “RESTful”이라고 표현한다.



# 1. REST API 중심 규칙

**URI는 자원**을 표현하는 데에 집중하고 **행위에 대한 정의는 HTTP Method**를 통해 하는 것이 REST한 API를 설계하는 중심 규칙이다.

URI = 명사

HTTP Method = 동사 

1. URI는 정보의 자원을 표현해야 한다.

   - ```code
     # bad
     GET /getBooks/1 // get X
     GET /books/show/1 // show X

     # good
     GET /books/1
     ```

      	

2. 자원에 대한 행위는 HTTP Method(GET,POST 등)으로 표현한다.

   - ```code
     # bad
     GET /books/delete/1

     # good
     DELETE /books/1
     ```

     ​

# 2. HTTP Method

4가지의 Method(GET, POST, PUT, DELETE)를 사용하여 CRUD를 구현한다.

| Method | Action         | 역할                    |
| ------ | -------------- | ----------------------- |
| GET    | index/retrieve | 모든/특정 리소스를 조회 |
| POST   | create         | 리소스를 생성           |
| PUT    | update         | 리소스를 갱신           |
| DELETE | delete         | 리소스를 삭제           |

# 3. REST API의 구성

REST API는 자원(Resource), 행위(Verb), 표현(Representations)의 3가지 요소로 구성된다. REST는 자체 표현 구조로 구성되어 REST API만으로 요청을 이해할 수 있다.

| 구성 요소       | 내용                    | 표현 방법             |
| --------------- | ----------------------- | --------------------- |
| Resource        | 자원                    | HTTP URI              |
| Verb            | 자원에 대한 행위        | HTTP Method           |
| Representations | 자원에 대한 행위의 내용 | HTTP Message Pay Load |



# 4. REST API의 Example
## 4.1 json-server

json-server를 사용해서 REST API를 사용해보자 

http://poiemaweb.com/js-rest-api#4-rest-api%EC%9D%98-example

## 4.2 GET

books 리소스에서 모든 책을 조회(index)한다.

로그인form을  GET 방식으로 전송하면 ?id=foo&password=bar 처럼 보내는 데이터가 전부 보인다. 정보를 가리기위해 POST를 사용한다. 



## 4.3 POST

books 리소스에 데이터를 생성한다. 

request message의 body에 원하는 데이터(payload)를 넣어서 보내자

```js
var req = new XMLHttpRequest();
req.open('POST', 'http://localhost:5000/books');
req.setRequestHeader('Content-type', 'application/json');
req.send(JSON.stringify({
  title: "ES6",
  author: "Choi"
}));

req.onreadystatechange = function (e) {
  if (req.readyState === XMLHttpRequest.DONE) {
    if(req.status === 201) {  // POST의 경우만 201번으로넘어 온다.
      console.log(req.responseText);
    } else {
      console.log("Error!");
    }
  }
};
```



## 4.4 PUT

books 리소스에서 원하는 값을 새로이 갱신한다

payload가 필요함

## 4.5 DELETE

books 리소스에서 책을 삭제한다.	

# Reference