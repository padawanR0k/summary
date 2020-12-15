# 1. 빌트인 디렉티브(Built-in directive)란?

디렉티브는 DOM의 모든것을 관리하는 명령어를 의미한다. HTML요소, 어트리뷰트의 형태로 사용된다.

디렉티브는 애플리케이션 **전역에서 사용할 수 있는 공통 관심사**를 **컴포넌트에서 분리**하여 **구현**한 것으로 컴포넌트의 **복잡도를 낮추고 가독성을 향상**시킨다.

# 2. 빌트인 어트리뷰트 디렉티브(Built-in attribute directive)

```typescript
// text-blue.directive.ts
import { Directive, ElementRef, Renderer2 } from '@angular/core';

@Directive({
  selector: '[textBlue]'
})
export class TextBlueDirective {
  constructor(el: ElementRef, renderer: Renderer2) {
    renderer.setStyle(el.nativeElement, 'color', 'blue');
  }
}

```

textBlue 디렉티브는 요소의 어트리뷰트로 사용한다. 단, 디렉티브는 모듈의 declarations 프로퍼티에 등록되어야 한다.

```typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <div textBlue>textBlue directive</div>
  `
})
export class AppComponent { }
```

컴포넌트 디렉티브

컴포넌트의 **템플릿을 표시하기 위한 디렉티브**이다. @Component 데코레이터의 메타데이터 객체의 seletor 프로퍼티에 임의의 디렉티브의 이름을 정의한다.



어트리뷰트 디렉티브

어트리뷰트 디렉티브는 HTML 요소의 어트리뷰트와 같이 사용하여 해당 요소의 모양이나 동작을 제어한다. ngClass, ngStyle와 같은 빌트인 어트리뷰트 디렉티브가 있다.



구조 디렉티브(Structural Directives)

구조 디렉티브는 DOM 요소를 반복 생성(ngFor), 조건에 의한 추가 또는 제거(ngIf, ngSwitch)를 통해 DOM 레이아웃(layout)을 변경한다.

## 2.1 ngClass

***여러개***의 css 클랙스는 추가 또는 제거 할 수 있다. 한개의 클래스를 추가, 제거할때는 쓰지말자



우변의 표현식을 평가한 후 HTML class 어트리뷰트를 변경한다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <ul>
      <!-- 문자열에 의한 클래스 지정  -->
      <li [ngClass]="stringCssClasses">bold blue</li>
      <!-- 배열에 의한 클래스 지정  -->
      <li [ngClass]="ArrayCssClasses">italic red</li>
      <!-- 객체에 의한 클래스 지정  -->
      <li [ngClass]="ObjectCssClasses">bold red</li>
      <!-- 컴포넌트 메소드에 의한 클래스 지정 -->
      <li [ngClass]="getCSSClasses('italic-blue')">italic blue</li>
    </ul>
  `,
  styles: [`
    .text-bold   { font-weight: bold; }
    .text-italic { font-style: italic; }
    .color-blue  { color: blue; }
    .color-red   { color: red; }
  `]
})
export class AppComponent {
  state = true;

  // 문자열 클래스 목록
  stringCssClasses = 'text-bold color-blue';
  // 배열 클래스 목록
  ArrayCssClasses = ['text-italic', 'color-red'];
  // 객체 클래스 목록
  ObjectCssClasses = {
    'text-bold': this.state,
    'text-italic': !this.state,
    'color-blue': !this.state,
    'color-red': this.state
  };

  // 클래스 목록을 반환하는 컴포넌트 메소드
  getCSSClasses(flag: string) {
    let classes;
    if (flag === 'italic-blue') {
      classes = {
        'text-bold': !this.state,
        'text-italic': this.state,
        'color-red': !this.state,
        'color-blue': this.state
      };
    } else {
      classes = {
        'text-bold': this.state,
        'text-italic': !this.state,
        'color-red': this.state,
        'color-blue': !this.state
      };
    }
    return classes;
  }
}
```





## 2.2 ngStyle

여러개의 HTML 인라인 스타일을 추가 또는 제거한다. 한개의 인라인 스타일을 추가 또는 제거할 때는 스타일 바인딩을 사용하는 것이 좋다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <div>
      Width: <input type="text" [(ngModel)]="width">
      <button (click)="increaseWidth()">+</button>
      <button (click)="decreaseWidth()">-</button>
    </div>
    <div>
      Height: <input type="text" [(ngModel)]="height">
      <button (click)="increaseHeight()">+</button>
      <button (click)="decreaseHeight()">-</button>
    </div>
    <button (click)="isShow=!isShow">{{ isShow ? 'Hide' : 'Show' }}</button>
    <!-- 스타일 지정  -->
    <div
		<!-- 항상 쌍따옴표로 감싸야함 -->
      [ngStyle]="{
        'width.px': width,
        'height.px': height,
        'background-color': bgColor,
        'visibility': isShow ? 'visible' : 'hidden'
      }">
    </div>
  `
})
export class AppComponent {
  width = 200;
  height = 200;
  bgColor = '#4caf50';
  isShow = true;

