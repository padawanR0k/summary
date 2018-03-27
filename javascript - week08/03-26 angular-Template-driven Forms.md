# 1. 템플릿 기반 폼(Template-driven Forms)이란?

컴포넌트 템플릿에서 디렉티브를 사용한다.

폼을 구성하는 방식으로 각 필드의 형식, 유효성 검증 규칙을 모두 템플릿에서 정의한다. 비교적 간단한 폼에 사용한다.

템플릿 기반 폼은 NgForm, NgModel, NgModelGroup 디렉티브를 중심으로 동작한다.  (Module에 추가해야 사용가능)

`import { FormsModule } from '@angular/forms';`



# 2. 템플릿 기반 폼의 중심 디렉티브

event, promise 등이 zone.js에 의해 몽키패치됨. 그래서 __zone_symbole_




## 2.1 NgForm 디렉티브

`NgForm`디렉티브는 폼 전체를 관리하는 디렉티브이다.  만약 ***FormsModule이 등록되어 있다면*** 모든 form요소는 자동으로  NgForm디렉티브로 관리된다. 관리된다는 것은 NgForm객체안에 개발자가 form을 제어및 검사할때 필요한 모든정보가 들어가 있다는것이다



폼 요소에 자동으로 적용되는 NgForm 디렉티브의 **적용을 취소하려면 폼 요소에 ngNoForm을 추가**한다. ngNoForm이 적용되면 navtive HTML 표준 폼으로 동작한다.

```html
<form ngNoForm></form>
```



HTML 표준 폼은 submit 버튼을 클릭하면 폼 데이터를 서버로 전송하고 페이지를 전환한다. NgForm 디렉티브가 적용된 템플릿 기반 폼은 submit 이벤트를 **인터셉트하여 데이터를 서버로 전송**하고 **페이지를 전환하는 기본 이벤트동작을 막는다**. 따라서 템플릿 기반 폼에서는 ngSubmit 이벤트를 사용한다.

```HTML 표준 폼은 submit 버튼을 클릭하면 폼 데이터를 서버로 전송하고 페이지를 전환하지만html
<form (ngSubmit)="onNgSubmit()"></form>
```



NgForm 디렉티브는 자신이 적용된 폼 요소(별도 설정이 없는 경우 폼 요소에 자동 적용된다)에 해당하는 FormGroup 인스턴스를 생성한다. 

```html
<form #userForm="ngForm" (ngSubmit)="onNgSubmit(userForm)">
      <input type="text" name="userid" placeholder="userid">
      <input type="password" name="password" placeholder="password">
      <input type="submit" value="submit">
</form>
```

`#userForm="ngForm"`  : ngForm객체를 할당시켜주면 DOM요소 대신 NgForm디렉티브가 만든 NgForm인스턴스를 넣어준다.

NgModel 디렉티브는 자신이 적용된 폼내부에  컨트롤 요소에 해당하는 FormControl 인스턴스를 생성한다. 그리고 FormControl 인스턴스 안에는 form을 관리할 수 있는 정보들이 들어있다. 이 정보들을 사용하려면 템플릿 참조변수에(`#userForm`)에 담아서 사용해야한다.

