ajax통신을 통하여 todos데이터를 변경했다.

GET 방식으로 데이터를 가져옴 (url경로) 

Ajax통신은 주소창의 url을 바꾸지 않는다. 그래서 브라우저에 의한 화면갱신이 일어나지 않는다. 그러다보니 SEO에 문제가 생긴다.

SEO문제를 해결하면서 화면갱신이 발생하지않는 문제도 해결한 것이 **Angular**





![angular logo](http://poiemaweb.com/img/angular-logo.png)



# 1. Angular 소개

**SPA(Single Page Application)** 개발을 위한 구글의 오픈소스 자바스크립트 프레임워크이다. 웹 애플리케이션, 모바일 웹, 네이티브 모바일과 데스크탑 애플리케이션까지 프론트엔드 개발에 필요한 대부분의 기능을 갖추고 있다.

모바일 애플리케이션을 만들때는 [아이오닉](http://webframeworks.kr/getstarted/ionic/)을 쓰고 [데스크탑 앱](https://electronjs.org/)은 일렉트론을 쓴다.

초기에는 매우 어려웠으나 현재 수많은 버전업을 통해 많이 달라졌다.

- html안에 템플릿문법을 쓸 수있게되어 .ts파일 내부 로직을 볼 수 있게되므로 javascript가 html에 종소돼지 않는다.
- 변화감지



# 2. Angular와 AngularJS의 차이점

Angular는 AngulaJS의 단순한 업그레이드 버전이 아니다. Angular는 정적 타이핑과 ECMAScript6 스펙을 충족시키기 위해 **TypeScript로 재작성되었고 AngulaJS와는 호환성이 없는 브레이킹 체인지를 다수 포함하고 있다.**

AngularJs는 MVC ( Model View Controller)

Angular는 CBD( Component Based Development) 

- 선택적 바인딩(one-way, two-way **Angular는 더 이상 양방향 데이터 바인딩을 빌트인으로 제공하지 않는다*.)을 지원하고 디렉티브(Directive)와 서비스, 의존성 주입(dependency injection)은 간소화 되었다.
- 주력 개발 언어로써 TypeScript를 도입하여 대규모 개발에 적합한 정적 타입과 인터페이스, 제네릭 등 타입 체크 지원 기능을 제공한다.
- ECMAScript6에서 새롭게 도입된 모듈, 클래스 등과 ECMAScript7의 데코레이터를 지원한다.
- 강력한 개발환경 지원 도구인 Angular CLI를 제공한다.

# 3. Angular의 장점



## 3.1 개선된 개발 생산성



###  3.1.1 컴포넌트 기반 개발

AngularJS의 경우 Controller와 $scope가 개발의 중심이었지만 Angular에서는 **컴포넌트가 개발의 중심이다.**

###  3.1.2 TypeScript의 도입

TypeScript를 사용하는 이유는 여러가지 있지만 가장 큰 장점은 다양한 도구의 지원을 받을 수 있다는 것이다. TypeScript는 **정적 타이핑을 지원하므로 높은 수준의 IntelliSense, 코드 어시스트, 타입 체크, 리팩토링 등을 지원**하며 이러한 도구의 지원은 대규모 프로젝트를 위한 필수적 요소이기도 하다.

TypeScript의 모듈, 클래스, 인터페이스 등의 강력한 Object Oriented Programming 지원은 크고 복잡한 프로젝트의 코드 기반을 쉽게 구성할 수 있도록 돕는다.

Angular는 TypeScript 뿐만 아니라 JavaScript, Dart로도 작성할 수 있다.

###  3.1.3 개발 도구의 통합 및 개발 환경 구축 자동화

Angular는 **Angular CLI**를 통해 간편한 개발 환경 구축을 지원한다. 간단한 명령어를 사용하여 간편하게 프로젝트 생성에서 빌드, 테스트, 구성요소 추가를 할 수 있으며 개발용 서버를 내장하고 있어 실행까지 가능하다. 이것은 개발환경 구축에 소요되는 시간을 최소화할 수 있어서 개발작업에 집중할 수 있도록 돕는다.

## 3.2 성능의 향상

1. Aot 컴파일 ( ahead of time compilation )
   - 예를 들어 ngIf, ngFor, ngSwitch와 같은 구조 디렉티브(Structural directive)를 브라우저가 실행 가능한 코드로 변환하여야 하는데 이러한 과정을 런타임에서 실시하지 않고 사전에 컴파일하여 실행 속도를 향상시키는 기법이다
2. Lazy Loading
   - 필요한 시점에 필요한 모듈만 로딩하는 방식. 불필요한 모듈을 로딩하는 리소스 낭비를 줄일 수  있다.
3. 코드 최적화
   - Angular 코드 자체도 지속적인 최적화가 수행되고 있어 45KB 정도의 크기로 축소되었다고 한다.(ng-conf 2016 
   - 기준) **Angular는 Mobile First를 지향하는 고성능 프레임워크를 표방**하고 있기 때문에 지속적인 코드 최적화가 진행될 것으로 예상된다.

# 4. 브라우저 지원 범위

Angular는 대부분의 모던 브라우저를 지원한다. IE의 경우, 9 이상을 지원한다.

| Chrome | Firefox | Edge | IE   | Safari | iOS  | Android                      | IE Mobile |
| ------ | ------- | ---- | ---- | ------ | ---- | ---------------------------- | --------- |
| latest | latest  | 14   | 11   | 10     | 10   | Nougat(7.0) Marshmallow(6.0) | 11        |
|        |         | 13   | 10   | 9      | 9    | Lollipop(5.0, 5.1)           |           |
|        |         |      | 9    | 8      | 8    | KitKat(4.4)                  |           |
|        |         |      |      | 7      | 7    | Jelly Bean(4.1, 4.2, 4.3)    |           |

