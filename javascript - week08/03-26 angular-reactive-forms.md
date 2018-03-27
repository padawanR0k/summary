# 1. 리액티브 폼(Reactive Forms / 모델 기반 폼)이란?

리액티브 폼은 템플릿 기반 폼보다 비교적 복잡한 경우 사용한다.

리액티브 폼은 템플릿이 아닌 **컴포넌트 클래스에서 폼 요소의 상태를 관리하는 객체인 폼 모델을 구성**하는 방식이다. 템플릿 폼과 다르게 Form객체에 직접 접근한다.



리액티브 폼(모델 기반 폼)은 컴포넌트 클래스에서 폼 요소의 값 및 유효성 검증 상태를 관리하는 자바스크립트 객체인 폼 모델(FormGroup, FormControl, FormArray)을 직접 정의/생성한다. 그리고 form* 접두사가 붙은 디렉티브(`formGroup, formGroupName, formControlName, formArrayName`)를 사용하여 템플릿의 폼 요소와 폼 모델을 프로퍼티 바인딩으로 연결한다.

컴포넌트 클래스 내부에서 정의/생성한 **폼 모델에 직접 접근**하여 **데이터 모델을 폼 모델에 반영**하고 템플릿의 폼 컨트롤 요소를 **관찰(observe)하고 변화에 대응**한다.



리액티브 폼은 FormControl, FormGroup, FormArray 클래스를 중심으로 동작한다. 이들을 사용하기 위해서 @angular/forms 패키지의 ReactiveFormsModule을 애플리케이션 모듈에 추가한다.

```typescript
// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { ReactiveFormsModule } from '@angular/forms';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, ReactiveFormsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```



# 2. 리액티브 폼의 중심 클래스와 디렉티브



## 2.1 FormGroup 클래스와 formGroup/formGroupName 디렉티브

FormGroup인스턴스는 자신의 자식들(FormControls, FormArray 인스턴스들)을 그룹화한다.

모든 form요소를 하나의 객체로 그룹화하여 모든 자식 폼 모델 인스턴스의 값과 유효성 상태를 관리한다. 만약 유효성을 검증할 때 자식 폼 모델 인스턴스 중 하나라도 유효하지 않다면 FormGroup은 유효하지 않게 된다.  `( && )`

```typescript
// 이런느낌
const myFormGroup = new FormGroup({
  // 자식 폼 모델 인스턴스
});

// app.component.ts
import { Component, OnInit } from '@angular/core';
import { FormGroup } from '@angular/forms';

@Component({
  selector: 'app-root',
  template: `
	<form [formGroup]="userForm" novalidate>
		<div FormGroupName="formControls"></div>
	</form>
<!--- novalidate는 기본html의 form요소 검사를 안하게하는 속성이다. -->
  `
})
export class AppComponent implements OnInit {

  userForm: FormGroup;

  ngOnInit() {
    this.userForm = new FormGroup({});
    console.log(this.userForm);
  }
}
```





**FormGroup 인스턴스**는 폼 요소 내부의 폼 컨트롤 요소들을 그룹화하기 위해 또다른 **FormGroup 인스턴스를 가질 수 있다.** formGroupName 디렉티브는 FormGroup 인스턴스의 자식 FormGroup 인스턴스와 폼 컨트롤 요소 그룹을 바인딩한다.

