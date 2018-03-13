#1. 템플릿 참조 변수(Template reference variable)

템플릿 참조 변수는 DOM 요소에 대한 참조를 담고 있는 변수이다. 템**플릿의 요소 내에서 해시 기호(#)를 변수명 앞에 추가**하여 템플릿 참조 변수를 선언하고 **템플릿 내 자바스크립트 코드에서는 해시 기호없이 참조**한다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <div>
      <!-- 템플릿 참조 변수 email의 선언 -->
      <input #email type='email' placeholder="이메일을 입력하세요">
      <!-- 템플릿 참조 변수 email의 참조  -->
      <button (click)="checkEmail(email.value)">이메일 형식 체크</button>
    </div>
    <span *ngIf="result">{{ result }}</span>
  `
})
export class AppComponent {
  result: string;

  checkEmail(email: string) {
    const regexr = /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/;

    if (regexr.test(email)) {
      this.result = '';
    } else {
      this.result = '이메일 주소의 형식이 유효하지 않습니다.';
    }
  };
}
```



#2. 세이프 내비게이션 연산자(Safe navigation operator)

세이프 내비게이션 연산자 `?`는 컴포넌트 클래스의 프로퍼티의 값이 null 또는 undefined일 때 발생하는 에러를 회피하기 위해 사용한다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <!-- 무시됨. obj가 null 또는 undefined일 때 아무것도 표시하지 않는다. -->
    {{ obj }}
    <!--이때는 무시가 아니라 에러 ERROR TypeError: Cannot read property 'id' of undefined -->
    {{ obj.id }}
    <!-- 세이프 내비게이션 연산자는 null 또는 undefined의 프로퍼티를 만나면 처리를 종료하고 에러를 발생시키지 않는다. -->
    {{ obj?.id }}
  `
})
export class AppComponent { }
```

