웹페이지를 어플리케이션처럼 깜빡임이없는 UX제공하기위해 aJax가 탄생했다. 



# 1. SPA (Single Page Application)

단일 페이지 애플리케이션(Single Page Application, SPA)은 모던 웹의 패러다임이다.

SPA는 웹 애플리케이션에 필요한 **모든 정적 리소스**를 애플리케이션 **최초에 한번만 다운로드**한다. 이후 새로운 페이지 요청 시, **페이지 갱신에 필요한 데이터만을 전달받아 페이지를 갱신**하므로 **전체적인 트래픽을 감소**할 수 있고, **네이티브 앱과 유사한 사용자 경험을 제공**할 수 있다.

단점

1. 초기 구동 속도
2. SEO 문제

# 2. Routing

라우팅이란 출발지에서 목적지까지의 경로를 결정하는 기능이다. 사용자가 요청한 URL 또는 이벤트를 해석하고 새로운 페이지로 전환하기 위한 서버에 필요 데이터를 요청하고 **화면을 전환하는 일련의 행위**를 말한다.

1. URL 입력
2. a태그 클릭
3. form요소의 submit 버튼
4. 뒤로가기 앞으로가기 버튼



SEO문제를 해결하기 위해서는 사용자 방문의 historty를 관리 할 수 있어야한다. history 관리를 위해서는 각 페이지는 브라우저의 주소창에서 구별할 수 있는 유일한 URL을 소유하여야 한다.

# 3. Angular Router 개요와 위치 정책(Location strategy)



## 3.1 개요

Angular는 클라이언트 사이드 내비게이션 구현 방식으로 Angular 라우터를 제공한다. 사용자의 요청 URL을 해석하고 애플리케이션의 뷰를 담당하는 컴포넌트와 연결하는 역할을 한다.

결국에는 기존에 가지고 있던 컴포넌트를 없에고 새로운 컴포넌트로 갈아 끼우는 것이다.

```typescript
const routes: Routes = [
  { path: '', redirectTo: 'home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'service', component: ServiceComponent },
  { path: 'about', component: AboutComponent },
  { path: '**', component: NotFoundComponent }
];
```



## 3.2 Location strategy

a태그의 href 어트리뷰트를 사용하지않는 ajax는 url을 변경시키지 않으므로 histtory 관리가 동작하지않는다.

angular에서는 이와 같은 문제점을 해결할 수 있는 2가지 전략을 제공한다.

### 3.2.1 PathLocationStrategy (HTML5 Histroy API pushState 기반 내비게이션 정책)

PathLocationStrategy는 **Angular 라우터의 기본 정책**으로 pushState 메소드를 별도로 호출할 필요가 없다. 

```code
localhost:4200/service
```



### 3.2.2 HashLocationStrategy (Hash 기반 내비게이션 정책)

URL 패스에 fragment identifier의 고유 기능인 앵커(anchor)를 사용하는 정책으로 ‘/#/service’, ‘/#/about’과 같이 [해시뱅](https://blog.outsider.ne.kr/698)을 기반으로 한다. fragment identifier는 hash mark 또는 hash라고 부르기도 한다.

URL이 동일한 상태에서 hash가 변경되면 브라우저는 서버에 어떠한 요청도 하지 않는다. 즉, hash는 변경되어도 서버에 새로운 요청을 보내지 않으며 따라서 페이지가 갱신되지 않는다. HashLocationStrategy는 대부분의 브라우저에서 동작한다.

```code
localhost:4200/#/service
```

Hash 기반 내비게이션 정책을 기본으로 사용하려면 루트 모듈의 imports 프로퍼티를 아래와 같이 수정한다.

```typescript
@NgModule({
  imports: [
    BrowserModule,
    // PathLocationStrategy (기본 정책)
    // RouterModule.forRoot(routes)

    // HashLocationStrategy
    RouterModule.forRoot(routes, { useHash: true })
    //라우팅 모듈을 사용하는 경우, imports 프로퍼티를 아래와 같이 수정한다.
      imports: [RouterModule.forRoot(routes, { useHash: true })],
...
```



# 4. 라우터 구성 요소

이제 라우터를 구성하는 요소에 대해 살펴보도록 하자. 일반적으로 라우터는 아래의 수순으로 작성한다.

1. 라우트 구성

   Rout인터페이스의 배열(Routes 타입)을 사용하여 **요청 URL의 패스와 컴포넌트의 쌍**으로 만들어진 라우트를 구성한다.

2. 라우트 등록

   RouterModule.forRoot 또는 RouterModule.forChild를 호출하여 라우트 구성이 포함된 모듈을 생성하고 루트 모듈 또는 기능 모듈에 등록한다.

3. 뷰의 렌더링 위치 지정

   라우터가 컴포넌트를 렌더링하여 뷰를 표시할 영역인 `<router-oulet>`을 구현하는 RouterOutlet디렉티브를 선언하여 컴포넌트 뷰가 렌더링될 위치를 지정한다. RouterOutlet 디렉티브는 루트 컴포넌트 또는 기능 모듈의 컴포넌트에 선언한다.

