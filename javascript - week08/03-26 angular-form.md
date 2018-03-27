웹이 발명되고 초기에 사용자가 form에 입력한 데이터의 유효성검사를 하기위해 javascript가 탄생했다. 이런 단순한 용도로 나온 언어가 발전되어 현재의 자바스크립트가 되었다.





# 1. 폼이란?

폼은 애플리케이션에서 사용자의 데이터를 입력받는 인터페이스를 의미한다. 

가끔 사용자가 입력하는 데이터는 개발자가 의도한 형식의 데이터가 아닌경우가 있다. 따라서 입력한 데이터의 형식을 	체크하는 것이 필요한데, 이를 **유효성검증**이라고 한다.

HTML 표준 폼으로도 어느 정도의 유효성 검증이 가능하고 사용자의 데이터를 서버로 전송할 수 있지만 여러모로 **단점과 한계**가 있기 때문에 애플리케이션 개발에 적용하기 어렵다. Angular는 HTML 표준 폼의 단점과 한계를 보완한 **템플릿 기반 폼과 리액티브(모델 기반) 폼을 제공**한다.



# 2. HTML 표준 폼

일반 HTML 폼에서는`pattern`어트리뷰트를 사용해 정규표현식으로 `input`태그의 값을 검사한다. 이런식으로 form요소의 속성중에는 값의 유효성을 검사할 수 있는 속성들이있다.

ex) pattern, max-length, min-length, required 등



HTML의 단점과 한계에는 뭐가 있을까

1. 기본 제공하는 유효성 검증 어트리뷰트로 유효성 검증을 할 수 있으나 **명확한 에러를 제공하지않는다.**
2. 포커스를 **다른 요소로 옮기면 에러정보가 사라진다.**
3. **에러메세지 팝업이** OS마다 다르기 때문에 **스타일변경이 어렵다.**


```html
 <form action="/signup" method="POST"> <!-- 폼 요소 -->
    <input type="email" name="email" placeholder="Email"
      pattern="^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$"
      required> <!-- 폼 컨트롤 요소 -->
    <input type="password" id="password1" name="password1"
      placeholder="Password" pattern="^[a-zA-Z0-9]{4,10}$"
      required> <!-- 폼 컨트롤 요소 -->
    <input type="password" id="password2" name="password2"
      placeholder="Confirm Password" pattern="^[a-zA-Z0-9]{4,10}$"
      required> <!-- 폼 컨트롤 요소 -->
    <input type="submit" name="submit" value="Signup">
  </form><!-- // 폼 요소 -->
```

폼요소

​	ㄴ 폼 컨트롤 요소

​	ㄴ 폼 컨트롤 요소

​	ㄴ 폼 컨트롤 요소


# 3. Angular 폼

```typescript
mport { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <input type="text" (keyup.enter)="checkValue($event.target.value)">
    <em>{{ checkResult }}</em>
  `
})
export class AppComponent {

  checkResult: string;

  checkValue(value) {
    if (value.length > 3) {
      this.checkResult = '';
    } else {
      this.checkResult = '4자 이상 입력하세요';
    }
  }
}
```

템플릿 문법 사용으로 에러정보가 사라지지않고 유효성검증이 가능해졌다.

하지만 폼요소가 많아진다면? 템플릿에 여러 코드가 추가되면서 굉장히 보기힘든 템플릿이 될것이다.  Angular는 HTML 표준 폼의 **단점과 한계를 보완**하고 효과적인 폼 데이터 변경 추적과 유효성 검증 및 에러 처리를 지원하는 템플릿 기반 폼과 **리액티브(모델 기반) 폼을 제공한다.**

