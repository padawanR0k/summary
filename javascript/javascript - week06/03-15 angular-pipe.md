# 1. 파이프(Pipe)란?

데이터 자체를 변경하는 것은 사이드 이펙트가 있으므로 **화면에 표시 형식만 변경하고 싶을 때 사용하는 것**이 파이프이다.	

```js
const today = new Date();

console.log(today.toString()); 
// Sat Sep 23 2017 00:26:55 GMT+0900 (KST) 보다는 “2017년 09월 23일 12시 26분 55초" 이 보기 편하지 않은가?



// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <p>{{ today }}</p>
    <p>{{ today | date }}</p>
    <p>{{ today | date: 'y년 MM월 dd일 hh시 mm분 ss초' }}</p>

    Sat Sep 23 2017 00:26:55 GMT+0900 (KST)

    Sep 23, 2017

    2017년 09월 23일 12시 26분 55초
  `
})
export class AppComponent {
  today = new Date();
}
```



# 2. 빌트인 파이프

| 파이프                                                   | 의미             |
| -------------------------------------------------------- | ---------------- |
| [date](https://angular.io/api/common/DatePipe)           | 날짜 형식 변환   |
| [JSON](https://angular.io/api/common/JsonPipe)           | JSON 형식 변환   |
| [uppercase](https://angular.io/api/common/UpperCasePipe) | 대문자 변환      |
| [lowercase](https://angular.io/api/common/LowerCasePipe) | 소문자 변환      |
| [currency](https://angular.io/api/common/CurrencyPipe)   | 통화 형식 변환   |
| [percent](https://angular.io/api/common/PercentPipe)     | 퍼센트 형식 변환 |
| [decimal](https://angular.io/api/common/DecimalPipe)     | 자리수 형식 변환 |
| [slice](https://angular.io/api/common/SlicePipe)         | 문자열 추출      |
| [async](https://angular.io/api/common/AsyncPipe)         | 비동기 객체 출력 |

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/observable/interval';
import 'rxjs/add/operator/take';

@Component({
  selector: 'app-root',
  template: `
    <h3>DatePipe</h3>
    <p>{{ today | date: 'y년 MM월 dd일 hh시 mm분 ss초' }}</p>
	<!-- 2018년 03월 14일 10시 34분 28초 -->

    <h3>CurrencyPipe</h3>
    <!-- 한국원:통화기호표시:소숫점위 최소 1자리 소숫점아래 1~2 -->
    <p>{{ price | currency: 'KRW':true:'1.1-2' }}</p>
	<!-- ₩0.12 -->

    <h3>SlicePipe : array</h3>
    <!-- slice:start[:end] -->
    <ul>
      <li *ngFor="let i of collection | slice:1:3">{{i}}</li>
    </ul>
	<!-- b --> 
     <!-- c -->

   
    <h3>JsonPipe</h3>
    <pre>{{ object | json }}</pre>
	<!-- {
          "foo": "bar",
          "baz": "qux",
		 "nested": {
    	  "xyz": 3
      		}
    	}
	-->

    <h3>DecimalPipe</h3>
    <p>{{ pi | number:'3.5' }}</p>
	<!-- 003.14159 -->

    <h3>PercentPipe</h3>
    <p>{{ num | percent:'3.3' }}</p>
	<!-- 134.950% -->

    <h3>UpperCasePipe</h3>
    <p>{{ str | uppercase }}</p>
	
    <h3>LowerCasePipe</h3>
    <p>{{ str | lowercase }}</p>

    <h3>AsyncPipe</h3>
    <p>{{ second$ | async }}</p>
	<!-- 숫자가 0에서 9까지 증가 -->
  `
})
export class AppComponent {
  today = new Date();
  price = 0.1234;
  collection: string[] = ['a', 'b', 'c', 'd'];
  str = 'abcdefghij';
  object: Object = { foo: 'bar', baz: 'qux', nested: { xyz: 3 } };
  pi = 3.141592;
  num = 1.3495;
  // 1s마다 값을 방출하고 10개를 take한다. (0 ~ 9)
  second$ = Observable.interval(1000).take(10);
}
```



# 3. 체이닝 파이프

여러개의 파이프를 조합하여 결과를 출력하는 것을 체이닝 파이프라 한다.

```typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h3>SlicePipe + UpperCasePipe</h3> 
    <p>{{ name | slice:4 | uppercase }}</p> 
	<!--UNG-MO 1.자르고 2.대문자화--> 
  `
})
export class AppComponent {
  name = 'Lee ung-mo';
}
```



# 4. 커스텀 파이프

사용자가 입력한 문자열을 반전하는 커스텀 파이프

```bash
$ ng generate pipe reverse
```

```typescript
// reverse.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'reverse' // 파이프의 식별자
})

// 파이프 클래스는 PipeTransform 인터페이스의 transform 메소드를 구현해야 한다.
export class ReversePipe implements PipeTransform {
  transform(value = ''): string {
    return value.split('').reverse().join('');
  }
}

// transform 문법
transform(value: any, ...args: any[]): any
```

transform 메소드는 **파이프의 변환 대상인 값(value)**와 **옵션 파라미터를 인자**로 받는다. 그리고 변환된 **값을 반환**한다.

커스텀 파이프는 **모듈의 declarations에 등록**되어야 한다. Angular CLI를 사용하여 파이프를 생성하면 모듈에 자동 등록된다.



```typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <input type="text" [(ngModel)]="value"> 
    <p>{{ value | reverse }}</p>
  `
})
export class AppComponent {
  value: string;
}
```

이제 입력할때마다 글자의 순서를 거꾸로 한 값이 value에 담긴다.

# 5. 파이프와 변화 감지(Change detection)

**뷰**와 **모델**간의 **동기화를 유지**하기 위해 **변화를 감지**하고 **이를 반영**하는것을 **변화감지**라고한다.

Angular는 **DOM 이벤트**(click, key press, mouse move 등), Timer(setTimeout, setInterval)의 tick 이벤트, 서버와의 Ajax 통신 이후 **변화 감지를 통해 데이터 바인딩 대상의 변경 사항을 찾는다**. 이런 행동은 **시스템에 부하를 증가**시키므로 가능한 부하를 최소한하기위해 파이프를 사용할 때는 보다 **간단하고 빠른 변경 감지 알고리즘을 사용**한다.



# 6. 순수 파이프(pure pipe)와 비순수 파이프(impure pipe)

파이프는 순수 파이프(pure pipe)와 비순수 파이프(impure pipe)로 분류할 수 있다. 비순수 파이프는 @Pipe 데코레이터의 메타데이터 pure 프로퍼티에 false를 지정한 것이다. limit 파이프를 비순수 파이프로 변경해보자.



adrress를 변화시키는 행동을 감지하는 못하는것이 순수 파이프

push()같은 adrress를 변화시키지 않는 행동은 파이프가 감지하지 못한다. **push()같은거 쓰지말자**

```typescript
// limit.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'limit',
  pure: false // pure속성에 false를 주면 비순수파이프가되지만 성능이 안좋아진다.
})
export class LimitPipe implements PipeTransform {
  transform(todos: any[], limit: number): any {
    return todos.filter((el, i) => i < limit);
  }
}
```

**반드시 필요한 경우가 아니라면 파이프보다는 컴포넌트의 프로퍼티를 사용하는 편이 유리하다.**