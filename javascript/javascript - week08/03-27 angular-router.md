# 1. 라우터 상태(Router state)

RouterLink 디렉티브는 자신의 값 즉 **URL 패스를 라우터에 전달**하고 **라우터는 이를 전달받아** 라우트 구성에서 전달받은 값(URL 패스)에 해당하는 **컴포넌트를 검색하고 활성화**하여 `<router-outlet></router-outlet>` 영역에 뷰를 **출력**한다.

```html
<a routerLink="/todo/:id">...</a>
```

```typescript
const routes: Routes = [
  { path: '', component: TodosComponent },
  { path: 'todo/:id', component: TodoDetailComponent }
];
```



**라우터 파라미터**로 전달할 값은 대부분 변수 또는 객체의 프로퍼티에 담겨있는 **동적인 값**일 것이다. 이러한 경우, RouterLink 디렉티브에 **URL 패스의 세그먼트로 구성된 배열을 할당**한다. 

```html
<a [routerLink]="['/todo', todoId]">...</a> <!-- todoId = 10 -->

<!-- 출력 --> 
<a routerLink="/todo/10">...</a>
```



navigate 메소드 사용시, url패스의 세그먼트로 구성된 배열을 인수로 전달함

```typescript
this.router.navigate(['/todo', todoId]);
```





라우터 구성과 모듈은 관심사가 다르므로 라우터를 구분하여야한다.



## 1.1 라우트 파라미터(Route Parameter) 전달

![모듈의 분리](http://poiemaweb.com/img/sep-module.png)

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

// 컴포넌트 임포트
import { TodosComponent } from './todos/todos.component';
import { TodoDetailComponent } from './todos/todo-detail.component';

// 라우트 구성
const routes: Routes = [
  { path: '', component: TodosComponent },
  { path: 'todo/:id', component: TodoDetailComponent }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ] // 각각의 모듈이 독립적이기 위하여 RouterModule을 export함 
})
export class AppRoutingModule { }
```

루트 URL(localhost:4200)로 접근하면 루트 컴포넌트의 `<router-outlet></router-outlet>` 영역에 TodosComponent가 표시되고 localhost:4200/todo/1과 같이 접근하면 `<router-outlet></router-outlet>` 영역에 TodoDetailComponent이 표시된다.

```typescript
// todos/todos.component.ts
import { Component, OnInit } from '@angular/core';

interface Todo {
  id: number;
  content: string;
  completed: boolean;
}

@Component({
  selector: 'app-todos',
  template: `
    <ul>
      <li *ngFor="let todo of todos">
        <a [routerLink]="['/todo', todo.id]">{{ todo.content }}</a>
      </li>
    </ul>
  `
})
export class TodosComponent implements OnInit {
  todos: Todo[];

  ngOnInit() {
    // 잠정 처리. 실제 환경에서는 서비스를 통해 서버로 부터 데이터를 취득할 것이다.
    this.todos = [
      { id: 3, content: 'HTML', completed: false },
      { id: 2, content: 'CSS', completed: true },
      { id: 1, content: 'Javascript', completed: false }
    ];
  }
}
```

```html
<li *ngFor="let todo of todos">
  <a [routerLink]="['/todo', todo.id]">{{ todo.content }}</a> // /todo에서 todo.id 를 검색
</li>
```



## 1.2 라우트 파라미터(Route Parameter) 취득

ActivatedRoute 객체는 다양한 라우터 상태를 가지며 이 중에서 라우트 파라미터를 추출할 수 있다. ActivatedRoute는 아래와 같은 프로퍼티를 제공한다.

```typescript
interface ActivatedRoute {
  snapshot: ActivatedRouteSnapshot
  url: Observable<UrlSegment[]>
  params: Observable<Params>
  queryParams: Observable<Params>
  fragment: Observable<string>
  data: Observable<Data>
  outlet: string
  component: Type<any>|string|null
  ...
}
```



TodoDetailComponent에 ActivatedRoute 객체의 인스턴스를 주입받도록 하자.

```typescript
// todo-detail.component.ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-todo-detail',
  template: `
    <h3>todo detail</h3>
    <p>todo id : {{ todoId }}</p>
    <a routerLink="/">Back to Todos</a>
  `
})
export class TodoDetailComponent implements OnInit {

  todoId: number;

  // ActivatedRoute 객체의 인스턴스를 의존성 주입을 통해 주입받는다.
  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    // 의존성 주입을 통해 받은 라우트 파라미터 값의 취득
    this.route.paramMap
      .subscribe(params => this.todoId = +params.get('id')); // +는 형변환때문
  }
}
```







옵저버블 스트림이 아닌 **특정 시점의 상태만을 조회**하는 경우, ActivatedRoute의 **snapshot 프로퍼티를 사용**할 수 있다. snapshot 프로퍼티는 **옵저버블로 래핑되지 않은 paramMap 객체를 반환**한다. 

```typescript
// todo-detail.component.ts

...

ngOnInit() {
    // this.route.paramMap
      // .subscribe(params => this.todoId = +params.get('id'));

    this.todoId = +this.route.snapshot.paramMap.get('id');
  }
```



## 1.3 라우트 정적 데이터(Route static data)
# 2. 자식 라우트(Child Route)
# 3. 모듈의 분리와 모듈별 라우트 구성
# 4. 라우트 가드(Route Guard)
 4.1 CanActivate
 4.2 CanActivateChild
 4.3 CanLoad
 4.4 Resolve
 4.5 CanDeactivate