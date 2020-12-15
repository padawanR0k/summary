>  *** 모든 객체는 결국 window의 프로퍼티이다.***



Built-in Object(내장 객체)는 웹페이지 등을 표현하기 위한 기능을 제공하며 웹페이지가 로드되자마자 바로 사용이 가능하다.

- Standard Built-in Objects (or Global Objects)
- BOM (Browser Object Model)
- DOM (Document Object Model)

**Standard Built-in Objects(표준 빌트인 객체)**를 제외한 BOM과 DOM을 **Native Object**라고 분류하기도 한다. 또한 사용자가 생성한 객체를 Host Object(사용자 정의 객체)라 한다.



>  통틀어 API라고도 한다. API는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.  개발자와 컴퓨터간에 편하게 소통하기 위한 표준적인 기능 



![object](http://poiemaweb.com/img/object.png)



- Native Object : 브라우저 자체에 관련된 객체

  - BOM
    - 브라우저의 스크롤, 주소창, 뒤로가기버튼 등  

  ​

# 1. Standard Built-in Objects 

자바스크립트는 사용자가 프로그램 전체의 영역에서 공통적으로 필요한 기능을 일일이 작성안해도 되게끔 Standard Built-in Object(표준 빌트인 객체)를 제공한다. String, Array, Number 처럼 대문자로 시작한다.





---



# 2. BOM (Browser Object Model)

BOM은브라우저탭 또는 브라우저창의 모델을 생성한다. 최상위 객체는 `window`객체로 현재 브라우저 창 또는 탭을 표현하는 객체이다.  이 객체의 자식 객체 들은 브라우저의 다른 기능들을 표현한다. 

![BOM](http://poiemaweb.com/img/BOM.png)

location은 Location()생성자함수로 만들어진다.

몇몇 속성들은 Location() 생성자함수에 있지만, 몇몇 속성들은 Location.prototype에만 있다.

location인스턴스에서 자신의 프로토타입의 메서드를 호출할때 prototype을 붙이지않고 호출하는 메서드를 스태틱메서드라고 부른다.

그렇다면 왜 나눠서 두는것인가?





---



# 3. DOM (Document Object Mode

문서 객체 모델은 현재 웹페이지의 모델을 생성한다. 최상위 객체는 `document` 객체로 전체 문서를 표현한다. 또한 이 객체의 자식 객체들은 문서의 다른 요소들을 표현한다. 이 객체들은 Standard Built-in Objects가 구성된 후에 구성된다.

![DOM](http://poiemaweb.com/img/DOM.png)