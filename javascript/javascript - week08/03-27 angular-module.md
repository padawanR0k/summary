파일을 구분할때 특성에 따라 폴더를 만들어 구분시켜 놓듯, 앵귤러를 개발할때에도 파일의 구분이 필요하다.

es6 모듈은 export, import으로만 이루어져있다. 앵귤러에서는 



# 1. 모듈(NgModule)이란?

**Angular의 모듈**(NgModule)은 관련이 있는 구성요소(컴포넌트, 디렉티브, 파이프, 서비스)를 하나의 단위로 묶는 메커니즘을 말한다.

모듈은 관련이있는 기능블록이라고 생각하면된다. 이런 모듈들이 모여 하나의 애플리케이션을 만든다. 모듈은 레고처럼 쉽게 재사용가능해야한다.

루트컴포넌트들은 모든 컴포넌트들의 부모이듯 루트 모듈은 모든 모듈의 부모이다.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
 // '@angular/' 는 앵귤러 라이브러리모듈을 의미한다. 앵귤러 라이브러리 모듈은 경로를 굳이 안써줘도 자동적으로 연결이 된다.


import { AppComponent } from './app.component'; // AppComponent라는 이름의 class를 import함 
import { SomeDirective } from './some.directive';
import { SomeComponent } from './some/some.component';
import { SomePipe } from './some.pipe';
import { SomeService } from './some.service';

@NgModule({ // 데코레이터는 뒤에 따라오는 class를 수식한다. 그러므로 데코레이터 바로뒤에는 class가 와야하며 반드시 호출 () 되야함. 데코레이터는 인자로 앵귤러를 위한 metadate를 받는다.
  declarations: [ // 컴포넌트, 파이프, 디렉티브를 정의
    AppComponent,
    SomeDirective,
    SomeComponent,
    SomePipe
  ],
  imports: [BrowserModule], // 모듈을 import한다.
  providers: [SomeService], // service가 와야한다. service는 컴포넌트에 등록하면 컴포넌트 레벨에서 service를 생성하고 모듈에 등록하면 모듈레벨에서 service를 생성한다. 모듈레벨로 등록된 service는 전역에서 사용가능해진다.
  bootstrap: [AppComponent] // css프레임워크가 아니라 모듈이 실행이될때 활성화되는 컴포넌트 
}) // 데코레이터는 앵귤러가 클래스로 인스턴스를 생성할때 constructor를 수정하기위해 필요하다. 
export class AppModule { }
```

- BrowserModeul
  - 루트모듈만 쓴다. 만약 웹 애플리케이션을 만드려고하면 반드시 필요한 모듈이다.
- NgModule
  - 데코레이터를 쓰기위한 모듈 



애플리케이션 개발에 있어서 모듈성(Modularity)은 중요한 의미를 갖는다. 코드의 복잡도가 커짐에 따라 **루트** 모듈(Root module), **기능** 모듈(Featue module), **공유** 모듈(Shared module), **핵심** 모듈(Core module)로 모듈을 분리하여 애플리케이션을 구성한다. 

# 2. @NgModule 데코레이터

데코레이터는 함수이며 모듈의 설정 정보가 기술된 메타데이터 객체를 인자로 전달받아 모듈을 생성한다. 

| 프로퍼티     | 내용                                                         |
| ------------ | ------------------------------------------------------------ |
| providers    | 주입 가능한 객체(injectable object) 즉 서비스의 리스트를 선언한다. 루트 모듈에 선언된 서비스는 애플리케이션 전역에서 사용할 수 있다. |
| declarations | 컴포넌트, 디렉티브, 파이프의 리스트를 선언한다. 모듈에 선언된 구성 요소는 모듈에서 사용할 수 있다. |
| imports      | 의존 관계에 있는 Angular 라이브러리 모듈, 기능 모듈(Feature module)이라 불리는 하위 모듈, 라우팅 모듈, 서드 파티 모듈을 선언한다. |
| bootstrap    | 루트 모듈에서 사용하는 프로퍼티로서 애플리케이션의 진입점(entry point)인 루트 컴포넌트를 선언한다. |

# 3. 라이브러리 모듈

라이브러리 모듈은 Angular가 제공하는 빌트인 모듈이다.

```json
// package.json 파일 내부
"dependencies": {
    "@angular/animations": "^5.2.0",
    "@angular/common": "^5.2.0",
    "@angular/compiler": "^5.2.0",
    "@angular/core": "^5.2.0",
    "@angular/forms": "^5.2.0",
    "@angular/http": "^5.2.0",
    "@angular/platform-browser": "^5.2.0",
    "@angular/platform-browser-dynamic": "^5.2.0",
    "@angular/router": "^5.2.0",
    "core-js": "^2.4.1",
    "rxjs": "^5.5.6",
    "zone.js": "^0.8.19"
  },
