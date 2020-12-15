> 로그인 정보를 파일애 넣어 놓자 -> Cookie

> 로그인 정보를 메모리상에 올려놓자 -> Session ( 엄밀히 따지면 Cookie + Session )
>
> 서버가 많아지면 모든 서버가 Session을 공유해야한다는 단점이 있음

> 토큰 
>
> 사용자 정보를 입력했을때 정보가 맞는다면 '토큰'을 받는다. 그후 사용자 정보를 확인해야할때 토큰으로 확인함. 
>
> 이후 서버에 리퀘스트를 날릴때는 리퀘스트헤더에 넣어서 보낸다.
>
> 토큰은 로컬 스토리지 혹은 Cookie에 저장한다.

# 1. 라우터 상태(Router state)



## 1.1 라우트 파라미터(Route Parameter) 전달

 **URL 패스를 라우터에 전달함**. 라우터는 이를 받아 해당하는 컴포넌트를 **검색후 활성화**한다. 그후   `<router-outlet></router-outlet>` 영역에 뷰를 출력한다.



navigate 메소드를 사용할 경우, 아래와 같이 URL 패스의 세그먼트로 구성된 배열을 인수로 전달한다.

```typescript
this.router.navigate(['/todo', todoId]);
```





## 1.2 라우트 파라미터(Route Parameter) 취득

`<router-oulet>` 영역에 렌더링된 컴포넌트 다시 말해 활성화된 컴포넌트는 ActivatedRoute 객체를 통해 라우터 상태(Router state)에 접근할 수 있다.

ActivatedRoute는 아래와 같은 프로퍼티를 제공한다.

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

라우트 **파라미터의 값을** 취득할 때는 ActivatedRoute의 **paramMap 프로퍼티**를 사용한다. paramMap 프로퍼티는 라우터에 전달된 라우트 파라미터의 맵을 포함하는 옵저버블이다.

**컴포넌트가 소멸되지 않은 상태**에서 라우터 **파라미터만 변경**된 라우터 상태를 **연속으로 수신**할 수 있기 때문에 **paramMap을 옵저버블로** 제공한다. paramMap의 **get 메소드**에 라우트 파라미터의 **키값을 인자로 전달**하여 라우트 **파라미터의 값을 취득**한다.

## 1.3 라우트 정적 데이터(Route static data)

**Route 인터페이스의 data 프로퍼티**는 컴포넌트로 전송할 **라우트 정적 데이터**로서 일반적으로 **애플리케이션 운영에 필요한 데이터를 전달할 때 사용**한다.

```typescript
const routes: Routes = [
  {
    path: 'todos',
    component: TodosComponent,
    data: { title: 'Todos', sidebar: true } /* 라우트 정적 데이터 */
  }
];
```



# 2. 자식 라우트(Child Route)

