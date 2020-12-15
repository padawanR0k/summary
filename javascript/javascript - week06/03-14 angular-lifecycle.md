# 1. 생명주기(Lifecycle)

**컴포넌트와 디렉티브**는 **생명주기(Lifecycle)**를 갖는다. 이 생명주기는 **생성하고 소멸되기까지의 여러 과정**을 말하며 **Angular에 의해 관리**된다. 다시 말해 Angular는 생명주기를 통해 **컴포넌트와 디렉티브를 생성**하고 **렌더링**하며 **프로퍼티의 변화를 체크**하고 **DOM에서 제거**하는 일련의 **과정을 관리**한다.

![lifecycle hooks](http://poiemaweb.com/img/hooks-in-sequence.png)

위 그림의 순서대로 컴포넌트와 디렉티브를 생성하고 소멸하는 과정 즉 생명주기를 관리하고 변화 감지에 의해 해당 인스턴스를 변경한다.



ngOinit, ngAfterContentInit, ngAfterViewInit, ngOnDestroy : 단한번만 불림

나머지는 체인지 디텍션에 의해 여러번 불림







# 2. 생명주기 훅 메소드(Lifecycle hooks)

생명주기 훅 메소드는 인터페이스의 형태로 제공된다. 

이와 같이 **생명주기(OnInit)**에는 동일한 이름의 **인터페이스(OnInit)가 존재**한다. 그리고 이 인터페이스는 **생명주기 이름 앞에 ng 접두어가 붙은 메서드(OnInit)를 포함**한다.

```typescript
interface OnInit {
  ngOnInit(): void
}
```



생명주기 OnInit에 실행되어야 할 행위를 정의하려면 OnInit 인터페이스의 ngOnInit 추상 메소드를 구현한다.

```typescript
export class AppComponent implements OnInit {
  name = 'Lee';

  constructor() {
    console.log('constructor');
  }

  // 생명주기 OnInit 단계에 실행할 처리를 구현한다.
  ngOnInit() {
    console.log('ngOnInit');
  }
}
// 인스턴스객체가 생성될때 constructor는 단 한번만 생성된다.
```



- 2.1 ngOnChanges

  - **부모 컴포넌트**에서 **자식 컴포넌트의 입력 프로퍼티로 바인딩한 값**이 초기화 또는 **변경**되었을 때 실행된다. 따라서 **컴포넌트에 입력 프로퍼티가 없는 경우, 호출되지 않는다**.

  - **기본자료형의 값이 재할당되었을 때와 객체의 참조가 변경되었을 때만 반응한다.** 즉 **객체의 프로퍼티**가 변경되었을 때에는 **반응하지 않는다.** 

  - ```typescript
    class MyComponent implements OnChanges {
      @Input() prop1: number;
      @Input() prop2: string;

      ngOnChanges(changes: SimpleChanges) {
        // changes는 모든 입력 프로퍼티의 이전 값과 현재 값을 포함한다.
        console.log(changes);
        /*
        {prop1: SimpleChange, prop2: SimpleChange}
          prop1: SimpleChange {previousValue: undefined, currentValue: 100, firstChange: true}
          prop2: SimpleChange {previousValue: undefined, currentValue: "string", firstChange: true}
        */
      }
    }
    ```

    ​

- 2.2 ngOnInit

  - ngOnChanges 이후, **모든 프로퍼티와 입력 프로퍼티의 초기화가 완료된 시점에 한번만 호출된다.**
  - Angular에서 관리하는 입력 프로퍼티의 경우, constructor가 호출되는 단계에서는 초기화되기 이전의 상태이며 참조시 undefined가 반환된다. 즉 **컴포넌트 프로퍼티의 참조는 ngOnInit 이후 보장된다.** 
  - 이때 모든 프로퍼티를 들여다 볼 수 있다.

- 2.3 ngDoCheck

  - ngOnInit 이후, **컴포넌트** 또는 **디렉티브의** 모든 **상태의 변화가 발생**할 때마다 **호출**된다. 즉 Angular의 변화 감지 로직이 상태 변화를 감지하면 호출된다.
  - ngOnChanges와 다르게 **모든 상태의 변경에 의해 호출됨**

- 2.4 ngAfterContentInit

  - ngContent 디렉티브를 사용하여 외부 콘텐츠를 컴포넌트의 뷰에 프로젝션한 이후 호출된다. 첫번째 ngDoCheck 호출 이후에 한번만 호출되며 컴포넌트에서만 동작하는 컴포넌트 전용 훅 메소드이다.

- 2.5 ngAfterContentChecked

  - 콘텐츠 프로젝션에 의해 컴포넌트로 프로젝션된 콘텐츠를 체크한 후 호출된다. ngAfterContentInit 호출 이후,

- 2.6 ngAfterViewInit

  - 컴포넌트의 뷰와 자식 컴포넌트의 뷰를 초기화한 이후 호출된다. 첫번째 ngAfterContentChecked 호출 이후 한번만 호출
  - 이때 부터 DOM 컨트롤이 가능해진다.

- 2.7 ngAfterViewChecked

  - 컴포넌트의 뷰와 자식 컴포넌트의 뷰를 체크한 이후 호출된다. 첫번째 ngAfterViewInit 호출 이후, ngAfterContentChecked 호출 이후 호출

- 2.8 ngOnDestroy

  - 컴포넌트와 디렉티브가 소멸하기 이전 호출된다. 
  - 이때 파이프라인을?? 명시적으로 이 타이밍이 끊어줘야한다.
# 3. 생명주기 훅 메소드 실습



# 3.1 컴포넌트 생명 주기 훅 메소드
```typescript
// child.component.ts
import { Component, Input, OnChanges, OnInit, DoCheck, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <p>child component</p>
    <p>부모 컴포넌트가 전달한 값: {{ prop }}</p>
  `
})
export class ChildComponent implements OnChanges, OnInit, DoCheck, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy {
  @Input() prop: string; // input 프로퍼티값을 사용할 때 주의하자.

