Javascript는 **프로토타입 기반** 객체지향형 언어다.

하위 객체들이 상위 객체들을 참조할 수 있는 환경이 자동적으로 구현이되어있는 언어이다. 그리고 다른언어와달리 자바스크립트는 객체를 만드는데 클래스가 필요없다.



ES5에서는 생성자 함수와 프로토타입을 사용하여 객체 지향 프로그래밍을 구현

```js
// ES5
var Person = (function () {
  // private member
  var _name = '';

  // Constructor
  function Person(name) {
    setName(name);
  }

  // private mathods
  function setName(name) { // 외부에서 참조불가
    _name = name;
  }

  function getName() { // 외부에서 참조불가
    return _name;
  }

  // public methods
  Person.prototype = { // Person 변수의 prototype객체에 메소드를 추갛면서 내부함수를 호출함
    sayHi: function () {
      console.log('Hi! ' + getName());
    },
    sayBye: function () {
      console.log('Bye~ ' + getName());
    }
  };

  // return constructor
  return Person;
}());

var me = new Person('Lee');
me.sayHi(); // Hi! Lee
me.sayBye(); // Bye~ Lee

console.log(me instanceof Person); // true
```

ES6의 클래스는 기존 프로토타입 기반 객체지향 프로그래밍보다 클래스 기반 언어에 익숙한 프로그래머가 보다 빠르게 학습할 수 있는 단순명료한 새로운 문법을 제시하고 있다. 



# 1. 클래스 정의 (Class Definition)

- 클래스 내부에는 반드시 메서드만 있어야한다.

```js
// class도 함수다! type of Person ==== Function
class Person {  // 선언문함수여서 함수호이스팅이 일어난다. 하지만 선언문과 초기화단계의 사이에서 일시적사각지대가 생기기때문에 선언전에 호출하면 에러가 난다.
  constructor(name) {
    this._name = name;
  }

  sayHi() {
    console.log(`Hi! ${this._name}`);
  }
}

const me = new Person('Lee');
me.sayHi(); // Hi! Lee

console.log(me instanceof Person); // true
```





# 2. 인스턴스의 생성

클래스의 인스턴스를 생성하려면 new 연산자와 함께 생성자를 호출한다.

```js
class Foo {}

const foo = new Foo();	
const bar = Foo();	//  class 생성자함수로 인스턴스를 생성할때 new연산자를 붙이지 않으면 타입에러가 발생한다.
// TypeError: Class constructor Foo cannot be invoked without 'new' 

```

# 3. constructor
# 4. 멤버 변수
# 5. 호이스팅(Hoisting)

---

# 6. getter, setter
## 6.1 getter
## 6.2 setter

---

# 7. 정적 메소드 (Static method)

---


# 8. 클래스 상속 (Class Inheritance)
## 8.1 extends 키워드
## 8.2 super 키워드
## 8.3 static 메소드와 prototype 메소드의 상속