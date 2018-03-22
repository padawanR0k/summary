# 1. HttpClient

대부분의 웹 애플리리케이션이 그러하듯이 Angular 애플리케이션은 HTTP 프로토콜을 통해 서버와 통신한다. 

Angular는 @angular/http 패키지의 Http 클래스를 통해 발전된 HTTP 요청 API와 인터셉터(Interceptor)를 제공한다.

HttpClient 클래스의 내부

```typescript
import {Injectable} from '@angular/core'; // injectalbe이 있는거보니 하나의 서비스이며 의존성주입을 통해 쓰는듯하다.
import { Observable } from 'rxjs/Observable';
...

import { HttpHandler } from './backend';
import { HttpHeaders } from './headers';
import { HttpParams } from './params';
import { HttpRequest } from './request';
import { HttpEvent, HttpResponse } from './response';
...

@Injectable()
export class HttpClient {
  constructor(handler: HttpHandler)

  request(first: string|HttpRequest<any>, url?: string, options: {...}): Observable<any>

  delete(url: string, options: {...}): Observable<any>

  get(url: string, options: {...}): Observable<any>

  head(url: string, options: {...}): Observable<any>

  jsonp<T>(url: string, callbackParam: string): Observable<T>

  options(url: string, options: {...}): Observable<any>

  patch(url: string, body: any|null, options: {...}): Observable<any>

  post(url: string, body: any|null, options: {...}): Observable<any>

  put(url: string, body: any|null, options: {...}): Observable<any>
}
```



# 2. HttpClientModule

**HttpClient 클래스**를 사용하려면 HttpClient를 제공하는 **HttpClientModule**을 **모듈에 추가**하여야 한다. HttpClient를 애플리케이션 전역에서 사용할 수 있도록 **루트 모듈에 HttpClientModule을 임포트**한다.

# 3. HTTP 요청



### 3. 1 GET

```typescript
// http-get.component.ts
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

interface Todo {
  id: number;
  content: string;
  completed: boolean;
}

@Component({
  selector: 'app-http-get',
  template: `
    <ul>
      <li *ngFor="let todo of todos">{{ todo.content }}</li>
    </ul>
    <pre>{{ todos | json }}</pre>
  `
})
export class HttpGetComponent implements OnInit {
  todos: Todo[];
  url = 'http://localhost:3000/todos';

  // HttpClient를 컴포넌트에 주입
  constructor(public http: HttpClient) {}

  ngOnInit() {
     // HTTP 요청: 타입 파라미터를 기술한다. 기술안하면 Object 타입이 넘어옴
    this.http.get<Todo[]>(this.url) 
      // 요청 결과를 프로퍼티에 할당
      .subscribe(todos => this.todos = todos);
  }
}
```

# 
# 3.1.1 responseType

```typescript
ngOnInit() {
  // HTTP 요청: 텍스트 파일을 요청
  this.http.get('/textfile.txt', {responseType: 'text'})
    // get 메소드는 Observable<string>를 반환한다.
    .subscribe(data => console.log(data));
}
```

responseType을 설정한 경우, 타입 파라미터를 지정할 필요가 없으며 get 메소드는 `Observable<지정한 타입>`을 반환한다.

# 3.1.3 HttpParams