  constructor() {
    console.log('[construnctor]');
    console.log(`prop: ${this.prop}`); // prop: undefined
    this.prop = 'TEST';
    console.log(`prop: ${this.prop}`); // prop: TEST
  }

  ngOnChanges(changes: SimpleChanges) {
    console.log('[OnChanges]');
    console.log(`prop: ${this.prop}`); // prop: Hello // constructor의 this.prop는 쓰나마나한 코드가 되었다.
    console.log('changes:', changes);
  }

  ngOnInit() {
    console.log('[OnInit]');
    console.log(`prop: ${this.prop}`); // prop: Hello // 여기서 선언하면 정확하다.
  }

  ngDoCheck() {
    console.log('[DoCheck]');
  }

  ngAfterContentInit() {
    console.log('[ngAfterContentInit]');
  }

  ngAfterContentChecked() {
    console.log('[ngAfterContentChecked]');
  }

  ngAfterViewInit() {
    console.log('[ngAfterViewInit]'); // View에 접근하면 100% 보인다.
  }

  ngAfterViewChecked() {
    console.log('[ngAfterViewChecked]');
  }

  ngOnDestroy() {
    console.log('[ngOnDestroy]');
  }
}
```



# 3.2 ngOnChanges와 ngDoCheck

`ngOnChanges`와 `ngDoCheck`는 모두 상태 변화와 관계가 있다. 하지만 ngOnChanges는 **입력 프로퍼티의 초기화, 변경 시에 호출**되고 ngDoCheck는 모**든 변화 감지 시점에 호출**된다. 하지만 객체의 경우, 내부 프로퍼티를 변경하여도 **객체의 참조는 변경되지 않기** 때문에 ngOnChanges는 이 **변화에 반응하지 않는다**. 즉 **기본자료형과 불변객체와 같이 이뮤터블(immutable)한 값**에만 반응한다.

# 3.3 디렉티브 생명 주기 훅 메소드

컴포넌트는 다른 컴포넌트를 사용할 수 도, 자식이 될 수 도 있다. 

디렉티브는 무조건 다른 컴포넌트의 자식으로 사용될 수 밖에 없다.





http://poiemaweb.com/angular-lifecycle 다시 보자...