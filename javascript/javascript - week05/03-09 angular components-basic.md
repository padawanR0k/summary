**컴포넌트**는 Angular의 핵심 구성 요소로서 Angular 애플리케이션은 컴포넌트를 중심(CBD, Component Based Development)으로 구성된다. 컴포넌트의 역할은 애플리케이션의 화면을 구성하는 **뷰(View)**를 생성하고 관리하는 것이다.

# 1. 컴포넌트 소개

## 1.1 웹 컴포넌트

**컴포넌트는 동작 가능한 하나의 부품이다.** 부품화를 위해서는 다른 컴포넌트에 간섭을 받지 않도록 독립된 스코프를 가져야 한다. 다시 말해 컴포넌트 내에서만 유효한 상태 정보와 로직, 스타일을 소유한 완결된 뷰를 생성하기 위한 것이 바로 컴포넌트이다. 컴포넌트는 독립적이고 완결된 뷰를 생성하기 위하여 **“HTML, CSS, JavaScript를 하나의 단위로 묶는 것”**으로 W3C 표준인 웹 컴포넌트(Web Component)를 기반으로 한다. Angular는 이러한 컴포넌트를 조립하여 **하나의 완성된 애플리케이션을 작성**한다.

![web-component](http://poiemaweb.com/img/web-component.png)



1. 컴포넌트의 뷰를 생성할 수 있어야 하며(HTML Template)
2. 외부로부터의 간섭을 제어하기 위해 뷰가 스코프(scope)를 분리하여 DOM을 캡슐화(Encapsulation)할 수 있어야 하며(Shadow DOM)
3. 외부에서 컴포넌트를 호출할 수 있어야 하고(HTML import)
4. 컴포넌트를 명시적으로 호출하기 위한 명칭(alias)을 선언하여 마치 HTML 요소와 같이 사용할 수 있어야 한다(Custom Element).



## 1.2 컴포넌트 트리

어떠한 복잡한 화면이라도 컴포넌트 하나로 생성하고 관리할 수 있다. 하지만 재사용이 가능한 부분이 존재하기 마련이기 때문에 하나의 컴포넌트로 화면 전체를 구성하는 것은 컴포넌트를 사용하는 것은 비효율적이다. 컴포넌트는 재사용이 용이한 구조로 분할하여 작성하며 이렇게 분할된 컴포넌트를 조립하여 중복없이 UI를 생성한다.

![HTML5 semantic elements](http://poiemaweb.com/img/building-structure.png)

위의 블록 구조를 컴포넌트로 전환하면 아래와 같은 구조를 갖는다. 흡사 DOM 트리와 유사한 형태를 가지게 되는데 이를 **컴포넌트 트리**라고 한다.

![component tree](http://poiemaweb.com/img/component-tree.png) 컴포넌트 간의 부모-자식 관계는 데이터와 이벤트가 왕래하는 정보 흐름의 통로가 되며 이를 통해 상태 공유가 이루어지기 때문에 컴포넌트 간의 부모-자식 관계는 Angular 애플리케이션에서 중요한 의미를 갖는다.

**설계 시점부터 화면을 어떠한 컴포넌트 단위로 분할할 것인지에 대한 검토가 필요하다.**



# 2. 컴포넌트 기본 구조



## 2.1 컴포넌트의 코드 구조

```typescript
// 컴포넌트는
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
// 생성자를 변형시킨다.
export class AppComponent { // export가 붙으면 모듈이된다. (import로 불려질 수 있으면)
  title = 'app';
}
```



#### 임포트 영역

컴포넌트에 필요한 외부 모듈을 임포트한다. Angular 라이브러리 모듈의 경우 @가 붙어있으며 경로를 명시하지 않는다. Angular 모듈이 아닌 외부 모듈의 경우 상대 경로를 명시하여야 한다.



#### @Component 데코레이터 영역

@Component 데코레이터에는 **메타데이터** 객체를 인자로 전달한다. 메타데이터는 컴포넌트 생성에 필요한 정보(셀렉터, 템플릿, 스타일 정의 등)를 담고 있는 객체이다. 예를 들어 메타데이터 객체의 templateUrl 프로퍼티에는 컴포넌트의 뷰를 정의한 html 파일인 **템플릿**의 상대경로를 설정한다.



#### 컴포넌트 클래스 영역

컴포넌트 뷰를 관리하기 위한 로직을 정의한다. @Component 데코레이터는 자신의 바로 아래에 위치한 클래스를 컴포넌트 클래스로 인식한다. 컴포넌트 클래스는 컴포넌트의 내부 관심사인 뷰의 관리에 집중해야 하며 애플리케이션 공통 관심사는 서비스로 분리하여야 한다.



컴포넌트 클래스에서는 view를 생성하기위한 모든 일을 한다.



## 2.2 컴포넌트의 기본 동작 구조

View는 상태를 가지고 있으며 로직은 그 상태를 보며 로직을 실행한다.

ex) 체크박스를 클릭시 특정 부분을 보여준다.



```html
<!-- rc/app.component.html -->
...
  <h1>
    Welcome to {{ title }}!
  </h1>
...
```

`{{ title }}`은 템플릿 문법 중 하나인 인터폴레이션(Interpolation)으로 컴포넌트 클래스의 데이터를 템플릿에 바인딩한다. 이러한 방식을 **데이터 바인딩**이라고 한다.

![data binding](http://poiemaweb.com/img/data-binding.png)

`{{ title }}`은 템플릿 문법 중 하나인 인터폴레이션(Interpolation)으로 컴포넌트 클래스의 데이터를 템플릿에 바인딩한다. 이러한 방식을 **데이터 바인딩**이라고 한다.



![component](http://poiemaweb.com/img/component.png)



#### 템플릿

컴포넌트의 뷰를 생성하기 위해 HTML과 Angular의 고유한 템플릿 문법로 작성됨

#### 메타데이터

컴포넌트 설정 정보를 담고 있는 객체이다. @Component 데코레이터는 메타데이터 객체를 인자로 전달받아서 컴포넌트 클래스에 반영한다.

#### 컴포넌트 클래스

컴포넌트의 뷰를 생성하는 템플릿의 상태(state)를 관리한다. **데이터 바인딩을 통해 템플릿에 필요한 데이터를 제공하거나 템플릿에서 발생한 이벤트를 처리**한다.









![template-class](http://poiemaweb.com/img/template-class.png)



1. input태그의 value에 이름입력후 버튼을클릭하면

2. input태그의 value를 인수로 해서  `setName()`호출

3. HellowComponent의 `name: string;` 상태가 변하게되고 그것을 템플릿이 감지하여 그 값을 가져간다.

   ​