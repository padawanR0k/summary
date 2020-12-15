# 0. 시작하는 말

>  컴퓨터공학관점에서 중복은 매우 비효율적이다. 과거의 웹에서는 중복되는 코드가 매우 많앗다. 현재의 웹은 그 부분을 보완하고 있다. 그 결과로 앵귤러가 나왔고 앵귤러에서도 중복이 있을 수 있기 때문에  앵귤러는 중복을 제거할 수 있는 기능을 갖고 있다.



웹에서 html, css가 분리되어있는 이유가 무었인가? 

관심사가 달라서, 이때 html의 tag들은 왜있는가?

메타데이터로서 기계에게 정보를 구조화할때 필요해서



어떤 것을 받아들일때 그냥 받아들이지말고 왜?를 생각하자.

중복은 프로그래머의 적이다. 중복은 절대 그냥 넘기지 말자.



# 1. 디렉티브(directive)란?

컴포넌트는 뷰를 생성하고 관리한다는 목적으로 생긴다.  만약 특정 컴포넌트가 애플리케이션에서 공통적으로 사용하는 부분이라면 **디렉티브로** 떼어내서 만드는 것이다. (컴포넌트가 디렉티브의 집합아래)

디렉티브(Directive) “DOM의 모든 것(모양이나 동작 등)을 관리하기 위한 지시(명령)”이다. HTML 요소 또는 어트리뷰트의 형태로 사용하여 디렉티브가 사용된 요소에게 무언가를 하라는 지시(directive)를 전달한다.

디렉티브는 애플리케이션 **전역에서 사용할 수 있는 공통 관심사**를 **컴포넌트에서 분리**하여 구현한 것으로 컴포넌트의 **복잡도를 낮추고 가독성을 향상**시킨다. **컴포넌트**도 뷰를 생성하고 이벤트를 처리하는 등 DOM을 관리하기 때문에 **큰 의미에서 디렉티브**로 볼 수 있다.



컴포넌트는 반드시 뷰(템플릿을 가져야한다.) 디렉티브는 뷰가 없고 어트리뷰트로 사용된 컴포넌트(호스트요소)가 있어야한다.

```html
<todo-form></todo-form>
<todo-nav></todo-nav>
<todo-list [todos]="todos"></todo-list>
<!-- todo-list가 호스트요소 [todos]="todos" 구조 디렉티브 -->
<todo-footer></todo-footer>
```



# 2. 디렉티브의 종류



- 컴포넌트 디렉티브(Component Directives)

  컴포넌트의 템플릿을 표시하기 위한 디렉티브이다. @component 데코레이터의 메타데이터 객체의 seletor 프로퍼티에서 임의의 디렉티브의 이름을 정의한다.

- 어트리뷰트 디렉티브(Attribute Directives)

  어트리뷰트 디렉티브는 HTML 요소의 어트리뷰트로 사용하여 해당 요소의 모양이나 동작을 제어한다. ngClass, ngStyle와 같은 빌트인 디렉티브가 있다.

- 구조 디렉티브(Structural Directives)

  구조 디렉티브는 DOM 요소를 반복 생성(ngFor), 조건에 의한 추가 또는 제거(ngIf, ngSwitch)를 통해 DOM 레이아웃(layout)을 변경한다.

- 사용자 정의 디렉티브

# 3. 사용자 정의 어트리뷰트 디렉티브
## 3.1 사용자 정의 어트리뷰트 디렉티브의 생성

```bash
$ ng generate directive textBlue
```

새로 성성된 xxx.directive.ts파일

```typescript
import { Directive } from '@angular/core';

@Directive({
  selector: '[appTextBlue]'
})
export class TextBlueDirective {

  constructor() { }

}



// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { TextBlueDirective } from './text-blue.directive'; // 디렉티브 임포트

@NgModule({
  // 이 모듈에 소속하는 컴포넌트, 디렉티브, 파이프를 선언
  declarations: [AppComponent, TextBlueDirective],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }


// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<p textBlue>textBlue directive</p>`
    ///         디렉티브 삽입
})
export class AppComponent { }
```

템플릿이없고 component대신 Directive가 들어가 있음

프레임워크는 객체의 생성과 삭제를 스스로 관리하기 때문에 new키워드를 사용하지 않는다.

## 3.2 이벤트 처리



```typescript
import { Directive, ElementRef, Renderer2, Input, OnInit, HostListener } from '@angular/core';
@Directive({
  selector: '[TextColor]' // 대괄호의 의미는 "어트리뷰트로 쓰일것이다."
})