![form name](http://poiemaweb.com/img/formname.png)

```html
<div [FormGroupName]="'formControls'"></div>
<!-- 위아래 두 표현은 동치이다 -->
<div FormGroupName="formControls"></div>

[id]="id"  // 프로퍼티 
id={{ id }} 는 동치이다. // 어트리뷰트
```



## 2.2 FormControl 클래스와 formControlName 디렉티브

**FormControl 인스턴스**는 폼을 구성하는 **기본 단위**로서 폼 컨트롤 요소의 값이나 유효성 검증 상태를 추적하고 **뷰와 폼 모델**을 **동기화된 상태로 유지**한다.

```html
 <form [formGroup]="userForm" novalidate>
      <div>
        <!-- 리액티브 폼에서는 컴포넌트 클래스에서 FormControl 인스턴스를 직접 생성하고 formControlName 디렉티브를 사용하여 FormControl 인스턴스와 폼 컨트롤 요소를 바인딩한다. -->
        <input type="text" formControlName="userid" placeholder="user id">
      </div>
      <div formGroupName="passwordGroup">
        <div>
          <input type="password" formControlName="password" placeholder="password">
        </div>
        <div>
          <input type="password" formControlName="confirmPassword" placeholder="confirm password">
        </div>
      </div>
    </form>
    <pre>{{ userForm.value | json }}</pre> <!-- 컴포넌트 클래스의 변수 사용가능 -->
    <pre>{{ userForm.status }}</pre>
</form>

```

```typescript
export class AppComponent implements OnInit {

  userForm: FormGroup;

  ngOnInit() {
    this.userForm = new FormGroup({
      userid: new FormControl(''),
      passwordGroup: new FormGroup({
        password: new FormControl(''),
        confirmPassword: new FormControl('')
      })
    });
    console.log(this.userForm);
  }
}
```





FormControl은 폼 요소의 자식 폼 컨트롤 요소를 위해 사용하기도 하지만 **폼 요소없이 단독으로 사용할 수도 있다.**  [Reactive Programming과 RxJS] 옵저버블 이벤트 스트림에서 살펴본 바와 같이 input 요소의 이벤트는 FormControl의 valueChanges 프로퍼티에 의해 옵저버블 스트림으로 변환된다.

```typescript
// ① Angular forms
  serchInput: FormControl = new FormControl('');
  githubUser: GithubUser;

  // ② HttpClient를 의존성 주입한다.
  constructor(private http: HttpClient) {}

  ngOnInit() {
    // ① valueChanges 이벤트 옵저버블을 구독하면 컨트롤 값의 변경 내용을 옵저버블 스트림으로 수신할 수 있다.
    this.serchInput.valueChanges
      // ③ debounceTime 오퍼레이터는 다음 이벤트를 즉시 발생시키지 않고 지정 시간만큼 지연시킨다.
      .debounceTime(500)
      // ④ switchMap 오퍼레이터는 옵저버블을 받아서 새로운 옵저버블을 생성한다.
      .switchMap(userId => this.getGithubUser(userId))
      // ⑥ 옵저버블을 subscribe 오퍼레이터로 구독하면 옵저버가 데이터 스트림을 사용할 수 있다.
      .subscribe(user => this.githubUser = user);
  }
```



## 2.3 FormArray 클래스와 formArrayName 디렉티브



# 3. 리액티브 폼 유효성 검증

리액티브 폼은 템플릿의 폼 컨트롤 요소에  컴포넌트 클래스 내부에서 생성한 FormControl에 추가한다. FormControl에 추가된 검증기는 템플릿의 폼 컨트롤 요소의 상태가 변화할 때 마다 호출된다.

빌트인 검증기는 Validators 클래스에 정적 메소드로 정의되어 있다.

```typescript
class Validators {
  static min(min: number): ValidatorFn // 최대 최소
  static max(max: number): ValidatorFn
  static required(control: AbstractControl): ValidationErrors|null
  static requiredTrue(control: AbstractControl): ValidationErrors|null
  static email(control: AbstractControl): ValidationErrors|null
  static minLength(minLength: number): ValidatorFn // 숫자길이
  static maxLength(maxLength: number): ValidatorFn 
  static pattern(pattern: string|RegExp): ValidatorFn
  static nullValidator(c: AbstractControl): ValidationErrors|null
  static compose(validators: (ValidatorFn|null|undefined)[]|null): ValidatorFn|null
  static composeAsync(validators: (AsyncValidatorFn|null)[]): AsyncValidatorFn|null
}
```



```html
<form [formGroup]="userForm" novalidate>
      <div>
        <input type="text" formControlName="userid" placeholder="user id">
      </div>
      <div formGroupName="passwordGroup">
        <div>
          <input type="password" formControlName="password" placeholder="password">
        </div>
        <div>
          <input type="password" formControlName="confirmPassword" placeholder="confirm password">
        </div>
      </div>
    </form>
    <pre>{{ userForm.value | json }}</pre>
<pre>{{ userForm.status }}</pre>
```

```typescript
 ngOnInit() {
    this.userForm = new FormGroup({
      // FormControl 생성자 함수의 두번째 인자에 검증기를 전달한다.
      // 2개 이상의 검증기를 사용하는 경우, 배열로 검증기를 추가한다. 검증기는 템플릿의 폼 컨트롤 요소의 상태가 변화할 때 마다 호출된다.
      userid: new FormControl('', [
        Validators.required,
        Validators.pattern('[a-zA-Z0-9]{4,10}')
      ]),
      passwordGroup: new FormGroup({
        // FormControl 생성자 함수의 두번째 인자에 검증기를 전달한다.
        password: new FormControl('', Validators.required),
        confirmPassword: new FormControl('', Validators.required)
      }),
      hobbies: new FormArray([
        new FormControl(''),
        new FormControl('')
      ])
    });
  }
```



# 4. 사용자 정의 검증기(Custom validator)

 Angular는 사용자 정의 검증기를 정의할 수 있으며 템플릿 기반 폼과 리액티브 폼 모두에 사용할 수 있다. 



패스워드와 확인 패스워드가 일치하는지 체크하는 사용자 정의 검증기

```typescript
// password-validator.ts
import { AbstractControl } from '@angular/forms';

export class PasswordValidator {

  static match(form: AbstractControl) {
    // 매개변수로 전달받은 검증 대상 폼 모델에서 password와 confirmPassword을 취득
    const password = form.get('password').value;
    const confirmPassword = form.get('confirmPassword').value;

    // password와 confirmPassword의 값을 비교한다.
    if (password !== confirmPassword) {
      // 검증에 실패한 경우, 에러 객체를 반환한다.
      // errors.match에 결과값이 있다는것이다.
      return { match: { password, confirmPassword }};
    } else {
      // 검증에 성공한 경우, null을 반환한다.
      return null;
    }
  }
}
```

```typescript
import { PasswordValidator } from './password-validator';
...

ngOnInit() {
  this.userForm = new FormGroup({
    passwordGroup: new FormGroup({
      password: new FormControl('', Validators.required),
      confirmPassword: new FormControl('', Validators.required)
    }, PasswordValidator.match) // 검증기 호출
  });
}
```

