# 1. 데이터 바인딩(Data binding)이란?

*관심사가 다른* 뷰와 모델을 분리시켜야 **구조화된 웹앱**을 만들 수 있다. 구조화된 웹앱은 유지보수성을 상당히 높여준다. 하지만 뷰와 모델은 유기적으로 (자주 변화하는) 동작하여야한다. 데이터 바인딩을 사용하면 이런 모순을 해결해준다.

데이터 바인딩은 뷰와 모델을 하나로 연결하는 것을 의미한다.



**왜 데이터 바인딩을 사용할까?**

jQuery를 사용하는 웹 애플리케이션의 경우를 살펴보자.

![procedural-programming](http://poiemaweb.com/img/procedural-programming.png)

이와 같은 구조 상 문제로 뷰가 변경되면 로직도 변경될 가능성이 매우 높다. 

자바스크립트가 DOM에서 특정 문서를 선택해야한다. ( 현재, 주체가  DOM 임. ) 이런구조는 간단한 사이트를 제작할때나 적합하다.

> h1태그에서 h2로 바뀌면 js파일도 변경되야함!





하지만 Angular는 DOM에 직접 접근하지 않고 템플릿과 컴포넌트 클래스의 상호 관계를 **선언하는 방식**(선언형)으로 뷰와 모델의 관계를 **데이터 바인딩으로** 관리한다. 데이터 바인딩은 **템플릿 문법**으로 기술된다. 

![declarative-programming](http://poiemaweb.com/img/declarative-programming.png)

현재, 주체가 템플릿 보조역할을 한다.



> h1태그가 바뀌뜬 말든 템플릿코드만 있으면 됀다.



컴포넌트 클래스와 템플릿이 서로 알 필요가 없는 **느슨한 결합**구조로 애플리케이션을 만드는게 중요하다.

# 2. 변화 감지(Change detection)

변화 감지란 **뷰와 모델의 동기화를 유지**하기 위해 **변화를 감지**하고 **이를 반영**하는 것을 말한다. 

> ex) form요소로인한 View변경을 ( 체크박스, 텍스트박스 ) Model이 알고 있어야하고 Model의 변경을 View가 반영해야한다. 이런 걸 가능하게 하려면 Model과 View가 양방향 바인딩이 되어야한다. 이때 왓쳐가 무한루프를 돌면서 변화를 감지 해야하는데 이 때문에 성능이 안좋아지게 된다. 현재의 앵귤러는 이를 단방향바인딩으로 해결했다.

변화 감지는 **DOM 이벤트를 캐치하는 것으로 감지**할 수 있다. 하지만 모델은 HTML 요소가 아니므로 이벤트가 발생하지 않는다. 따라서 모델의 변화 감지를 위해서는 별도의 조치가 필요하다. 모델이 변경된다는 것은 **컴포넌트 클래스의 프로퍼티 값이 변경되는 것을 의미**한다.

![change detection](http://poiemaweb.com/img/change-detection.png)



컴포넌트 클래스의 프로퍼티 값: name

setName() 메서드가 실행되면서 this.name에 inputYourName.value가 할당 = 값이 변경됨



**zone.js 가 변화를 감지하는법: 모델이 변화할 가능성이 있는 경우** (컴포넌트 클래스의 프로퍼티 값이 변경되는 경우)

- DOM 이벤트(click, key press, mouse move 등)
- Timer(setTimeout, setInterval)의 tick 이벤트
- Ajax 통신 / Promise

이를 위해 zone.js는 addEventListener, Timer 함수, XMLHttpRequest, Promise 등을 ***몽키패치***한다.

> 몽키패치 : 내부를 로직을 변경하는것

```js
// node_modules/zone.js/dist/zone.js
function zoneAwareAddEventListener() {...}
...

window.prototype.addEventListener = zoneAwareAddEventListener;
...
```



위와 같은 **비동기식 처리**가 수행될 때 컴포넌트 클래스의 데이터가 변경될 수 있다.

# 3. 데이터 바인딩

Angular는 단방향 데이터 바인딩과 양방향 데이터 바인딩을 지원한다.

| 데이터 바인딩        | 데이터의 흐름            | 문법                               |
| -------------------- | ------------------------ | ---------------------------------- |
| 인터폴레이션         | 컴포넌트 클래스 ⟹ 템플릿 | {{ expression }}                   |
| 프로퍼티 바인딩      | 컴포넌트 클래스 ⟹ 템플릿 | [property]=”expression”            |
| 어트리뷰트 바인딩    | 컴포넌트 클래스 ⟹ 템플릿 | [attr.attribute-name]=”expression” |
| 클래스 바인딩        | 컴포넌트 클래스 ⟹ 템플릿 | [class.class-name]=”expression”    |
| 스타일 바인딩        | 컴포넌트 클래스 ⟹ 템플릿 | [style.style-name]=”expression”    |
| 이벤트 바인딩        | 컴포넌트 클래스 ⟸ 템플릿 | (event)=”statement”                |
| 양방향 데이터 바인딩 | 컴포넌트 클래스 ⟺ 템플릿 | [(ngModel)]=”variable”             |

## 3.1 인터폴레이션(Interpolation)

`{{ expression }}`

단방향 바인딩에 사용되는 템플릿 문법으로 **표현식의 평가 결과를 문자열로 변환하여 템플릿에 바인딩**한다.

> 템플릿에서 사용하는 표현식에는 대입연산자(=, +=, -=), 증감 연산자(++, --), 비트 연산자(|, &), 객체 생성 연산자(new)와 같이 템플릿에서 컴포넌트 클래스의 프로퍼티를 변경할 있는 연산은 금지된다. 이는 인터폴레이션 뿐만 아니라 템플릿에서 사용하는 모든 표현식에 적용된다.



```typescript
import { Component } from '@angular/core';
@Component({
  selector: 'app-root',
  template: `
    <p>name: {{ name }}</p>
    <p>age: {{ age }}</p>
    <p>admin: {{ admin }}</p>
    <p>address: {{ address.city }} {{ address.country }}</p>
    <p>gender: {{ gender }}</p> <!-- 없어서 무시됨 -->
    <p>sayHi(): {{ sayHi() }}</p> <!-- return 값 -->
    <p>age * 10: {{ age * 10 }}</p> <!-- 200으로 수렴 --> 
    <p>age > 10: {{ age > 10 }}</p> <!-- true로 수렴 -->
    <p>'stirng': {{ 'stirng' }}</p>
 `
})
export class AppComponent {
  name = 'Angular';
  age = 20;
  admin = true;
  address = {
    city: 'Seoul',
    country: 'Korea'
  };

  sayHi() {
    return `Hi! my name is ${ this.name }.`;
  }
}
```

컴포넌트 클래스의 프로퍼티가 **문자열이 아닌 경우 문자열로 변환**되며 **존재하지 않는 프로퍼티**에 접근하는 경우 에러 발생없이 **아무것도 출력**하지 않는다. (무시된다)

## 3.2 프로퍼티 바인딩(Property binding) ★

##### 여기서 프로퍼티와 어트리뷰트의 차이점은?

프로퍼티 : 객체의 일원이다.

어트리뷰트 : 어떤 한 요소의 특정 속성을 나타낸다.



##### 웹페이지가 눈에 보이는 과정

index.html을 로딩하여 문자열인 것을 렌더트리로 만든다. (파싱) 이때 객체화 된다. (프로퍼티화)



**컴포넌트 클래스의 프로퍼티와 템플릿** 간의 단방향 바인딩(One-way binding)에 사용되는 템플릿 문법

표현식의 평가 결과를 HTML 요소의 **DOM 프로퍼티에 바인딩**한다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <!-- value 프로퍼티에 컴포넌트 클래스의 name을 프로퍼티 바인딩 -->
    <input type="text" [value]="name">
	<!-- type 애트리뷰트..의 값은 언제나 문자열.. 파싱이후엔 객체화.. value는 프로퍼티 []-->	

    <!-- innerHTML 프로퍼티에 컴포넌트 클래스의 contents를 프로퍼티 바인딩 -->
    <p [innerHTML]="contents"></p>
	<p>{{contents}}</p> <!-- 는 상위 표현의 문법적 설탕이다. -->
    <!-- src 프로퍼티에 컴포넌트 클래스의 imageUrl을 프로퍼티 바인딩 -->
    <img [src]="imageUrl"><br>

    <!-- disabled 프로퍼티에 컴포넌트 클래스의 isUnchanged를 프로퍼티 바인딩 -->
    <button [disabled]="isDisabled">disabled button</button>
  `
})
export class AppComponent {
  name = 'ungmo2';
  contents = 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.';
  imageUrl = 'https://via.placeholder.com/350x150';
  isDisabled = true;
}
```



```html
<p>{{ contents }}</p>
<input type="text" value="{{ name }}">

```

Angular는 렌더링 이전에 인터폴레이션을 프로퍼티 바인딩으로 변환한다. 사실 **인터폴레이션은 프로퍼티 바인딩의 Syntactic sugar인 것**이다. 위 코드는 아래의 코드와 동일하게 동작한다.

```html
<p [innerHTML]="contents"></p>
<input type="text" [value]="name">
```

> 표현식의 평가 결과를 HTML 요소의 **DOM 프로퍼티에 바인딩**한다. 



## 3.3 어트리뷰트 바인딩(Attribute binding)

브라우저는 HTML 문서를 파싱하여 DOM 트리로 변환하고 메모리에 적재한다. 이때 **HTML 요소는 DOM 노드 객체**로, **HTML 어트리뷰트는 DOM 노드 객체의 프로퍼티**로 **변환**된다. **HTML 어트리뷰트**의 값은 **언제나 문자열**이지만 **DOM 프로퍼티**는 객체를 비롯하여 **모든 값**을 가질 수 있다. 주의하여야 할 것은 **어트리뷰트와 프로퍼티가 언제나 1:1로 매핑되는 것은 아니라는 것**이다. 예를 들어 살펴보자.

- id 어트리뷰트와 id 프로퍼티와 1:1 매핑한다.
- class 어트리뷰트는 classList 프로퍼티로 **변환**된다.
- td 요소의 **colspan 어트리뷰트**의 경우, 매핑하는 **프로퍼티가 존재하지 않는다**.
- textContent프로퍼티의 경우, 대응하는 어트리뷰트가 존재하지 않는다.
- input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 매핑하지만 서로 다르게 동작한다.

```html
<input id="user" type="text" value="ungmo2"> <!-- 어트리뷰트 3개 -->
```



![html attributes](http://poiemaweb.com/img/html-attributes.png)

id 어트리뷰트는 id 프로퍼티와 1:1 매핑하므로 DOM 노드 객체 HTMLInputElement에는 **id 프로퍼티가 생성**되고 **id 어트리뷰트의 값 ‘user’가 할당**된다. 하지만 value 어트리뷰트는 value 프로퍼티와 1:1 매핑하지만 서로 다르게 동작한다. **DOM 노드 객체에 value 프로퍼티가 생성**되고 v**alue 어트리뷰트의 값 ‘ungmo2’이 할당**된다. 여기까지는 1:1 매핑하는 id 어트리뷰트와 동일하지만 사용자에 의해 input 요소에 새로운 값이 입력되면 다르게 동작하기 시작한다. 만약 사용자에 의해 **“lee”가 입력**되면 DOM 노드 객체의 **value 프로퍼티는 “lee”로 변경**된다. 하지만 **value 어트리뷰트는 초기값 “ungmo2”인 상태에서 변경되지 않는다.** 이는 HTML 요소가 DOM 노드 객체로 변환된 이후에 HTML 요소의 어트리뷰트는 변하지 않기 때문이다. 하지만 DOM 프로퍼티는 언제든지 바뀔 수 있다. 즉 **어트리뷰트**는 **DOM 프로퍼티의 초기값**을 의미하며 **DOM 프로퍼티**는 **현재값**을 의미한다.

1줄요약 : attributes 안에 있는 값 = 초기값 지정, properties의 값 = 초기값을 가지는건 동일하나 언제든지 변경 될 수 있다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <!-- 프로퍼티 바인딩 -->
    <input id="user" type="text" [value]="name">
    <!-- 어트리뷰트 바인딩 -->
    <input id="user" type="text" [attr.value]="name">
  `
})
export class AppComponent {
  name = 'ungmo2';
}

```





**프로퍼티 바인딩**은 ***DOM 노드 객체*에 컴포넌트 클래스 프로퍼티를 바인딩**하고 **어트리뷰트 바인딩**은 ***HTML 요소의 어트리뷰트*에 컴포넌트 클래스 프로퍼티를 바인딩**한다. 따라서 위 코드는 아래와 같이 변환될 것이다.

```html
<!-- 프로퍼티 바인딩의 변환 결과 -->
<input id="user" type="text"> <!-- document.getElementById('user').value에 -->

<!-- 어트리뷰트 바인딩의 변환 결과(name = 'ungmo2'일때) -->
<input id="user" type="text" value="ungmo2"> <!-- document.getElementById('user').arttributes 안에 -->
```



`<td>`태그에 없는 colspan 어트리뷰트를 주려면?

> 프로퍼티에 없는 것들은 어트리뷰트에 바인딩을 하면된다

## 3.4 클래스 바인딩(Class binding)

클래스 바인딩은 **우변의 표현식을 평가한 후** HTML class **어트리뷰트를 변경**한다. 

HTML class 어트리뷰트에 의해 **이미 클래스가 지정**되어 있을 때 **한개의 클래스를 대상**으로 하는 클래스 바인딩([class.class-name])은 **HTML class 어트리뷰트를 병합(merge)**하여 **새로운** HTML class **어트리뷰트를 작성**한다. 하지만 **복수의 클래스를 대상**으로 하는 클래스 바인딩([class])은 **기존 HTML class 어트리뷰트를 삭제**하고 **새로운 HTML class 어트리뷰트를 작성**한다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <!-- 조건의 의한 클래스 바인딩
         우변의 표현식이 true이면 클래스를 추가한다 -->
    <div [class.text-large]="isLarge">text-large</div>

    <!-- 조건의 의한 클래스 바인딩
         우변의 표현식(isRed === false)이 false이면 클래스(color-red)를 삭제한다 --> 
    <div class="text-small color-red" [class.color-red]="isRed">text-small</div>

	<!-- 여러개의 클래스를 한번에 지정할 수 있다 -->
    <div [class]="myClasses">text-large color-red</div>
    
	<!-- 클래스 바인딩은 기존 클래스 어트리뷰트보다 우선한다.
         따라서 기존 클래스 어트리뷰트는 클래스 바인딩에 의해 reset된다.
         클래스 바인딩의 위치는 관계없다. -->
    <div class="text-small color-blue" [class]="myClasses">text-large color-red</div>
	<!-- 기존 클래스 삭제 -->
  `,
  styles: [`
    .text-small { font-size: 18px;}
    .text-large { font-size: 36px;}
    .color-blue { color: blue;}
    .color-red { color: red;}
  `]
})
export class AppComponent {
  isLarge = true;
  isRed = false;
  // 클래스 바인딩은 문자열을 바인딩한다.
  myClasses = 'text-large color-red';
}
```

`<element [class.class-name]="booleanExpression">...</element>` 

- 한개의 class 만 지정할 때

`ngClass`

여러개의 class를 지정할때

## 3.5 스타일 바인딩(Style binding)

스타일 바인딩은 우변의 표현식을 평가한 후 HTML style 어트리뷰트를 변경한다. HTML style 어트리뷰트에 의해 **이미 스타일이 지정되어 있을 때 스타일 바인딩은 중복되지 않은 스타일은 병합(merge)**하여 그대로 사용하고 중복된 스타일은 스타일 바인딩의 스타일으로 덮어쓴다. 스타일 프로퍼티(border-radius 등)는 **케밥표기법(kebab-case) 또는 카멜표기법(camelCase)을 사용**한다. 사용 방법은 아래와 같다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <button class="btn"
      [style.background-color]="isActive ? '#4CAF50' : '#f44336'"
      [style.font-size.em]="isActive ? 1.2 : 1"
      (click)="isActive=!isActive">Toggle</button>
  `,
  styles: [`
    .btn {
      background-color: #4CAF50;
      border: none;
      border-radius: 8px;
      color: white;
      padding: 10px;
      cursor: pointer;
      outline: none;
    }
  `]
})
export class AppComponent {
  isActive = false; 
}
```



## 3.6 이벤트 바인딩(Event binding)

**이벤트 바인딩**은 뷰의 상태 변화(**버튼 클릭, 체크박스 체크, input에 텍스트 입력** 등)에 의해 **이벤트가 발생**하면 **이벤트 핸들러를 호출**하는 것을 말한다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <!-- (1) -->
    <input type="text" [value]="name" (input)="onInput($event)"> 
	<!--(발생한 이벤트) = "실행시킬 함수($event <-- 이벤트객체)" -->
    <!-- (2) -->
    <button (click)="onClick()">clear</button>
    <!-- (3) -->
    <p>name: {{ name }}</p>
  `
})
export class AppComponent {
  name = '';

  onInput(event) {
    console.log(event);
    // event.target.value에는 사용자 입력 텍스트가 담겨있다.
    this.name = event.target.value;
  }

  onClick() {
    this.name = '';
  }
}
```

1. 입력이벤트(input) 발생
2. onInput() 호출
3. $event (DOM 이벤트객체) 이벤트 핸들러에게 전달
4. $event를 통해 이벤트가 발생한 input태그 프로퍼티나 함수에 접근
5. 원하는 프로퍼티 (value)에서 값 추출 후 name에 할당
6. name 프로퍼티는 프로퍼티 바인딩에 의해 input 요소에 바인딩됨





## 3.7 양방향 데이터 바인딩(Two-way binding)

**뷰의 상태가 변화**하면 **컴포넌트 클래스의 상태도 변화**하고 그 반대로 **컴포넌트 클래스의 상태가 변화**하면 **뷰의 상태도 변화하는 것**이다.

```sequence
View->Component: View에서 변화감지
Component-->View: Component에서 변화감지
```

ngModel 디렉티브를 이벤트 바인딩(())과 프로퍼티 바인딩([]) 형식으로 기술한 후 우변에 뷰와 컴포넌트 클래스가 공유할 프로퍼티를 기술한다. ngModel 디렉티브를 사용하기 위해서는 FormsModule을 모듈에 등록하여야 한다.

```typescript
// src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, FormsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <input type="text" [(ngModel)]="name">
    <p>name: {{ name }}</p>
  `
})
export class AppComponent {
  name = '';
}
```

input 요소의 value 프로퍼티가 변화하면 컴포넌트 클래스의 name 프로퍼티도 동일한 값으로 변화하고 반대로 컴포넌트 클래스의 name 프로퍼티가 변화하면 input 요소의 value 프로퍼티도 동일한 값으로 변화한다.

사실 **Angular는 양방향 바인딩을 지원하지 않는다.** `[()]`(이것을 Banana in a box라고 부른다)에서 추측할 수 있듯이 양방향 바인딩은 이벤트 바인딩과 프로퍼티 바인딩의 축약 표현(Shorthand syntax)일 뿐이다. **즉 양방향 바인딩의 실제 동작**은 **이벤트 바인딩**과 **프로퍼티 바인딩의 조합**으로 이루어진다. 위 코드를 이벤트 바인딩과 프로퍼티 바인딩으로 표현한것이 아래의 예시다.



```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <input type="text" [value]="name" (input)="name=$event.target.value">
    <p>name: {{ name }}</p>
  `
})
export class AppComponent {
  name = '';
}
```





**ngModel**은 이벤트 바인딩과 프로퍼티 바인딩으로 구현되는 **양방향 바인딩을 간편하게 작성할 수 있도록 돕는 디렉티브**로서 사용자 입력과 관련한 DOM 요소(input, textarea, select 등의 폼 컨트롤 요소)에서만 사용할 수 있다. 

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <input [ngModel]="name" (ngModelChange)="name=$event">
    <p>name: {{ name }}</p>
  `
})
export class AppComponent {
  name = '';
}
```



