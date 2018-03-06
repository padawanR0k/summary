이벤트 핸들러는 대부분 DOM엘리먼트로 이벤트를 잡아낸다.

어떤 이벤트들은 서버에 정보를 보내야하기도 한다.  ajax보낸 정보를 이용하여 View를 변화시키는 방법을 쓰는게 오늘날의 대부분의 웹이다.



상태의 변화감지 

> 입력을 하는 부분(ex 로그인폼)에 값이 변경될때마다 그것을 감지하는 것. angular는 변화를 감지할 수 있다.





서버의 기본기능

1. 정적파일의 제공 
2. REST API - 수많은 request을 구별하고 각각의 request에 대한  response를 해줘야 한다.
3. DB



이전의 대부분의 백엔드 기술셋 

- Java / Spring	: 장황한 면이 있음

요즘 치고올라오는 백엔드

- python
- nodeJS
  - 장점 : 빨리 쉽게만들 수 있다.
  - 단점 : 안정성 부족, 레퍼런스 부족



# 1. Introduction

Node.js는 크롬의 V8엔진으로 빌드된 런타임환경이다(브라우저 외부환경에서 자바스크립트를 작동하게끔 만들어짐)

브라우저 외부 환경에서 JavaScript 애플리케이션 개발에 사용되며 이에 필요한 모듈, 파일 시스템, HTTP 등 Built-in API를 제공한다.

 **Node.js**는 **Non-blocking I/O(비동기처리)와 단일 스레드 이벤트 루프**를 통한 높은 Request 처리 성능을 가지고 있다. 단일 스레드는 장점이 될수도 있고 단점이 될수도 있다.

Node.js는 데이터를 실시간 처리하여 빈번한 I/O가 발생하는 SPA(Single Page Application)에 적합하다. 간단하지않은 처리를하는 CPU 사용률이 높은 무거운 애플리케이션에는 권장하진 않는다.



프론트엔드와 백엔드의 구분은 정확히 나누어져 있지 않고 겹쳐있다.

![Isomorphic-JavaScript](http://poiemaweb.com/img/Isomorphic-JavaScript.png)







# 2. Install

[http://nodejs.org/](https://nodejs.org/)

LTS(Long Term Supported) 버전을 깔자. 



# 3. Update



## 3.1 Node.js

nodeJs는 업데이트가 비교적 빠르기 때문에 관리가 용이하게 버전매니저 nvm 을 설치하자.



## 3.2 npm

npm은 Node.js보다 자주 업데이트되므로 최신 버전이 아닐 수 있다. 최신 버전으로 npm을 업데이트하도록 한다.

```bash
$ npm install -g npm@latest
$ npm -v
5.6.0
```

# 4. REPL

REPL(Read Eval Print Loop: 입력 수행 출력 반복)은 Node.js는 물론 대부분의 언어(Java, Python 등)이 제공하는 가상환경으로 간단한 코드를 직접 실행해 결과를 확인해 볼 수 있다. 터미널(윈도우의 경우 커맨드창)에 다음과 명령어를 실행시켜 보자.



Node.js 파일을 실행하려면 node 명령어 뒤에 파일명을 입력한다.

```bash
$ node
> 1 * 0
0
> x = 10
10
> console.log('Hello World')
Hello World
undefined
$ node index.js
```



# 5. Node.js 맛보기 : HTTP Server

간단한 HTTP Server를 작성해 보자. Node.js는 http 서버 모듈을 내장하고 있어서 아파치와 같은 별도의 웹서버를 설치할 필요가 없다.

```js
// app.js
const http = require('http'); // http 모듈을 로딩, http에 할당

http.createServer((request, response) => { //.createServer의 반환값은 서버객체
  // request, response 객체로 여기서 지지고 볶고 한다.
  response.statusCode = 200; // response객체의 상태코드
  response.setHeader('Content-Type', 'text/plain'); // response 메시지의 헤더
  response.end('Hello World'); // 
}).listen(3000); // 서버를 기동시켜서 대기상태에 들어가게함. 그때 포트번호 3000번 으로 지정함

console.log('Server running at http://127.0.0.1:3000/');
```



app.js 실행 후  http://localhost:3000/ 에 접속하면 Hello World가 출력

```bash
$ node app.js
```

