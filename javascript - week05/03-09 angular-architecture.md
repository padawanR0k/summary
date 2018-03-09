# 1. Angular 애플리케이션의 파일 구조

Angular CLI를 사용하여 프로젝트를 생성하면 아래와 같은 파일 구조의 스캐폴딩이 생성된다.

```code
my-app/
├── .angular-cli.json
├── .editorconfig
├── .git/
├── .gitignore
├── e2e/
│   ├── app.e2e-spec.ts
│   ├── app.po.ts
│   └── tsconfig.e2e.json
├── karma.conf.js
├── node_modules/
├── package.json
├── protractor.conf.js
├── README.md
├── src/
│   ├── app/
│   │   ├── app.component.css
│   │   ├── app.component.html
│   │   ├── app.component.spec.ts
│   │   ├── app.component.ts
│   │   └── app.module.ts
│   ├── assets/
│   ├── environments/
│   │   ├── environment.prod.ts  // 환경변수지정 or ng build -prod (개발이 다 끝낫을때)
│   │   └── environment.ts
│   ├── favicon.ico
│   ├── index.html
│   ├── main.ts // angular가 동작할때 가장먼저 실행될 자바스크립트
│   ├── polyfills.ts
│   ├── styles.css
│   ├── test.ts
│   ├── tsconfig.app.json
│   ├── tsconfig.spec.json
│   └── typings.d.ts
├── tsconfig.json
└── tslint.json
```



## 1.1 src 폴더

`src` 폴더는 Angular의 모든 구성요소, 공통 CSS, 이미지, 설정 파일 등 애플리케이션 필수 파일을 담고 있다.

`src/app` 폴더에는 Angular 구성요소가 위치하게 된다. 현재는 컴포넌트와 모듈만 존재하지만 구성요소가 추가되면 이 폴더에 위치하게 된다. 개발자가 작성하는 대부분의 파일은 이곳에 포함된다.

- app/app.component.{ts, html, css, spec.ts}

  루트 컴포넌트를 구성하는 컴포넌트 클래스, HTML 템플릿, CSS, 유닛 테스트 파일.

- app/app.module.ts

  Angular 구성요소를 등록하는 루트 모듈.