![Child Route](http://poiemaweb.com/img/child-route.png)

자식 컴포넌트도 루트컴포넌트와는 별도로 자신의 자식만을 위한 `<router-outlet>`을 가질 수 있다.

**children 프로퍼티**는 **자식 라우트**를 구성할 때 사용한다. 부모 컴포넌트는 루트 컴포넌트와는 별도의 `<router-oultet>`을 가지며 자식 컴포넌트는 부모 컴포넌트의 `<router-oultet>` 영역에 표시된다.



# 3. 모듈의 분리와 모듈별 라우트 구성

구성 요소를 모듈 단위로 구성하는 것과 동일하게 라우트를 모듈 단위로 구성할 수 있다.

```typescript
const routes: Routes = [ ... ];

@NgModule({
  ...
	//루트 모듈 또는 AppRoutingModule에서는 전체 라우트 정보를 담고 있는 라우트 구성을 RouterModule의 forRoot 메서드의 인자로 전달
  imports: [ RouterModule.forRoot(routes) ],
  ...
})



// 모듈 단위로 라우팅 구성을 분리하는 경우, 분리한 모듈에 RouterModule의 forChild 메서드의 인자로 라우트 구성을 등록
const routes: Routes = [ ... ];

@NgModule({
  ...
  imports: [ RouterModule.forChild(routes) ],
  ...
})
```



![모듈의 분리](http://poiemaweb.com/img/sep-module.png)

# 4. 라우트 가드(Route Guard)

라우터를 통해 컴포넌트나 **모듈을 활성화시킬 때** 또는 **컴포넌트에서 빠져나갈 때 권한 등을 체크**하여 **접근을 제어하는 방법**이다. 예를 들어 사용자 인증을 하지 않은 사용자의 접근을 제어하거나 다른 뷰로 이동하기 이전에 저장하지 않은 사용자 입력 정보가 있다면 사용자에게 알릴 수 있다.

## 4.1 CanActivate

가드는 라우트를 활성화할 수 있는지 결정하며 주로 뷰의 접근권한을 체크하고 접근을 제어할 때 사용함

CanActivate 인터페이스를 구현하여 가드 클래스를 정의한다. 이때 CanActivate.canActivate 메소드는 접근 권한 체크 로직을 수행하고 true 또는 false를 반환한다.

```typescript
// auth.guard.ts
import { Injectable } from '@angular/core';
import { Router, CanActivate } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Injectable()
export class AuthGuard implements CanActivate {

  constructor(private router: Router, private auth: AuthService) { }

  // 접근 권한 체크 로직을 수행하고 true 또는 false를 반환한다.
  canActivate() {
    // 토큰 유효성 확인
    if (!this.auth.isAuthenticated()) {
      // 토큰이 유효하지 않으면 로그인 페이지로 강제 이동
      this.router.navigate(['signin']);
      return false;
    }
    return true;
  }
}
```



**가드는 모듈에 등록되어야 한다.**

```typescript
// 라우트 구성
...
  {
    path: 'user',
    component: UserComponent,
    canActivate: [ AuthGuard ] /* 가드에 의한 접근 제한 */
  },
...
```

**AuthGuard.canActivate 메소드**의 **실행 결과가 true**일 경우에만 UserComponent를 활성화한다.

## 4.2 CanActivateChild

자식 라우트를 활성화할 수 있는지 결정한다. 주로 자식 컴포넌트로의 접근 권한을 체크하고 접근을 제어할 때 사용한다.

```typescript
// auth.guard.ts
import { Injectable } from '@angular/core';
import { Router, CanActivateChild } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Injectable() // CanActivateChild 인터페이스를 구현하고 가드클래스를 정의함
export class AuthChildGuard implements CanActivateChild {

  constructor(private router: Router, private auth: AuthService) { }

  // 접근 권한 체크 로직을 수행하고 true 또는 false를 반환한다.
  canActivateChild() { 
    // CanActivateChild.canActivateChild 메소드는 접근 권한 체크 로직을 수행하고 true 또는 false를 반환한다.
    // 토큰 유효성 확인
    if (!this.auth.isAuthenticated()) {
      // 토큰이 유효하지 않으면 로그인 페이지로 강제 이동
      this.router.navigate(['signin']);
      return false;
    }
    return true;
  }
}
```



가드는 모듈에 등록되어야한다.

```typescript
// 라우트 구성
...
  {
    path: 'customer',
    component: CustomerComponent,
    canActivateChild: [ AuthChildGuard ], /* 가드에 의한 접근 제한 canActivateChild 프로퍼티로 가드를 선언 */
    children: [
      { path: ':id', component: CustomerDetailComponent }
    ]
  },
...
```

자식 컴포넌트 CustomerDetailComponent를 활성화하기에 앞서 가드가 실행되고 가드의 실행 결과에 따라 자식 컴포넌트에의 접근을 제어한다. AuthChildGuard.canActivateChild 메소드의 실행 결과가 true일 경우에만 CustomerDetailComponent를 활성화.

## 4.3 CanLoad
## 4.4 Resolve
## 4.5 CanDeactivate







```sequence

클라이언트->서버: 로그인 
Note right of 서버: 세션에 저장
서버-->클라이언트: 24시간 저장

```









