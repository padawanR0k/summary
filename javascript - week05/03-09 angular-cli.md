# 1. Angular CLI란?

 간단한 명령어를 사용하여 Angular 프로젝트 스캐폴딩(scaffolding)을 생성, 실행, 빌드할 수 있으며 다양한 구성 요소를 선별적으로 추가할 수 있는 커맨드-라인 인터페이스이다. **개발용 서버를 내장**하고 있어서 간단히 프로젝트를 실행시켜서 동작을 확인할 수 있다.



Angular CLI가 지원하는 기능은 아래와 같다.

- Angular 프로젝트 생성
- Angular 프로젝트에 컴포넌트, 디렉티브, 파이프, 서비스, 클래스, 인터페이스 등의 구성 요소 추가
- LiveReload를 지원하는 내장 개발 서버를 사용한 Angular 프로젝트 실행
- Unit (하나하나 테스트하는 것)/E2E(end-to-end) 테스트 환경 지원 (실제유저처럼 테스트)
- 배포를 위한 Angular 프로젝트 패키징

<https://cli.angular.io/>

# 2. Angular CLI 설치

```bash
$ npm install -g @angular/cli
```



# 3. 프로젝트 생성

```bash
$ ng new <project-name>
```



기본 애플리케이션 구조

```code
my-app/
├── .git/
├── e2e/
├── node_modules/
├── src/
├── `.angular-cli.json`
├── .editorconfig
├── .gitignore
├── karma.conf.js
├── package.json
├── protractor.conf.js
├── README.md
├── tsconfig.json
└── tslint.json
```



# 4. 프로젝트 실행

프로젝트를 로컬 환경에서 실행(preview)하기 위해서는 `ng serve` 명령어를 사용한다.

```bash
$ cd <project-name>
$ ng serve
```

이미 포트 4200번을 사용하고 있다면 Angular CLI 내장 서버를 실행할 수 없다. 포트번호를 변경하고자는 경우에는 다음과 같이 `--port`(축약형 -p) 옵션을 추가한다.

```bash
$ ng serve --port 4201
```



# 5. 프로젝트 구성 요소 추가

프로젝트에 새로운 구성요소를 추가하기 위해서는 `ng generate` 명령어를 사용한다. `ng generate` 명령어는 축약형 `ng g`와 동일하게 동작한다.

| 추가 대상 구성요소 | 명령어                               | 축약형                | 결과                               |
| ------------------ | ------------------------------------ | --------------------- | ---------------------------------- |
| 컴포넌트           | ng generate component component-name | ng g c component-name | app/ 밑에 새로운 컴포넌트폴더 생성 |
| 디렉티브           | ng generate directive directive-name | ng g d directive-name |                                    |
| 파이프             | ng generate pipe pipe-name           | ng g p pipe-name      |                                    |
| 서비스             | ng generate service service-name     | ng g s service-name   |                                    |
| 모듈               | ng generate module module-name       | ng g m module-name    |                                    |
| 가드               | ng generate guard guard-name         | ng g g guard-name     |                                    |
| 클래스             | ng generate class class-name         | ng g cl class-name    |                                    |
| 인터페이스         | ng generate interface interface-name | ng g i interface-name |                                    |
| Enum               | ng generate enum enum-name           | ng g e enum-name      |                                    |

###  *.components.ts

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-home', // 태그명
  templateUrl: './home.component.html', // 상대위치 지정
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

```



새로운 컴포넌트를 추가하면 루트에 모듈파일이 생긴다. 그 모듈이 하위 모듈을 통합해서 관리한다.

각각의 컴포넌트의 css는 지역의 스코프를 가진다.

전역CSS는 루트의 styles.css 에서 작성한다.

### 파일의 암묵적 변경

주의해야 할 것은 `ng generate component`명령어 다음에 지정한 컴포넌트명이 실제 생성된 파일명과 다를 수 있다는 것이다. 예를 들어 아래와 같이 새로운 컴포넌트를 추가해 보자. 

```bash                                                                                                                       
$ ng g c newComponent
$ ng g c NewComponent
$ ng g c new-component