```

라이브러리 모듈 패키지에서 필요한 모듈만을 선택하여 임포트한다. 예를 들어 @angular/platform-browser 패키지에서 BrowserModule 모듈을 임포트하는 경우, 아래와 같이 기술한다.

```typescript
// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
```





# 4. 루트 모듈

Angular 애플리케이션은 적어도 하나 이상의 모듈을 소유하여야 한다. 루트 모듈은 애플리케이션의 최상위에 존재하는 유일한 모듈로 애플리케이션 레벨의 컴포넌트, 디렉티브, 파이프, 서비스를 선언하거나 의존 라이브러리 모듈과 기능 모듈(Feature module)이라 불리는 하위 모듈을 포함(import)할 수 있다.



BrowserModule은 NgIf 및 NgFor와 같은 빌트인 디렉티브와 파이프를 포함하는 CommonModule을 내부에서 import한다. root모듈에서는 BrowserModule을 반드시 import해야하고 root모듈을 제외한 모듈은 CommonModule을 import해야한다.

![angular-process](http://poiemaweb.com/img/angular-process.png)

# 5. 모듈의 분리

애플리케이션이 커짐에 따라 루트 모듈에 등록된 컨포넌트, 디렉티브, 파이프, 서비스도 늘어나게 된다. 이떄 기능모듈, 핵심모듈, 공유모듈로 분리하면 모듈의 관리가 쉬워지고 이름이 중복하는 가능성 또한 작아진다.

| 모듈      | 개요                                                         | 대상                                                         |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 기능 모듈 | 관심사가 유사한 구성 요소로 구성한 모듈                      | 특정 화면을 구성하는 구성 요소. 기능모듈 하나가 하나의 화면이다. |
| 공유 모듈 | 애플리케이션 전역에서 사용될 구성 요소들로 구성한 모듈로서 기능 모듈에 의해 임포트된다. | 애플리케이션 전역에서 사용하는 컴포넌트, 디렉티브, 파이프 등 |
| 핵심 모듈 | 애플리케이션 전역에서 사용될 구성 요소들로 구성한 모듈로서 루트 모듈에 등록하여 **싱글턴으로 사용한다.** | 애플리케이션 전역에서 사용하는 데이터 서비스, 인증 서비스, 인증 가드 등 |

핵심 모듈 > 공유 모듈 > 기능 모듈

핵심 모듈은 모든곳에서 쓰이는 Service

공유 모듈은 특정화면에서 쓰이는 Component, Directive, pipe



##  5.1 기능 모듈(Feature module)

기능 모듈은 관심사가 유사한 구성 요소로 구성한 모듈이다. **일반적으로 기능 모듈은 특정 화면 단위를 기준으로 구성한다.** 기능 모듈은 루트 모듈과 마찬가지로 @NgModule 데코레이터와 메터데이터로 구성한다.

위의 예제에서 home.component.ts는 home 페이지를 위한 컴포넌트로서 사용자의 정보를 표시한다. 이 컴포넌트는 특정 화면을 담당하므로 기능 모듈로 분리할 수 있다. 

```typescript
// home/home.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

/* HomeComponent 임포트 */
import { HomeComponent } from './home.component';

