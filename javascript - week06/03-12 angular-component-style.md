각 컴포넌트마다 지역의 스코프를 갖는 CSS가 있다. 어떻게 가능하게 하는 것인가?

# 1. 컴포넌트 스타일

Angular 컴포넌트는 동작 가능한 하나의 부품으로 다른 컴포넌트에 간섭을 받지 않는 독립된 스코프의 스타일 정보를 갖는다.

```typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h3>Component Style: Parent</h3>
    <button class="btn-primary">Button</button>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent {}
```

```css
/* app.component.css */
h3 {
  color: dimgray;
}
.btn-primary {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}
```

부모 컴포넌트t에는 스타일을 정의하였으나 자식 컴포넌트에는 아무런 스타일도 정의하지 않았다. 이때 **부모 컴포넌트의 스타일은 자식 컴포넌트에 어떠한 영향을 주지 않는다.**

# 2. 뷰 캡슐화 (View Encapsulation)



![Encapsulation](http://poiemaweb.com/img/encapsulation.png)

 Angular가 임의로 추가한 어트리뷰트 = [_ngcontent-c1]

 어트리뷰트 셀렉터를 추가하는 방식으로 해당 컴포넌트를 스코프로 한정하여 스타일이 적용될 수 있도록 한다.

Angular는 컴포넌트의 CSS 스타일을 컴포넌트의 뷰에 캡슐화하여 다른 컴포넌트에는 영향을 주지 않는다. 위의 경우와 같이 **Angular는 기본적으로 임의의 어트리뷰트를 추가하는 방식(Emulated)**을 사용하여 뷰 캡슐화를 구현하지만 브라우저가 웹 컴포넌트를 지원한다는 전제 하에 웹 컴포넌트의 **Shadow DOM을 이용하여 뷰 캡슐화를 구현**할 수도 있다.

이를 위해 **@Component 메타데이터 객체**에 `encapsulation` 프로퍼티에 V**iewEncapsulation 옵션을 지정하여 컴포넌트 별로 뷰 캡슐화 전략을 설정**할 수 있다. ViewEncapsulation은 열거형으로 아래의 3가지 캡슐화 전략을 제공한다.

| ViewEncapsulation | 의미                                                         |
| ----------------- | ------------------------------------------------------------ |
| Emulated          | 임의의 어트리뷰트를 추가하는 브라우저의 기본 Shadow DOM 구현 방식이다. 컴포넌트의 스타일은 해당 컴포넌트에만 적용된다. (기본 전략) |
| Native            | 웹 컴포넌트의 Shadow DOM을 사용하는 방식이다. 컴포넌트의 스타일은 해당 컴포넌트에만 적용된다. => 표준 방식을 따른다. (현재 지원안하는 브라우저 있음) |
| None              | 스타일 캡슐화를 지원하지 않는다. 컴포넌트의 CSS는 전역에 지정되어 다른 다른 컴포넌트에 영향을 준다. |

```typescript
// app.component.ts
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h3>Component Style: Parent</h3>
    <button class="btn-primary">Button</button>
    <app-child></app-child>
  `,
  styleUrls: ['./app.component.css'],
  encapsulation: ViewEncapsulation.Native // 익스를 지원안할꺼면 Native 쓴다 
})
export class AppComponent {}
```



# 3. 쉐도우 DOM 스타일 셀렉터 (Shadow DOM Style Selector)

| 쉐도우 DOM 스타일 셀렉터 | 의미                                                         |
| ------------------------ | ------------------------------------------------------------ |
| :host                    | 호스트 요소(컴포넌트 자신)을 선택한다.                       |
| :host-context            | 호스트 요소의 외부(예를 들어 body)의 조건에 의해 컴포넌트의 요소를 선택한다. |

###### host 요소란? 

컴포넌트의 셀럭터에  CSS를 부여할때 필요함

## 3.1 :host 셀렉터

```typescript
// app.component.ts
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h3>Component Style: Parent</h3>
    <button class="btn-primary">Button</button>
    <app-child></app-child>
  `,
  styleUrls: ['./app.component.css'],
  encapsulation: ViewEncapsulation.Native // 익스를 지원안할꺼면 Native 쓴다 
})
export class AppComponent {}
```



```css
/* app.component.css */
/* 호스트 요소 <app-root>에 대해 적용된다 */
:host {
  display: block;
  background-color: lightgray;
  border: 1px solid dimgray;
  padding: 20px;
}

h3 {
  color: dimgray;
}

.btn-primary {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}
```

`<app-root></app-root>` 에 :host 로 선택한 CSS가 적용된다.  참고로 **앵귤러가 만들어준 요소들은 기본 CSS가 하나도 적용이 안되있다. display조차도.**

## 3.2 :host-context 셀렉터

호스트 요소의 외부의 조건 즉 부모 요소를 포함하는 조상 요소의 클래스 선언 상태에 의해 컴포넌트의 요소를 선택하는 경우 사용한다. 

```typescript
// app.component.ts
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h3>Component Style: Parent</h3>
    <button class="btn-primary">Button</button>
    <div class="theme-red">
      <app-child class="active"></app-child>
    </div>
  `,
  styleUrls: ['./app.component.css'],
  encapsulation: ViewEncapsulation.Native
})
export class AppComponent {}
```

```css
/* child.component.css */
/* 호스트 요소 <app-child>에 대해 적용된다 */
:host {
  display: block;
}

/* 호스트 요소에 active class가 선언되어 있을 때 적용된다. */
:host(.active) {
  background-color: lightcyan;
}

/* 호스트 요소가 hover 상태일 때 적용된다. */
/* :click, :active도 사용할 수 있다. */
:host(:hover) {
  background-color: royalblue;
}

h3 {
  color: deepskyblue;
}

/* 컴포넌트의 조상 요소에 theme-red 클래스가 선언되어 있을 때 적용된다. */
:host-context(.theme-red) .btn-primary { 
  color: #fff;
  background-color: crimson;
  border-color: crimson;
}
:host-context(.theme-blue) .btn-primary {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}
```



조상요소들중 특정 조건에 맞는다면 CSS를 적용시킴



# 4. 글로벌 스타일

애플리케이션 전역에 적용되는 글로벌 스타일을 적용하려면 **src/styles.css**에 CSS 룰셋을 정의한다. 또는 .angular-cli.json 파일의 apps.styles 프로퍼티에 CSS 파일의 경로를 추가한다.

```json
{
  ...
  "apps": [
    {
      ...
      "styles": [
        "styles.css",
        "another-global.css" <---
      ],
```



# 5. Angular CLI로 Sass 적용 프로젝트 생성

```bash
$ ng new sass-project --style=scss
```

이때 생성된 .angular-cli.json 파일을 살펴보면 apps.styles 프로퍼티와 defaults.styleExt 프로퍼티의 값이 scss로 변경된 것을 알 수 있다.

```json
{
  ...
  "apps": [
    ...
    "styles": [
      "styles.scss"
    ],
  ...
  "defaults": {
    "styleExt": "scss",
    "component": {}
  }
}
```



# 6. Angular Material

[Getting started](https://material.angular.io/guide/getting-started)