4. 내비게이션 작성

   RouterLink 디렉티브를 사용한 HTML 링크 태그를 사용하여 내비게이션을 작성한다.

## 4.1 라우트 구성

```typescript
const routes: Routes = [
  { path: '', redirectTo: 'home', pathMatch: 'full' }, // 루트에서 접근시 home으로 리다이렉트
  { path: 'home', component: HomeComponent },
  { path: 'service', component: ServiceComponent },
  { path: 'about', component: AboutComponent },
  { path: '**', component: NotFoundComponent } // 지정해놓지않은 경로로 접근시 notFound로 
];

```

위 라우트 구성의 의미는 아래와 같다.

| 요청 URL 패스  | URL 범례               | 활성화될 컴포넌트 |
| -------------- | ---------------------- | ----------------- |
| home           | localhost:4200/home    | HomeComponent     |
| service        | localhost:4200/service | ServiceComponent  |
| about          | localhost:4200/about   | AboutComponent    |
| 상기 패스 이외 | localhost:4200/some    | NotFoundComponent |

## 4.2 라우트 등록

라우트는 모듈 단위로 구성한다. 그리고 구성된 라우트는 모듈에 등록한다.

먼저 Routes와 RouterModule을 import하고 라우트 구성을 추가한다.

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

// 컴포넌트 임포트
import {
  HomeComponent,
  ServiceComponent,
  AboutComponent,
  NotFoundComponent
} from './pages';

// 라우트 구성
const routes: Routes = [
  { path: '', redirectTo: 'home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'service', component: ServiceComponent },
  { path: 'about', component: AboutComponent },
  { path: '**', component: NotFoundComponent }
];

@NgModule({
  /* 모든 라우트 구성을 포함한 모듈을 생성하고 라우팅 모듈에 추가 */
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }



// 루트 모듈에 라우팅 모듈을 등록한다.
// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
// 라우팅 모듈 임포트
import { AppRoutingModule } from './app-routing.module';

import { AppComponent } from './app.component';

// 컴포넌트 임포트
import {
  HomeComponent,
  ServiceComponent,
  AboutComponent,
  NotFoundComponent
} from './pages';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    ServiceComponent,
    AboutComponent,
    NotFoundComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule /* 라우팅 모듈 등록 */
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- 라우팅 모듈 app-routing.module.ts를 자동 생성한다.
- app.component.ts에 `<router-outlet></router-outlet>`을 추가한다.
- 루트 모듈 app.module.ts에 라우팅 모듈을 import한다.



## 4.3 뷰의 렌더링 위치 지정과 내비게이션 작성
# 4.3.1 RouterOutlet

라우터가 컴포넌트를 렌더링하여 뷰를 표시할 영역인 `<router-oulet>`을 구현한 디렉티브로 컴포넌트의 뷰를 렌더링할 위치를 설정한다.

```html
<router-outlet></router-outlet>
```

# 4.3.2 RouterLink

Angular 라우터를 사용하기 위해서는 컴포넌트의 템플릿에는 뷰를 전환하기 위한 a 태그의 href 어트리뷰트 대신 RouterLink 디렉티브를 사용하여 URL 패스를 지정한다.

```html
<!-- app.component.ts -->
<nav>
  <a routerLink="/home">Home</a>
  <a routerLink="/service">Service</a>
  <a routerLink="/about">About</a>
</nav>
<router-outlet></router-outlet>
```



# 4.3.3 RouterLinkActive

현재 브라우저의 URL 패스가 RouterLink 디렉티브에서 **지정한 URL 패스의 트리에 포함**되는 경우, RouterLinkActive에 지정된 클래스명을 DOM에 자동으로 추가한다.

```html
<a routerLink="/service"  [routerLinkActiveOptions]="{ exact: true }"
 routerLinkActive="active">Service</a>
```

`routerLinkActiveOptions`는 지정한 url패스가 **정확히 일치**하는경우 클래스를 추가하게끔 한다.

# 5. navigate 메소드

템플릿의 **링크 태그를 사용하지 않고**(`button` ) 컴포넌트 클래스의 코드만으로 화면을 전환하기 위해서는 navigate 메소드를 사용한다.

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { Routes, RouterModule } from '@angular/router';

import { AppComponent } from './app.component';
import { TodosComponent } from './todos.component';

// 라우트 구성
const routes: Routes = [
  { path: 'todos', component: TodosComponent }
];

@NgModule({
  imports: [
    BrowserModule,
    RouterModule.forRoot(routes)
  ],
  declarations: [
    AppComponent,
    TodosComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }



// app.component.ts
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  template: `
    <button (click)="gotoTodos()">goto todos</button>
    <router-outlet></router-outlet>
  `
})
export class AppComponent {
  constructor(private router: Router) {}

  gotoTodos() {
    // /todos로 이동
    this.router.navigate(['todos']);
  }
}
```