```

이와 같은 파일명의 암묵적 변경은 컴포넌트뿐만이 아니라 `ng generate` 명령어로 추가되는 모든 구성요소에 모두 적용된다. 혼란을 방지하는 위해 `ng generate` 명령어에 지정하는 구성요소 명칭은 하이픈으로 구별된 케밥 표기법(kebab-case) 명칭을 사용하는 것이 좋다.



### selector 프로퍼티값의 접두사(prefix)와 컴포넌트 클래스 이름

```typescript
// src/app/home/home.component.ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-home', // 셀렉터의 이름은 프로젝트의 이름을 붙여준다. 
  // 기본적으로는 app- 를 붙여준다.
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor() { }

  ngOnInit() { }
}
```

selector 프로퍼티값 ‘app-home’는 `ng generate component home` 명령어에서 지정한 컴포넌트명 `home` 앞에 접두사(prefix) app이 자동으로 추가된 값이다. Angular는 다른 애플리케이션의 `selector`또는 HTML 요소와 충돌을 방지하기 위해 접두사를 추가하여 케밥 표기법으로 명명하는 것을 권장하고 있다. 

```JSON
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "project": {
    "name": "my-app"
  },
  "apps": [
    {
      ...
      "prefix": "app",
      ...
    }
  ],
  ...
}

```

`.angular-cli.json`의 prefix 프로퍼티값을 수정하면 이후 생성되는 컴포넌트의 셀렉터 접두사는 수정된 값으로 변경된다. 프로젝트 생성 단계에서부터 컴포넌트의 기본 셀렉터 접두사를 변경하고 싶은 경우에는 `ng new` 명령어로 프로젝트 생성 시에 `--prefix` 옵션을 추가한다.

### templateUrl, styleUrls 프로퍼티와 template, styles 프로퍼티

`templateUrl`, `styleUrls` 프로퍼티는 외부 파일을 로드하기 위해 사용한다.

- - templateUrl

    외부 파일로 작성된 HTML 템플릿(컴포넌트의 뷰를 정의)의 경로

- - styleUrls

    외부 파일로 작성된 CSS 파일의 경로

```typescript
// src/app/home/home.component.ts
...
@Component({
  selector: 'app-home', // 파일이 태그로 사용될것이다.'
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
// 이 뒤에올 컴퍼넌트는 앵귤러가 생성하는데 @compononet 객체를 보고 생성한다.
// 객채생성할때 필요한 데이터만 넘겨주면 된다.
...
```



```typescript
// src/app/home/home.component.ts
...
@Component({
  selector: 'app-home',
  template: `
    <p>home works!</p>
  `,
  styles: [`
    p { color: red; }
  `]
})
...

```

`ng generate component` 명령어를 사용하여 컴포넌트를 추가할 때 HTML 템플릿과 CSS를 외부 파일로 생성하지 않고 인라인 HTML 템플릿과 CSS를 사용하고자 하는 경우에는 아래의 명령어를 사용한다.

```typescript
# 인라인 HTML 템플릿을 사용하는 경우
$ ng g c home --inline-template
# 인라인 CSS를 사용하는 경우
$ ng g c home --inline-style
# 인라인 HTML 템플릿과 인라인 CSS를 사용하는 경우
$ ng g c home --inline-template --inline-style
```





# 6. 프로젝트 빌드



## 6.1 트랜스파일링과 의존 모듈 번들링

TypeScript 기반으로 개발이 진행되는 Angular 애플리케이션은 TypeScript를 JavaScript로 변환하여야 한다. 또한 의존 모듈의 해결이 필요한데 수작업으로 프로젝트 의존 모듈을 HTML의 script 태그에 기술하는 것은 매우 곤란한 일이다.

Angular CLI 빌드 기능은 내부적으로 모듈 번들러 [webpack](https://webpack.github.io/)을 사용하며 아래와 같은 작업의 자동화를 지원한다.

- TypeScript에서 JavaScript로의 트랜스파일링
- 디버깅 용도의 map 파일 생성
- 의존 모듈과 HTML, CSS, JavaScript 번들링
- AoT 컴파일
- 소스코드의 문법 체크
- 코드 규약 준수 여부 체크
- 불필요한 코드의 삭제 및 압축





```html
빌드 이전과 빌드 이후의 index.html을 비교하여 보자.

<!-- src/index.html -->
...
<body>
  <app-root></app-root>
</body>
</html>




<!-- dist/index.html -->
...
<body>
  <app-root></app-root>
<script type="text/javascript" src="inline.bundle.js"></script><script type="text/javascript" src="polyfills.bundle.js"></script><script type="text/javascript" src="styles.bundle.js"></script><script type="text/javascript" src="vendor.bundle.js"></script><script type="text/javascript" src="main.bundle.js"></script></body>
</html>
```

![build-dist](http://poiemaweb.com/img/build-dist.png)styles.bundle.js 

- CSS가 자바스크립트안에서 작동한다.

polyfills.bundle.js

- 호환성을 위한 폴리필들이 주석처리되어있다.

vendor.bundle.js

- 외부 라이브러리 번들

# 