- assets/*

  이미지 등과 같은 정적 파일을 위한 폴더. 프로젝트 생성 초기에는 빈 폴더이다.

- environments/*

  프로젝트 빌드 시에 사용될 프로덕션 또는 개발 환경 설정 파일.

- favicon.ico

  파비콘

- index.html

  웹 애플리케이션에 방문시 처음으로 로딩되는 디폴트 페이지. 루트 컴포넌트(/src/app/app.component.*)의 셀렉터인 <app-root>에 의해 루트 컴포넌트의 뷰가 로드되어 브라우저에 표시된다. 빌드 시에는 번들링된 JavaScript 파일이 자동 추가된 /dist/index.html이 생성된다.

![index.html](http://poiemaweb.com/img/index.html.png)

빌드 시에 index.html에 자동 추가되는 JavaScript 파일

- main.ts

  프로젝트의 메인 진입점. platformBrowserDynamic()에 의해 JIT 컴파일러가 실행되고 루트 모듈(AppModule)을 부트스트랩한다.

- polyfills.ts

  변화 감지(Change Detection)를 위한 zone.js와 ES6/ES7와 크로스 브라우저 웹 표준 지원을 위한 폴리필들을 임포트하는 역할을 한다. 자세한 내용은 [Browser support](https://angular.io/docs/ts/latest/guide/browser-support.html) 참조.

- styles.css

  애플리케이션 전역에 적용되는 글로벌 CSS.

- test.ts

  유닛 테스트를 위한 메인 진입점.

- tsconfig.{app|spec}.json

  TypeScript 컴파일 옵션 설정 파일.

- typings.d.ts

  TypeScript를 위한 타입 선언 파일.





# 2. Angular 애플리케이션의 처리 흐름

![angular-process](http://poiemaweb.com/img/angular-process.png)

app-root 부자관계

main.bundle.js 실행 => .angular-cli.json에서 설정확인 => main.ts 에서 루트 모듈 실행 => root 컴포넌트의 클래스를 가지고 객체를 생성 => app.components.ts 에서 html,css 실행 => app.component.html에 databinding => `<app-root></app-root>`에 코드 삽입됨



angular의 모듈은 es6모듈의 개념과 다르다.

## 2.1 index.html

웹 브라우저에 의해 **가장 먼저 로딩되는 프로젝트 파일은 /my-app/dist/index.html**이다. 이것은 **ng build 명령어로 프로젝트 빌드를 실행**하였을 때 /my-app/src/index.html에 번들링된 JavaScript 파일이 추가되어 **자동으로 생성되는 파일**이다.

## 2.2 main.ts

프로젝트의 메인 진입점이다. platformBrowserDynamic()에 의해 JIT 컴파일러가 실행되고 모든 모듈의 아버지인 루트 모듈(/src/app/app.module.ts)을 부트스트랩한다.

>  부트스트랩(*Bootstrap*)이란, 일반적으로 한 번 시작되면 알아서 진행되는 일련의 과정을 뜻한다. 

## 2.3 app.module.ts

@NgModule 데코레이터의 인자로 전달되는 메타데이터에 애플리케이션 전체의 설정 정보를 기술한 루트 모듈이다.

루트 모듈은 루트 컴포넌트 루트 컴포넌트(/src/app/app.component.ts)를 부트스트랩한다.



## 2.4 app.component.ts
모든 컴포넌트의 부모 역할을 담당하는 루트 컴포넌트이다.

my-app 프로젝트의 경우 /dist/index.html의 `<app-root>`에 의해 루트 컴포넌트의 뷰가 로드되어 `<app-root>`의 콘텐츠로 브라우저에 표시된다.



# 3. Angular의 구성 요소

#### 컴포넌트 (Component)

컴포넌트는 화면을 구성하는 **뷰(View)**를 생성하고 관리하는 것이 주된 역할이며 화면은 1개 이상의 컴포넌트를 조립하여 구성한다. 템플릿과 메타데이터, 컴포넌트 클래스로 구성되며 데이터 바인딩에 의해 연결된다.



#### 디렉티브 (Directive)

애플리케이션 전역에서 사용할 수 있는 공통 관심사를 컴포넌트에서 분리하여 구현한 것으로 컴포넌트의 복잡도를 낮추고 가독성을 향상시킨다. 컴포넌트도 뷰를 생성하고 이벤트를 처리하는 등 DOM을 관리하기 때문에 큰 의미에서 디렉티브로 볼 수 있다. 구조 디렉티브(Structural directive)와 어트리뷰트 디렉티브(Attribute directive), 커스텀 디렉티브(Cunstom directive)로 구분할 수 있다.



#### 서비스 (Service)

다양한 목적의 애플리케이션 공통 로직을 담당한다. 컴포넌트에서 애플리케이션 전역 관심사를 분리하기 위해 사용하며 의존성 주입(Dependency Injection)이 가능한 클래스로 작성된다.



#### 라우터(Router)

컴포넌트를 교체하는 방법으로 뷰를 전환하여 화면 간 이동을 구현한다.



#### 모듈 (NgModule)

기능적으로 관련된 구성요소(컴포넌트, 디렉티브, 파이프, 서비스)를 하나의 단위로 묶는 메커니즘을 말한다. **모듈은 관련이 있는 기능들이 응집된 기능 블록으로 애플리케이션을 구성하는 하나의 단위를 만든다**. 모듈은 다른 모듈과 결합할 수 있으며 Angular는 여러 모듈들을 조합하여 애플리케이션을 구성한다. **컴포넌트, 디렉티브, 파이프, 서비스 등의 Angular의 구성요소는 모듈에 등록되어야 사용할 수 있다.**

![angular-archtecture](http://poiemaweb.com/img/angular-archtecture.png)