@NgModule({
  imports: [CommonModule],
  declarations: [HomeComponent], /* HomeComponent 선언 */
  providers: [],
  exports: [HomeComponent] /* HomeComponent root컴포넌트에 공개 */
})
export class HomeModule { }
```



```typescript
// app.component.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

/* HomeModule 임포트 */
import { HomeModule } from './home/home.module'; // 컴포넌트를 지우고 모듈을 import

import { AppComponent } from './app.component';
import { HeaderComponent } from './header/header.component';

import { UserService } from './user.service';

@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent
  ],
  imports: [
    BrowserModule,
    HomeModule // 컴포넌트를 지우고 모듈을 import
  ],
  providers: [UserService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```



##  5.2 공유 모듈(Shared module)

공유 모듈은 애플리케이션 전역에서 사용될 구성 요소들로 구성한 모듈로서 다른 모듈(주로 기능 모듈)에서 공통적으로 사용된다. 

**루트 모듈은 기능 모듈을 임포트하고 기능 모듈은 공유 모듈을 임포트하여 사용**한다. 이렇게 **모듈을 구성**하여 **기능 모듈의 중복을 제거**하여 모듈 선언을 간소화한다. 다시 말해 **공유 모듈**은 루트 모듈에 직접 임포트되지 않고 **기능 모듈에 의해 임포트되어 사용**된다.

![Shared module](http://poiemaweb.com/img/shared-module.png)

```typescript
// shared/shared.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

/* HeaderComponent 임포트 */
import { HeaderComponent } from './header.component';

@NgModule({
  imports: [CommonModule],
  declarations: [HeaderComponent], /* HeaderComponent 선언 */
  providers: [],
  exports: [HeaderComponent] /* HeaderComponent 공개 */
})
export class SharedModule { }

// app.component.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

/* HomeModule 임포트 */
import { HomeModule } from './home/home.module';
/* SharedModule 임포트 */
import { SharedModule } from './shared/shared.module';

import { AppComponent } from './app.component';
import { UserService } from './user.service';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    HomeModule,  /* HomeModule 임포트 */
    SharedModule /* SharedModule 임포트 */
  ],
  providers: [UserService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```



##  5.2 핵심 모듈(Core module)

핵심 모듈은 애플리케이션 **전역에서 사용될 구성 요소들로 구성한 모듈로서 루트 모듈에 등록**한다. 애플리케이션 전역에서 사용된다는 의미에서 공**유 모듈과 유사하지만** 핵심 모듈은 **루트 모듈에 등록하여 싱글턴으로 사용**하고 **공유 모듈은 기능 모듈에 의해 사용**된다. 예를 들어 애플리케이션 전역에서 사용하는 **데이터 서비스, 인증 서비스, 인증 가드 등이 대상**이 된다.

```typescript
// core.module.ts
import { NgModule } from '@angular/core';

/* UserService 임포트 */
import { UserService } from './user.service';

@NgModule({
  imports: [],
  declarations: [],
  providers: [UserService], /* UserService 제공 */
  exports: []
})
export class CoreModule { }


// app.component.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

/* HomeModule 임포트 */
import { HomeModule } from './home/home.module';
/* SharedModule 임포트 */
import { SharedModule } from './shared/shared.module';
/* CoreModule 임포트 */
import { CoreModule } from './core/core.module';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    HomeModule,   /* HomeModule 임포트 */
    SharedModule, /* SharedModule 임포트 */
    CoreModule    /* CoreModule 임포트 */
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

```code
src
└── app
  ├── core
  │   ├── core.module.ts   # 핵심 모듈
  │   └── user.service.ts
  ├── home
  │   ├── home.component.ts
  │   └── home.module.ts   # 기능 모듈
  ├── shared
  │   ├── header.component.ts
  │   └── shared.module.ts # 공유 모듈
  ├── app.component.ts     # 루트 컴포넌트
  ├── app.module.ts        # 루트 모듈
  └── user.ts
```



# ++

요구사항 => 해결방안 => UI => **설계** => 구현