![img](http://poiemaweb.com/img/form-no-ngmodel.png)

생성된 FormGroup 인스턴스는  자식인 FormControl 인스턴스를 그룹화하기위한 컨테이너 역할을 한다.

만약 유효성을 검증할 때 FormControl 인스턴스 중 하나라도 유효하지 않다면 FormGroup은 유효하지않은 invalid(무효한)  상태가 된다.

FormGroup 인스턴스에 의해 자식 폼 **컨트롤 요소가 관리되도록 하기 위해서는 NgModel 디렉티브가 필요하다.**  즉 엘리먼트에 ngModel 디렉티브를 추가하지않으면 더 이상 관리대상이 아니게 된다.

```html
<form #userForm="ngForm" (ngSubmit)="onNgSubmit(userForm)">
      <input type="text" name="userid" placeholder="userid" ngModel>
      <input type="password" name="password" placeholder="password" ngModel>
      <input type="submit" value="submit">
    </form>
```



## 2.2 NgModel 디렉티브

![img](http://poiemaweb.com/img/form-formcontrol.png)



ngModel 디렉티브를 달아줌으로써 FormControl 인스턴스가 생성된다. 이 인스턴스가 요소의 값이나 유효성검증 상태를 추적할 수 있는 기능을 제공한다.

**폼 컨트롤 요소에는 반드시 name 어트리뷰트를 지정하여야 한다.**

왜냐? 참조변수 `userForm.value`로 값을 참조하면

```json
{
  "userid": "myid",
  "password": "1234"
}
```

이런 식으로 값이 저장되는데 name어트리뷰트가 **key역할을 해서**



```html
<input type="text" name="userid" ngModel #userid="ngModel"> <!-- FormControl 요소에도 참조변수를 사용할 수 있다.-->
<p>value: {{ userid.value }}</p> <!-- userid는 네이티브 DOM을 가리키지 않고 userid 폼 컨트롤 요소를 가리키는 NgModel 인스턴스를 가리킨다. -->
```



이때 템플릿참조변수 userid에 ngModel을 할당하면 userid가 

userid의 formControl요소를 가리키는 NgModel 인스턴스를 가리키게 되어 **참조변수를 통해 값 똔느 유효성 검증 상태 추적이 가능해진다.**

```html
<input type="text" name="userid" ngModel #userid="ngModel">

<!-- 템플릿 참조 변수를 통해 폼 컨트롤 요소의 값을 참조 -->
<p>userid value: {{ userid.value }}</p>
<!-- 템플릿 참조 변수를 통해 폼 컨트롤 요소의 유효성 검증 상태를 참조 -->
<p>userid valid: {{ userid.valid }}</p>
```



## 2.3 NgModelGroup 디렉티브

NgForm 디렉티브와 유사하게 **FormGroup 인스턴스를 생성**하고 NgModelGroup 디렉티브가 **적용된 폼 그룹 요소의 자식 요소 중**에서 **NgModel 디렉티브가 적용된 요소를 탐색**하여 **FormGroup 인스턴스에 추가**한다.

```html
@Component({
  selector: 'user-form',
  template: `
    <form #userForm="ngForm" (ngSubmit)="onNgSubmit(userForm.value)">
      <input type="text" name="userid" placeholder="id" ngModel>
      <div ngModelGroup="password">
        <input type="password" name="password1" placeholder="password" ngModel>
        <input type="password" name="password2" placeholder="confirm password" ngModel>
      </div>

      <input type="submit" value="submit">
    </form>
  `
})
```

![ngModelGroup](http://poiemaweb.com/img/ngmodelgroup.png)

# 3. NgModel과 양방향 바인딩

NgModel 디렉티브는 앞에서 살펴본 바와 같이 폼 컨트롤 요소의 값과 유효성 검증 상태를 관리하는 FormControl 인스턴스를 생성한다.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <input [ngModel]="name" (ngModelChange)="name=$event">
	<input [(ngModel)]="name">
	// 위 두 줄은 같다. [()]는 문법적설탕이다. 

    <p>name: {{ name }}</p>
  `
})
export class AppComponent {
  name = 'Lee';
}
```

`[ngModel]="name"` : 컴포넌트의 프로퍼티 name의 상태변화를 수신하여 업데이트함

`(ngModelChange)="name=$event" `: 템플릿의 상태변화 이벤트를 발신하여 컴포넌트의 name의 상태를 업데이트함 

# 4. 템플릿 기반 폼 유효성 검증

input요소에 ngModel 디렉티브를 달아서	각각의 input도 유효성 검사를 할 수 있다.

```html
 <input type="text" name="userid" placeholder="userid"
          pattern="^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$"
          #userid="ngModel" ngModel required>
<!-- ngif를 사용하여 각 상황에 맞는 에러메세지를 보여주게 하자 --> 
<!-- errors? 는 에러가 있을때를 뜻한다. errors 객체 안에는 여러 프로퍼티들이 있다. -->
<em *ngIf="userid.errors?.pattern && userid.touched">이메일 형식이 맞지않습니다.</em>
<em *ngIf="userid.errors?.required && userid.touched">입력하고 가세여</em>
```



form, input 태그들에는 ` class="ng-touched ng-dirty ng-valid"`와 같이 각 요소의 상태를 알려주는 클래스가 유동적으로 추가된다. 이를 사용해 각 상황에 맞는 디자인을 연출할 수 있다.