

# 1. 컴포넌트의 계층적 트리 구조

컴포넌트의 계층적 트리 구조, 즉 컴포넌트 간의 부모-자식 관계는 데이터와 이벤트가 왕래하는 정보 흐름의 통로가 되며 이를 통해 다른 컴포넌트와의 상태 공유가 이루어지기 때문에 컴포넌트 간의 **부모-자식 관계는 Angular 애플리케이션에서 중요한 의미**를 갖는다. 이 계층적 구조는 DOM 트리와 유사한 형태를 가지게 되는데 이를 컴포넌트 트리라고 한다.

![component-interaction](http://poiemaweb.com/img/component-interaction.png)



```code
component-interaction/
├── src/
│   ├── app/
│   │   ├── user-list/
│   │   │   └── user-list.component.ts
│   │   ├── app.component.ts
│   │   └── app.module.ts
...
```



부모-자식 컴포넌트들 간에는 느슨한 관계를 가져야 하나 상태공유할때는 아니다.



**프로퍼티 바인딩** 부모 => 자식   

**이벤트 바인딩** 자식 => 부모 



- @Input, @Output 데코레이터

- ViewChild와 ViewChildren

- 서비스 중재자 패턴을 구현한 상태 공유 서비스

  - 컴포넌트들이 자신의 상태변화를 다른 컴포넌트에게 전파할때 컴포넌트들간의 선을 따라 움직임

    그래서 편하게 하려고 service라는 걸 따로 만들어 놓음

    다른 컴포넌트가 service를 계속 watch하고 있게함

    많은 컴포넌트가 watch하고 있으면 service에 과부하가 걸리기때문에 service만의 스토어를 지정해줘야함?

- 상태 관리를 위한 외부 라이브러리사용

# 2. 부모 컴포넌트와 자식 컴포넌트의 상태 공유



## 2.1 부모 컴포넌트에서 자식 컴포넌트로 상태 전달



### 2.1.1 @Input 데코레이터

![parent to child](http://poiemaweb.com/img/parenttochild.png)

상태를 변화시키는 form 요소를 가지고 있는 부모 컴포넌트의 경우 자식 컴포넌트에게 상태변화를 전할 경우가 있다. 이러한 경우 부모 컴포넌트는 **프로퍼티 바인딩**, 자식 컴포넌트는 부모 컴포넌트가 전달한 상태 정보를 **@Input 데코레이터**를 통해 컴포넌트 프로퍼티(입력 프로퍼티)에 바인딩한다.

[state] 프로퍼티에 myState변수를 보낸다. `@input데코레이터 프로퍼티명`으로 변수를 받을 수 있다.



이것은 다른 컴포넌트와 결합도를 낮게 유지하면서 다른 컴포넌트와 상태 정보를 교환할 수 있다는 것을 의미한다.

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { User } from './models/user.model';

@Component({
  selector: 'app-root',
  template: `
    <div class="container">
      <div class="row">
        <form class="form-inline">
          <div class="form-group" style="margin: 30px 0">
            <label for="name">Name:</label>
            <input
              #name type="text" id="name"
              class="form-control"
              placeholder="이름을 입력하세요">
            <label for="role">Role:</label>
            <select #role id="role" class="form-control">
              <option>Administrator</option>
              <option>Developer</option>
              <option>Designer</option>
            </select>
            <button
              class="btn btn-default"
              (click)="addUser(name.value, role.value)">Add user
            </button>
          </div>
          <app-user-list [users]="users"></app-user-list> <!--프로퍼티 바인딩을 통해 자식 컴포넌트에게 상태 정보를 전달-->
        </form>
      </div>
    </div>
  `
})
export class AppComponent {
  // 자식 컴포넌트와 공유할 상태 정보
  users: User[]; // User모델을 만들어야 사용가능  

  constructor() {
    this.users = [
      new User(1, 'Lee', 'Administrator'),
      new User(2, 'Baek', 'Developer'),
      new User(3, 'Park', 'Designer')
    ];
  }

  // 사용자 추가
  addUser(name: string, role: string): void {
    if (name && role) {
      this.users = [...this.users, { id: this.getNextId(), name, role }];
    }
  }

  // 새로운 사용자의 id를 취득
  getNextId(): number {
    return this.users.length ? Math.max(...this.users.map(({ id }) => id)) + 1 : 1;
  }
}
```



```typescript
// models/user.model.ts
export class User {
  constructor(public id: number, public name: string, public role: string) { }
}
```

![@input-property-1](http://poiemaweb.com/img/@input-property-1.png)

@Input 데코레이터 바로 뒤의 프로퍼티명 users와 부모 컴포넌트에서 실행한 프로퍼티 바인딩의 프로퍼티명 users는 반드시 일치하여야 한다.

### 2.1.2 @Input 데코레이터와 setter를 이용한 입력 프로퍼티 조작

![@input-property-3](http://poiemaweb.com/img/@input-property-3.png)

 부모 컴포넌트가 전달한 데이터가 자식 컴포넌트의 **입력 프로퍼티에 바인딩되는 시점**에 필요한 로직을 동작시킬 수 있다.



## 2.2 자식 컴포넌트에서 부모 컴포넌트로 상태 전달



### 2.2.1 @Output 데코레이터와 EventEmitter

![child to parent](http://poiemaweb.com/img/childtoparent.png)



자식이 부모에게 이벤트를 보내면 부모는 **EventEmiiter로 이벤트를 감지**한다.  부모 컴포넌트는 자식 컴포넌트가 전달한 상태를 **이벤트 바인딩**을 통해 접수한다.

```typescript
<td>
    <button class="btn btn-danger btn-sm" (click)="remove.emit(user)">
        <!-- remove 실행, emit함수를 user로 받앗고,입
        <span class="glyphicon glyphicon-remove"></span>
	</button>
</td>
```



# 3. Stateful 컴포넌트와 Stateless 컴포넌트

참조하거나 삭제하거나 수정했다. Stateful 컴포넌트 (비순수)

모든 컴포넌트들을 Stateless 컴포넌트 (순수)



대형프로젝트에서는 폴더에있는 폴더들이 너무나 많으면;

상태를 많이 주고받는걸 아끼려 하다보니 statuful된다.



# 4. 원거리 컴포넌트 간의 상태 공유

![complex-component](http://poiemaweb.com/img/complex-component.png)**A 컴포넌트**에서 변경된 상태를 **C 컴포넌트에서도 공유할 필요**가 있을 때, 프로퍼티 바인딩과 이벤트 바인딩을 통해 상태를 공유할 수 있다.