  increaseWidth()  { this.width  += 10; }
  decreaseWidth()  { this.width  -= 10; }
  increaseHeight() { this.height += 10; }
  decreaseHeight() { this.height -= 10; }
}
```



# 3. 빌트인 구조 디렉티브(Built-in structural directive)

 **DOM 요소를 반복 생성(ngFor)**, 조건에 의한 **추가 또는 제거를 수행(ngIf, ngSwitch)**을 통해 **뷰의 구조를 변경**한다.

- 구조 디렉티브에는 `*` 접두사를 추가하며 `[]`을 사용하지 않는다.
- 하나의 호스트 요소(디렉티브가 선언된 요소)에는 하나의 구조 디렉티브만을 사용할 수 있다. -> ngFor, ngif 동시에 못씀

## 3.1 ngIf

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <!-- ngIf에 의한 show/hide - DOM상에서 완전 제거됨 -->
    <p *ngIf="isShow">Lorem ipsum dolor sit amet</p>

    <!-- 스타일 바인딩에 의한 show/hide 그냥 안보이기만함 -->
    <p [style.display]="isShow ? 'block' : 'none'">Lorem ipsum dolor sit amet</p>

    <button (click)="isShow=!isShow">{{ isShow ? 'Hide' : 'Show' }}</button>
  `,
  styles: [`
    p { background-color: #CDDC39; }
  `]
})
export class AppComponent {
  isShow = true;
}

```

**ngif 디렉티브로 제거된 요소는 DOM에서도 완전히 사라진다.**



if else

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
	<!-- 버튼이 mySkill을 변경시킴 -->
    <div>
      <input type="radio" id="one" name="skill"
            (click)="mySkill=$event.target.value" value="HTML">
      <label for="one"> HTML</label>
      <input type="radio" id="two" name="skill"
            (click)="mySkill=$event.target.value" value="CSS">
      <label for="two"> CSS</label>
    </div>

    <!-- 참인 경우, 별도의 ng-template를 사용하지 않는 방법  -->
    <div *ngIf="mySkill==='HTML'; else elseBlock">HTML</div>
    <ng-template #elseBlock><div>CSS</div></ng-template>

    <!-- 참인 경우, 별도의 ng-template를 사용하는 방법  -->
    <div *ngIf="mySkill==='HTML'; then thenBlock_1 else elseBlock_1"></div>
    <ng-template #thenBlock_1><div>HTML</div></ng-template>
    <ng-template #elseBlock_1><div>CSS</div></ng-template>
  `
})
export class AppComponent {
  mySkill = 'HTML';
}
```

ng-template는 호스트 요소를 감싸는 무의미한 요소를 만든다.

## 3.2 ngFor

컴포넌트 클래스의 컬렉션을 **반복하여 호스트 요소(ngFor 디렉티브가 선언된 요소) 및 하위 요소를 DOM에 추가**한다. 컬렉션은 일반적으로 배열을 사용한다.

```typescript
import { Component } from '@angular/core';

interface User {
  id: number;
  name: string
}

@Component({
  selector: 'app-root',
  template: `
    <!-- user를 추가한다 -->
    <input type="text" #name placeholder="이름을 입력하세요">
    <button (click)="addUser(name.value)">add user</button>
    <ul>
      <!-- users 배열의 length만큼 반복하며 li 요소와 하위 요소를 DOM에 추가한다 -->
      <li *ngFor="let user of users; let i=index">
	  <!-- 요소를 user에 담는다. users의; ngFor 구문은 변수를 가지고있는데 그중에 하나가 index;-->
	  <!-- index이외의 변수 :(first, last, even, odd 등)와 같은 로컬 변수가 제공
        {{ i }}: {{ user.name }}
        <!-- 해당 user를 제거한다 -->
        <button (click)="removeUser(user.id)">X</button>
      </li>
    </ul>
    <pre>{{ users | json }}</pre>
  `
})
export class AppComponent {
  users: User[] = [
    { id: 1, name: 'Lee' },
    { id: 2, name: 'Kim' },
    { id: 3, name: 'Baek' }
  ];

  // user를 추가한다
  addUser(name: string) {
    if (name) {
      this.users.push({ id: this.getNewUserId(), name });
    }
  }

  // 해당 user를 제거한다.
  removeUser(userid: number) {
    this.users = this.users.filter(({ id }) => id !== userid);
  }

  // 추가될 user의 id를 반환한다
  getNewUserId(): number {
    return this.users.length ? Math.max(...this.users.map(({ id }) => id)) + 1 : 1;
  }
}
```

ES6의 for…of에서 사용할 수 있는 이터러블(iterable)이라면 사용이 가능하다.(꼭 배열이 아니여도됨)

ngFor 디렉티브는 컬렉션 데이터(users)가 변경되면 컬렉션과 **연결된 모든 DOM 요소를 제거하고 다시 생성**한다. 이는 컬렉션의 변경 사항을 추적(tracking)할 수 없기 때문이다. 때문에 크기가 **매우 큰 컬렉션을 다루는 경우,** 퍼포먼스 상의 **문제를 발생**시킬 수 있다. ngFor 디렉티브는 **퍼포먼스를 향상시키기 위한 기능으로 `trackBy`를 제공**한다.

## 3.3 ngSwitch

ngSwitch 디렉티브는 switch 조건에 따라 여러 요소 중에 하나의 요소를 선택하여 DOM에 추가한다. 

```typescript
ngSwitch 디렉티브는 switch 조건에 따라 여러 요소 중에 하나의 요소를 선택하여 DOM에 추가한다. 
```