![uri](http://poiemaweb.com/img/uri.png)

GET 요청은 쿼리 파라미터와 함께 전달할 수 있다. 참고로 URI(Uniform Resource Identifier)는 아래와 같은 구성을 갖는다.

```typescript
// http://localhost:3000/todos?id=1&completed=false을 코딩해보자


ngOnInit() {
  // 쿼리 파라미터 생성
  const params = new HttpParams()
    .set('id', '1')
    .set('completed', 'false');

  // HTTP 요청
  this.http.get<Todo[]>(this.url, {params})
    // 요청 결과를 프로퍼티에 할당
    .subscribe(todos => this.todos = todos);
}
```





# 3.1.4 HttpResponse

지금까지의 예제는 **todos 데이터(response body)만을 리턴**받았을 뿐이다. 특정 **헤더 정보나 상태 코드**(status code)를 **확인**하려면 **전체 응답(response)을 받아야** 한다. 

```typescript
this.http.get<Todo[]>(this.url, { observe: 'response' }) // observe 옵션을 사용하여 전부받는다
  .subscribe(res => {
    console.log(res);
    // HttpResponse {headers: HttpHeaders, status: 200, statusText: "OK", url: "http://localhost:3000/todos", ok: true,&nbsp;…}

    console.log(res.headers);
    // HttpHeaders {normalizedNames: Map(0), lazyUpdate: null, lazyInit: ƒ}

    console.log(res.status); // 200
    this.todos = res.body;   // todos
  });
```



# 3.1.5 에러 핸들링

서버 요청이 실패하였거나 네트워크 연결에 문제가 있어서 에러가 발생하였을 경우, HttpClient는 정상 응답 대신 에러를 반환한다. 이때 subscribe의 두번째 콜백함수(Observer의 error 함수)가 호출된다.

```typescript
ngOnInit() {
  this.http.get<Todo[]>(this.url)
    .subscribe(
      // 요청 성공 처리 콜백함수 (Observer의 next 함수)
      todos => this.todos = todos,
      // 요청 실패 처리 콜백함수 (Observer의 error 함수)
      (err: HttpErrorResponse) => {
        if (err.error instanceof Error) { // Error 객체내부의 err.error
          // 클라이언트 또는 네트워크 에러
          console.log(`Client-side error: ${err.error.message}`);
        } else {
          // 백엔드가 실패 상태 코드 응답
          console.log(`Server-side error: ${err.status}`);
        }
      }
    );
}
```

에러의 유형은 두 가지이다.

- 네트워크 오류로 인해 요청이 성공적으로 완료되지 못한 경우 또는 RxJS 오퍼레이터의 예외가 발생한 경우, err 파라미터는 Error 객체의 인스턴스이다. 이때 에러는 클라이언트 측의 문제로 발생한 것이다.
- err 파라미터가 Error 객체의 인스턴스가 아닌 경우, 백엔드가 실패한 상태 코드를 응답한 에러이다. 이때 status 프로퍼티로 응답 코드(404, 500 등)를 확인 할 수 있다.

## 3.2 POST

```typescript
ngOnInit() {
    this.getTodos();
  }

  getTodos() { // 공통메서드로 빼놓음
    this.http.get<Todo[]>(this.url)
      .subscribe(todos => this.todos = todos);
  }

  // 새로운 todo를 생성한다
  addTodo() {
    if (!this.content) { return; }

    // 서버로 전송할 요청 페이로드
    // id는 json-server에 의해 자동 생성된다
    const payload = { content: this.content, completed: false }; // json server 에서 id를 자동으로 채번함

    this.http.post(this.url, payload)
      .subscribe(() => this.getTodos());

```



# 3.2.1 HttpHeaders

브라우저가 자동 성성하는 헤더 이외에 커스텀 헤더를 추가할 때 HttpHeaders 클래스를 사용한다.

 반드시 체이닝하여 사용해야 한다.

```typescript
addTodo() {

  // 헤더 생성
  const headers = new HttpHeaders()
    .set('Authorization', 'my-auth-token');

  const payload = { content: this.content, completed: false };

  // 요청 페이로드와 커스텀 요청 헤더 전송
  this.http.post(this.url, payload, { headers })
    .subscribe(() => this.getTodos());

  this.content = '';
}
```



# 3.3 PUT

PUT 요청은 리소스를 갱신할 때 사용한다. PUT 요청은 데이터의 값 일부만을 갱신할 때 사용한다. ex) 여러 todos중 todo하나만 바꿀때

```typescript
export class HttpPutComponent implements OnInit {
  todos: Todo[];
  url = 'http://localhost:3000/todos';

  // HttpClient를 컴포넌트에 주입
  constructor(public http: HttpClient) {}

  ngOnInit() {
    this.getTodos();
  }

  getTodos() {
    this.http.get<Todo[]>(this.url)
      .subscribe(todos => this.todos = todos);
  }

  // id가 일치하는 todo의 모든 프로퍼티를 변경한다
  editTodo(id) { // 버튼요소가 눌릴때마다 put요청을 보낸다. payload는 수정하려는 내용
    const payload = { content: 'Angular!', completed: true };

    this.http.put(`${this.url}/${id}`, payload)
      .subscribe(() => this.getTodos()); // getTodos()를 사용해 다시 그린다.
  }
```



# 3.4 PATCH

PATCH 요청은 리소스의 일부를 갱신할 때 사용한다. PATCH 요청은 리소스의 일부를 갱신할 때 사용한다. ex) todo리소스중 completed를 반전시키고 싶을때.

```typescript
 completeTodo(todo) {
    const {id, completed} = todo;
    const payload = { completed: !completed };

    this.http.patch(`${this.url}/${id}`, payload) // {id, content, completed}중 completed만 갱신함
      .subscribe(() => this.getTodos());
 }
```



# 3.5 DELETE

DELETE 요청은 리소스를 삭제할 때 사용한다.

```typescript
 deleteTodo(id) {
    this.http.delete(`${this.url}/${id}`) // id가 일치하는 todo를 삭제한다
      .subscribe(() => this.getTodos());
  }
```



# 4. HTTP 요청 중복 방지

```typescript
import 'rxjs/add/operator/shareReplay';

ngOnInit() {
  const tods$ = this.getTodos();
  tods$.subscribe(todos => this.todos = todos);
  tods$.subscribe(todos => this.todos = todos);
    // todo$를 2번 구독함
}

getTodos(): Observable<Todo[]> {
  return this.http.get<Todo[]>(this.url)
    .shareReplay(); // 요청이 2번가는게 정상이지만 shareReplay()를 써서 구독을 공유함?으로써 http요청은 1번만감
}
```

