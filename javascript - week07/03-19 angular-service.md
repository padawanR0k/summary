# 1. 서비스(Service)란?

 **애플리케이션 전역 관심사를 분리**하는 것, 애플리케이션에서 유일한 객체, 서비스는 클래스로 이루어져있고 인스턴스를 직접 생성하지 않는다. 

view를 관리하는 로직과 서버를 통신하는 로직이 섞이는것은 관심사가 섞인다는 것과 같다.  기능이 다르다면 분리해 놓는것이 좋다.

ex) todolist를 db에서 추가, 삭제, 수정할때 여러 페이지에서 사용할 가능성이 높기때문에 앵귤러의 서비스를 사용하여 분리해놓는다. direct와 비슷한느낌



그럼 watcha의 평점매길때?

# 2. 서비스의 생성

서비스는 의존성 주입(Dependency Injection)이 가능한 클래스로 작성

```typescript
// greeting.service.ts
import { Injectable } from '@angular/core';

@Injectable()
export class GreetingService {
  sayHi() { return 'Hi!'; }
}
```

**서비스는 직접 모듈에 등록해야함**



```typescript
// app.component.ts
import { Component } from '@angular/core';
// 컴포넌트에서 사용할 서비스를 임포트
import { GreetingService } from './greeting.service';

@Component({
  selector: 'app-root',
  template: `
    <button (click)="sayHi()">Say Hi</button>
    <p>{{ greeting }}</p>
  `
})
export class AppComponent {
  greeting: string;
  // 서비스의 인스턴스를 담을 프로퍼티
  greetingService: GreetingService;

  constructor() {
    // 서비스의 인스턴스의 명시적 생성
    this.greetingService = new GreetingService();
  }

  sayHi() {
    // 서비스의 사용
    this.greeting = this.greetingService.sayHi();
  }
}
```