export class TextBlueDirective implements OnInit {
  @Input() color: string;
  defaultColor = 'gray';
  // host요소에 이벤트리스너를 달았다.
  @HostListener('mouseenter') onmouseenter() {
    this.changeTextColor(this.color);
    this.render.setStyle(this.el.nativeElement, 'font-weight', 'bold');
    console.log('mouseenter');
  }
  @HostListener('mouseleave') onmouseleave() {
    this.changeTextColor(this.defaultColor);
    console.log('mouseleave');
  }
  constructor(private el: ElementRef, private render: Renderer2) {
    // contructor 내부는 타입스크립트가 관리함.
    // 매개변수에 접근제한자를 안쓸시 지역변수가 되어 ngOnInit에서 참조가 불가능하게 됨
    // 객체가 구현되는 순서는 1. constructor내부 2. class 내부 3. ngOnInit

    console.log(this.color);
    // el은 호스트요소의 DOM node객체를 가져온다.
    console.log(el.nativeElement);

  }
  ngOnInit() {
    this.render.setStyle(this.el.nativeElement, 'color', this.defaultColor);
    console.log(this.color);
  }
  // 앵귤러가 자동으로 만든 el: ElementRef 자동으로 넣어줌
  // host요소의 참조가 필요한데, 그때 필요한게 el
  // Dependency Injection

  changeTextColor(color) {
    this.render.setStyle(this.el.nativeElement, 'color', color);
  }
}


```



## 3.3 @Input 데이터 바인딩

현재 textBlue 디렉티브는 이벤트에 의해 어트리뷰트 호스트의 텍스트 컬러를 파란색으로 변경한다. 이제 어트리뷰트 호스트에서 지정한 컬러를 사용하여 어트리뷰트 호스트의 텍스트 컬러를 변경하도록 리팩토링하여 보자. 이를 위해 어트리뷰트 호스트에서 지정한 값을 디렉티브로 가져올 수 있어야 한다.



# 4. 사용자 정의 구조 디렉티브
## 4.1 ng-template 디렉티브

ng-template 디렉티브는 페이지에서 렌더링 될 요소를 div 또는 span 등의 요소와 함께 사용할 필요가 없는 요소들을 그룹화할 때 사용한다.

ngIf, ngFor, ngSwtch 디렉티브의 경우, ng-template 디렉티브로 변환된다.

```html
<ul>
  <li *ngFor="let item of items">{{ item }}</li>
</ul>

<!--
NgFor 디렉티브 앞에 붙은 *(asterisk)는 아래 구문의 문법적 설탕(syntactic sugar)이다.
즉 위 코드는 아래의 코드로 변환된다.
-->

<ul>
  <ng-template ngFor let-item [ngForOf]="items">
    <li>{{ item }}</li>
  </ng-template>
</ul>
```



## 4. ng-container 디렉티브

ng-container 디렉티브도 ng-template와 마찬가지로 페이지에서 렌더링 될 요소를 **div 또는 span 등의 요소와 함께 사용할 필요가 없는 요소들을 그룹화**할 때 사용한다.

```html
<p>
  안녕하세요!
  <span *ngIf="user">
    {{ user.name }} 님
  </span>
  방갑습니다.
</p>

<p>
  안녕하세요!
  <ng-container *ngIf="user">
    {{ user.name }} 님
  </ng-container>
  방갑습니다.
</p>
```



Angular는 같은 요소에 하나 이상의 구조 디렉티브 사용을 금지한다. **일반적으로 ng-container는 동일한 요소에 하나 이상의 \*ngIf 또는 *ngFor와 같은 구조 디렉티브를 사용하기 위한 헬퍼 요소로서 사용한다.**

```typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <ng-container *ngIf="isShow">
      <ng-container *ngFor="let item of items">
        <span>{{ item }}</span>
      </ng-container>
    </ng-container>
    <button (click)="isShow=!isShow">
      {{ isShow ? 'hide' : 'show' }}
    </button>
  `
})
export class AppComponent {
  isShow = true;
  items = [1, 2, 3];
}
```

