정적파일을 제공하며 라우터의 기능

프로젝트의 뼈대를 만들어주는 툴이있다.





 Node.js 환경에서 동작하는 Web application Framework이다. Express는 Web Application 구성에 필요한 Routing, View Helper, Session 등 기능을 제공한다.

> 요즘엔 Session보다는 JWT을 사용한다.

# 1. Install

코드 : http://poiemaweb.com/express-basics

프로젝트 폴더를 생성하고 npm init로 package.json을 생성한 후 express install

# 2. Hello world example

```js
const express = require('express'); // express 담고
const app = express(); // express 호출하고 app에다가 할당

app.get('/', (req, res) => res.send('Hello World! ~~~'));
app.get('/test', (req, res) => res.send('test'));
// get메서드로 root url로 요청이오면 send()에 들어온걸 실행함.
// 서버가 클라이언트한테 줄 정보는 res에 담아서 준다.
// 요것들을 이벤트리스너라고 생각하자.

app.listen(3000, () => console.log('Example app listening on port 3000!'));


```



프로젝트 폴더(myapp)에 app.js를 생성

터미널에서 다음 명령을 실행하여 application을 구동

서버는 port 3000에서 사용자의 접속을 대기하고 있다. 클라언트가 root URL(http://localhost:3000/)로 요청를 보내면 서버는 ‘Hello World!’로 응답하게 된다.

# 3. Routing

이러한 클라이언트 요청에 응답하는 방법을 결정하는 것을 라우팅이라 한다. 각 라우트는 하나 이상의 핸들러 함수를 가질 수 있으며, 이러한 함수는 라우트가 일치할 때 실행된다.

라우트 정의에는 다음과 같은 구조가 필요하다.

![define route](http://poiemaweb.com/img/define-route.png)

app : express()로 생성된 객체

method : get, put 같은 메소드

path: 경로



위의 클라이언트 요청에 대응하는 route를 설정해보자.

먼저 request body parsing 미들웨어인 body-parser를 설치한다. body-parser 미들웨어는 페이로드(POST 요청 데이터와 같이 Request message의 body에 담겨 보내진 데이터)를 request 객체의 body 프로퍼티에 바인딩한다.

```bash
$ npm install body-parser
```

설치가 되었으면 아래와 같이 app.js를 수정한다.







## 3.1 Route method

Express는 HTTP 메소드에 해당하는 다음과 같은 라우팅 메소드를 지원한다.

```code
get, post, put, head, delete, options, trace, copy,
lock, mkcol, move, purge, propfind, proppatch, unlock,
report, mkactivity, checkout, merge, m-search, notify,
subscribe, unsubscribe, patch, search, connect.
```

여기서 자주쓰이는 메소드는 `get, post, put, delet, patch` 정도가 있다.

app.all() 메소드는 모든 HTTP method에 대응한다. next()를 사용하면 후속 route handler로 제어를 전달할 수 있다.

```js
// 모든 요청 메소드에 대응
app.all('/', (req, res, next) => { // next라는 매개변수 받음
  console.log('[All]');
  next(); // 후속 핸들러에게 컨트롤을 패스한다.
});

app.get('/', (req, res, next) => { // 컨트롤 받았음.
  console.log('[GET 1] next 함수에 의해 후속 핸들러에게 response가 전달된다.');
  next();
}, (req, res, next) => {
  console.log('[GET 2] next 함수에 의해 후속 핸들러에게 response가 전달된다.');
  next();
}, (req, res) => res.send('Hello from GET /'));

```

콜백함수 내부에 공통된 처리가 필요할때 공통된 부분을 `.all`에 넣고 다른부분만 나누어 놓는다.



## 3.2 Route path

Route path에는 문자열 또는 정규표현식을 사용할 수 있다.

```js
// localhost:3000/
app.get('/', (req, res) => res.send('root'));

// localhost:3000/about
app.get('/about', (req, res) => res.send('about'));

// localhost:3000//random.text
app.get('/random.text', (req, res) => res.send('random.text'));

// localhost:3000/<number>
app.get(/^\/[0-9]+$/, (req, res) => res.send('regexp'));

// localhost:3000/user/<userId>/item/<itemId>
app.get('/user/:userId/item/:itemId', (req, res) => { // ':' 이붙으면 매개변수를 뜻함
  const { userId, itemId } = req.params; // /user/:userId/item/:itemId 요청파리미터에서 값을 뽑아올때 사용 '.params'
  res.send(`userId: ${userId}, itemId: ${itemId}`);
});
```



## 3.3 Route handler

Route handler는 요청을 처리하는 콜백함수이다.

```js
app.get('/example/a', (req, res) => res.send('Hello from A!'));
```

## 3.4 Response method

| 메소드                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [res.download()](http://expressjs.com/ko/4x/api.html#res.download) | 다운로드될 파일을 전송한다.                                  |
| [res.end()](http://expressjs.com/ko/4x/api.html#res.end)     | 응답 프로세스를 종료한다.                                    |
| [res.json()](http://expressjs.com/ko/4x/api.html#res.json)   | JSON 응답을 전송한다.                                        |
| [res.jsonp()](http://expressjs.com/ko/4x/api.html#res.jsonp) | JSONP 지원을 통해 JSON 응답을 전송한다.                      |
| [res.redirect()](http://expressjs.com/ko/4x/api.html#res.redirect) | 요청 경로를 재지정한다. 다른 라우터로 리다이렉트한다.        |
| [res.render()](http://expressjs.com/ko/4x/api.html#res.render) | view template을 렌더링한다.  ex) ejs, pug, handlebar         |
| [res.send()](http://expressjs.com/ko/4x/api.html#res.send)   | **다양한 유형의 응답을 전송한다.**                           |
| [res.sendFile()](http://expressjs.com/ko/4x/api.html#res.sendFile) | 파일을 옥텟 스트림(이메일이나 http에서 사용되는 content-type에서 application의 형식이 지정되어 있지 않은 경우에 octet-stream이라고 한다)의 형태로 전송한다. |
| [res.sendStatus()](http://expressjs.com/ko/4x/api.html#res.sendStatus) | 응답 상태 코드(response status code)를 설정한 후 해당 코드를 문자열로 표현한 내용을 응답 본문으로서 전송한다.  구체적인 statuscode를 보낼때 사용한다. |

# 4. Middleware

express의 핵심

```js
const express = require('express');

const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');

const app = express();

// parse application/json
app.use(bodyParser.json());
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());

// 여기서 .use가 미들웨어 
// bodyParser, cookieParser() 같은 메서드, 함수를 호출하면서 res, req를 건드린다.
// 보기엔 떨어져있지만 같은 res, req를 사용하기 때문에 체이닝이 되는거나 마찬가지임
// 내부에서는 next()를 호출한다. == control이 움직인다.
```





# 5. 정적 파일의 제공

`express.static`을 사용하면 정적 파일들이 저장되어 있는 디렉터리명을 express.static 함수에 전달하면 정적 파일 서비스를 사용할 수 있다.

```js
app.use(express.static('public')); // public으로 루트폴더 지정
```

Express의 기본 제공 미들웨어 함수 `express.static()`



클라이언트는 값을 보내야할때 send메세지의 인자로 준다.  

클라이언트가 값을 받을수 있는 기능은? 2가지 payload, URL에 붙여서



# 6. Template engine

handlebars, pug, ejs와 같은 템플릿 엔진을 사용할 수 있다.

html을 서버에서 렌더링하여 보낸다.

```bash
$ npm install express-handlebars
```

```js
...
const exphbs = require('express-handlebars');

// 템플릿은 views 디렉터리에 작성한다.(기본 설정)
app.engine('handlebars', exphbs({ defaultLayout: false }));
app.set('view engine', 'handlebars');

app.get('/', (req, res) => {
  res.render('home', { body: 'Hello world' })
});
```

```js
<!-- views/home.handlebars -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Example App</title>
</head>
<body>
  {{{ body }}} // Hello world
</body>
</html>
```