![dependency](http://poiemaweb.com/img/dependency.png)



컴포넌트가 Greeting 서비스와  **긴밀히 결합(Tight Coupling)**, 의존관계에 있기때문에 유지보수시에 비효율 적이다. 즉 A가 B의 메소드를 사용한다면 B의 메소드 형식이 변경되었을 때 A도 수정되어야 한다.

B는 A가없어도 동작하지만, A는 B가 없으면 동작하지않는다 === A는 B에 의존한다.



# 3. 의존성 주입(Dependency Injection)

의존성 주입(Dependency Injection, DI)은 구성 요소 간의 의존 관계를 코드 내부가 아닌 **외부의 설정파일 등을 통해 정의**하는 **디자인 패턴 중의 하나**로서 컴포넌트 간 **결합도를 낮추고 재사용성**을 높인다.



> 모듈에 프로바이더에 사용하고자하는 클래스타입을 지정해주고 그 클래스가 만들어지는 인스턴스를 constructor의 파라미터에 public으로 받으면 사용가능하다

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { GreetingService } from './greeting.service';

@Component({
  selector: 'app-root',
  template: `
    <button (click)="sayHi()">Say Hi</button>
    <p>{{ greeting }}</p>
  `,
  providers: [GreetingService]
  // 프로바이더를 컴포넌트 레벨로 등록하였기 때문에 GreetingService 타입의 AnotherGreetingService는 컴포넌트 레벨로 생성된다. 만약 모듈에 AnotherGreetingService를 등록하면 새로운 AnotherGreetingService 인스턴스를 생성하여 2개의 인스턴스가 존재하게 된다.
})
export class AppComponent {
  greeting: string;

  constructor(private greetingService: GreetingService) {}

  sayHi() {
    this.greeting = this.greetingService.sayHi();
  }
}
```



Angular가 인스턴스를 생성하고 컴포넌트에 전달하는 방식이다. 컴포넌트는 필요한 의존관계 객체를 요구하고, **프레임워크**는 **제어권(Control)을 갖는 주체로 동작**하여 요구된 **의존관계 객체의 인스턴스를 직접 생성하여 전달**한다. 이를 **제어권의 역전(Inversion of Control, IoC)**이라 한다.

Angular는 **설정 정보**에 의해 **인스턴스를 생성하여 컴포넌트에게 전달(주입, Inject)**해 줄 것이다. 이를 **의존성 주입(Dependency Injection, DI)**이라 한다.



C타입 클래스가 P타입클래스에게 상속되어있다고 하면... 

C는 P타입클래스로 불러질수 있다.



I타입 클래스로 만들어진 I1, I2, I3 인스턴스들은 I타입으로 불려질 수 있다.

# 4. 인젝터(Injector)

![Injector](http://poiemaweb.com/img/injector.png)

 Angular는 컴포넌트에 필요한 인스턴스를 인젝터에 요청한다. 인젝터는 이전에 이미 생성한 인스턴스를 담고 있는 컨테이너를 관리하고 있는데 요청된 인스턴스가 컨테이너에 존재하지 않으면 인스턴스를 생성하고 컨테이너에 추가한다. 그리고 요청된 인스턴스를 컴포넌트 생성자의 인자로 주입한다.



# 5. 인젝터 트리(Injector tree)

**컴포넌트는 트리 구조로 구성**된다. 컴포넌트는 각각 하나의 인젝터를 가지고 있기 때문에 **컴포넌트의 트리 구조**와 **동일**한 **인젝터 트리**가 만들어진다.

컴포넌트의 주입 요청이 있을 때 **인젝터**는 **현재 컴포넌트**에서 등록한 **프로바이더에서 주입 대상을 검색**한다. 이때 해당 컴포넌트의 프로바이더에서 주입 대상을 찾을 수 없으면 상위 컴포넌트의 프로바이더에서 주입 대상을 검색한다. 상위 컴포넌트의 프로바이더를 검색하여 주입 대상을 찾으면 해당 인젝터는 인스턴스를 생성하며 **상위 인젝터가 생성한 인스턴스는 하위 컴포넌트에서 사용할 수 있다.**

따라서 **인젝터 트리의 최상위 인젝터 즉 루트 인젝터**에서 **생성한 인스턴스**는 **전역에서 사용** 가능한 전역 서비스로 사용할 수 있다.

# 6. 프로바이더(Provider)

providers 프로퍼티는 모듈의 @NgModule 또는 컴포넌트의 @Component 어노테이션에 등록한다.

**모듈의 @NgModule**에 설정하면 모두가 사용가능한 싱글턴.

**컴포넌트의 @Component 어노테이션**에 설정하면 싱글턴이 아니고 자신을 포함한 자식만 사용가능함.

![싱글톤에 대한 이미지 검색결과](http://cfile26.uf.tistory.com/image/9953133359E2D96C231537)

싱글톤 



## 6.1 클래스 프로바이더(Class Provideer)

```typescript
@Component({
  ...
  // 주입할 인스턴스의 클래스 리스트 shorthand
  providers: [GreetingService]
  
  //   원래 표현
  providers: [{
    provide: GreetingService,
    useClass: GreetingService
  }]
})
```

GreetingService의 인스턴스를 컴포넌트가 직접 생성하지 않았다.



GreetingService를 쓰다가 수정할 일이 생기면  AnotherGreetingService로 쓰고 싶을때 useClass 부분만 바꾸면 된다.

```typescript
providers: [{
  // 의존성으로 주입될 객체의 타입(토큰, Token)
  provide: GreetingService,
  // 의존성으로 주입될 객체의 인스턴스를 생성할 클래스
  useClass: AnotherGreetingService
}]
```

덕 타이핑 : 구조가 같으면 다른 클래스라도 같은 타입으로 인정한다.



## 6.2 값 프로바이더(Value Provider)

값 프로바이더는 고정 값을 의존성 주입하기 위한 설정을 등록한다. 아래의 예제를 살펴보자. 주입될 클래스에 고정 값을 공급하고 있다.

```typescript
// app.config.ts
export class AppConfig {
  url: string;
  port: string;
}

// 공급 대상 고정 값
export const MY_APP_CONFIG: AppConfig = {
  url: 'http://somewhere.io/api',
  port: '5000'
};


// app.compoenet.ts
import { Component, Inject } from '@angular/core';

@Component({
  selector: 'app-root',
  template: '{{ myConfig }}',
  providers: [
    { provide: 'myConfig', useValue: 'Hello World' }
  ]
})
export class AppComponent {
  constructor(@Inject('myConfig') public myConfig: string) {
    console.log(myConfig); // Hello World
  }
}
```



## 6.3 팩토리 프로바이더(Factory Provider)

의존성을 생성할 때 어떠한 로직을 거쳐야 한다면 팩토리 함수를 사용한다. 예를 들어 조건을 인자로 받아 의존성을 생성하거나 여러 의존성 중에 어떤 것을 생성할 지 결정해야 하는 경우, 팩토리 함수를 사용한다.

개발 모드(isDev가 true)인 경우, MockUserService를 생성하고 그외의 경우 UserService를 생성하는 예제를 작성해 보자.

```typescript
// user.ts
export class User {
  constructor(public id: string, public password: string) {}
}

// user.service.ts
import { Injectable } from '@angular/core';
import { User } from './user';

@Injectable()
export class UserService {
  getUser(): User { return new User('real user', '123'); }
}

// mock-user.service.ts
import { Injectable } from '@angular/core';
import { User } from './user';

@Injectable()
export class MockUserService {
  getUser(): User { return new User('mock user', 'abc'); }
}

// user.service.provider.ts
import { MockUserService } from './mock-user.service';
import { UserService } from './user.service';

// 팩토리 함수
const userServiceFactory = (isDev: boolean) => {
  console.log(isDev);
  return isDev ? new MockUserService() : new UserService();
};

// 프로바이더
export const UserServiceProvider = {
  // 최종적으로 생성될 인스턴스의 타입
  provide: UserService,
  // 인스턴스 생성을 담당할 팩토리 함수
  useFactory: userServiceFactory,
  // 인스턴스 생성에 필요한 의존성 토큰
  deps: ['isDev']
};
```



## 6.4 인젝션 토큰(Injection Token)

지금까지 살펴본 예제는 문자열을 애플리케이션 공통 상수로 사용하는 경우를 제외하고 토큰으로 클래스를 사용하였다. 인젝션 토큰은 클래스가 아닌 의존성(non-class dependency), 예를 들어 객체, 문자열, 함수 등을 위한 토큰을 주입받기 위해 사용한다.

예를 들어 객체 리터럴로 작성된 애플리케이션 설정 정보를 주입받기 위해 프로바이더를 등록하여 보자.



```typescript
// app.config.ts
import { InjectionToken } from '@angular/core';

export interface AppConfig {
  url: string;
  port: string;
}

export const MY_APP_CONFIG: AppConfig = {
  url: 'http://somewhere.io/api',
  port: '5000'
};

// AppConfig 타입의 InjectionToken
export const APP_CONFIG = new InjectionToken<AppConfig>('app.config');

// Providers
export const AppConfigProvider = {
  provide: APP_CONFIG,
  useValue: MY_APP_CONFIG
};
```

InjectionToken 클래스를 사용하여 인터페이스 AppConfig 타입의 인젝션 토큰 APP_CONFIG를 생성하였다. InjectionToken 클래스의 생성자 인수는 개발자를 위한 설명(description) 문자열이다. InjectionToken 클래스는 클래스가 아닌 의존성(non-class dependency), 예를 들어 객체, 문자열, 함수 등을 위한 토큰을 생성한다. InjectionToken 클래스로 생성한 인젝션 토큰 APP_CONFIG를 인터페이스 대신 provide 프로퍼티에 등록한다.

```typescript
// app.component.ts
import { Component, Inject } from '@angular/core';
import { AppConfig, APP_CONFIG, AppConfigProvider } from './app.config';

@Component({
  selector: 'app-root',
  template: '{{ appConfig | json }}',
  providers: [ AppConfigProvider ]
})
export class AppComponent {

  constructor(@Inject(APP_CONFIG) public appConfig: AppConfig) {
    console.log(appConfig);
    // {url: "http://somewhere.io/api", port: "5000"}
  }
}
```

## 6.5 선택적 의존성 주입(Optional Dependency)

프로바이더 등록을 하지 않으면 의존성 주입은 실패하고 애플리케이션은 중단된다. @Optional 데코레이터를 사용하면 의존성 주입이 필수가 아닌 선택 사항임을 Angular에 알린다. 즉 주입받을 의존성이 없더라도 에러로 인해 애플리케이션이 중단되지 않는다. 사용 방법은 아래와 같다.

```typescript
// app.component.ts
import { Component, Optional } from '@angular/core';
import { GreetingService } from './greeting.service';

@Component({
  selector: 'app-root',
  template: '{{ greeting }}'
})
export class AppComponent {
  greeting: string;

  constructor(@Optional() public greetingService: GreetingService) {
    if (this.greetingService) {
      console.log(this.greetingService.sayHi());
      this.greeting = this.greetingService.sayHi();
    } else {
      console.log('Hi...');
      this.greeting = 'Hi...';
    }
  }
}
```

# 7. 서비스 중재자 패턴(Service Mediator Pattern)

컴포넌트는 *독립적인 존재이지만 다른 컴포넌트와 결합도를 낮게 유지하면서 상태 정보를 교환할 수 있어야* 한다. @Input, @Output 데코레이터를 사용하여 컴포넌트 간에 상태를 공유할 수 있지만 **원거리 컴포넌트 간의 상태 공유를 위해서 상태 공유가 필요없는 컴포넌트를 경유해야 하고 일관된 자료 구조가 존재하지 않기 때문에 개별적인 프로퍼티만을 교환할 수 밖에 없는 한계**가 있다. 이러한 경우, 컴포넌트 간 데이터 중개자로 서비스를 사용하면 일정한 형식의 자료 구조를 사용하면서 컴포넌트 간의 상태 공유가 가능하다.