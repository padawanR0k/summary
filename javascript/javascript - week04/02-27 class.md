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

ES6의 클래스는 기존 프로토타입 기반 객체지향 프로그래밍보다 클래스 기반 언어에 익숙한 프로그래머가 보다 빠르게 학습할 수 있는 단순한 새로운 문법을 제시하고 있다.



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

**constructor**는 인스턴스를 생성하고 초기화하기 위한 특수한 메소드이다.

클래스 내에 **한 개만** 존재할 수 있으며 만약 클래스가 2개 이상의 constructor 메소드를 포함하면 SyntaxError가 발생한다.

```js
class Bar {
  // constructor는 인스턴스의 생성과 동시에 멤버 변수의 생성 및 초기화를 실행할 수 있다.
  constructor(num) {
    this.num = num; // this에 바인딩된 변수는 멤버변수
  }
}

console.log(new Bar(1)); // Bar { num: 1 }
```



# 4. 멤버 변수

클래스 바디에는 **메소드만을 포함**할 수 있다. 클래스 바디에 멤버 변수를 선언하면 SyntaxError가 발생한다.

```js
class Foo {
  let name = ''; // SyntaxError

    constructor() {
         this.name = name; // 멤버변수의 선언과 초기화
    }
}
console.log(new Foo('Lee')); // Foo { name: 'Lee' }

const foo = new Foo('Lee');
console.log(foo.name); // Lee 멤버변수는 외부에서 참조가능하다. (public)
```



# 5. 호이스팅(Hoisting)

이스팅되지 않는 것처럼 동작한다. 즉 class 선언문 이전에 class를 참조하면 ReferenceError가 발생한다.

자바스크립트는 ES6의 class를 포함하여 모든 선언을 호이스팅한다. 하지만 클래스는 스코프의 선두에서 class의 선언까지 **일시적 사각지대(TDZ)**에 빠지게 되기 때문에 **class 선언문 이전에** class를 참조하면 ReferenceError가 발생한다.

```js
const foo = new Foo(); // ReferenceError: Foo is not defined
class Foo {}
```



---

# 6. getter, setter



## 6.1 getter

```js
class Foo {
  constructor(arr = []) {
    this._arr = arr;
  }

  // getter: firstElem은 멤버 변수 이름과 같이 사용된다.
  // getter는 반드시 무언가를 반환하여야 한다.
  get firstElem() { // 변수를 가져오면서 변경할이유가 있다면 getter를 쓴다.
    return this._arr.length ? this._arr[0] : null;
  }
}

const foo = new Foo([1, 2]);
// 프로퍼티 firstElem에 접근하면 getter가 호출된다.
console.log(foo.firstElem); // 1
```

getter는 어떤 멤버 *변수에 접근*할 때마다 **멤버 변수의 값을 조작하는 행위가 필요**할 때 사용한다. 

returen 이 꼭 잇어야함



## 6.2 setter

setter는 어떤 멤버 *변수에 값을 할당*할 때마다 멤버 변수의 값을 조작하는 행위가 필요할 때 사용한다. 

매개변수가 꼭 있어야함

```js
class Foo {
  constructor(arr = []) {
    this._arr = arr;
  }

  // getter: firstElem은 멤버 변수 이름과 같이 사용된다.
  get firstElem() {
    return this._arr.length ? this._arr[0] : null;
  }

  // setter: firstElem은 멤버 변수 이름과 같이 사용된다.
  set firstElem(elem) {
    // ...this._arr은 this._arr를 개별 요소로 분리한다
    this._arr = [elem, ...this._arr];
  }
}

const foo = new Foo([1, 2]);

// 멤버 변수 lastElem에 값을 할당하면 setter가 호출된다.
foo.firstElem = 100;

console.log(foo.firstElem); // 100
```



---

# 7. 정적 메소드 (Static method)

인스턴스화(instantiating)없이 클래스 이름으로 호출하며 클래스의 **인스턴스로 호출할 수 없다.** 

대부분의 유틸리티함수들이 정적(static)메소드이다. ex) Math

```js
class Foo {
  constructor(prop) {
    this.prop = prop;
  }

  static staticMethod() { // Foo.staticMethod
    return 'staticMethod';
  }

  prototypeMethod() { // Foo.prototype.prototypeMethod
    return 'prototypeMethod';
  }
}

const foo = new Foo(123);

console.log(Foo.staticMethod());
console.log(foo.staticMethod()); 


// es5 버전
var Foo = (function () {
  function Foo(prop) {
    this.prop = prop;
  }

  Foo.staticMethod = function () {
    return 'staticMethod';
  };

  Foo.prototype.prototypeMethod = function () {
    return 'prototypeMethod';
  };

  return Foo;
}());

var foo = new Foo(123);

console.log(Foo.staticMethod());
console.log(foo.staticMethod()); // Uncaught TypeError: foo.staticMethod is not a function

```

함수 객체는 prototype 프로퍼티를 갖는데 일반 객체의 [[Prototype]] 프로퍼티와는 다른 것이며 일반 객체는 prototype 프로퍼티를 가지지 않는다.



```js
class Foo {
  constructor(prop) {
    this.prop = prop;
  }
  static staticMethod() {
    return 'staticMethod';
  }
  prototypeMethod() {
    return 'prototypeMethod';
  }
}
const foo = new Foo(123);

console.log(typeof Foo.staticMethod); // function
console.log(Foo.staticMethod());      // staticMethod

console.log(typeof Foo.prototype.prototypeMethod); // function
console.log(foo.prototypeMethod());                // prototypeMethod

console.log(foo.staticMethod()); // TypeError: foo.staticMethod is not a function
```



![class prototype](http://poiemaweb.com/img/class-prototype.png)



---

# 8. 클래스 상속 (Class Inheritance)

**코드 재사용**의 관점에서 매우 유용하다. 새롭게 정의할 클래스가 기존에 있는 **클래스와 매우 유사하다면, 동일한 구현은 상속을 통해 그대로 사용하고 다른 점만 구현**하면 된다.

Sass의 mixin과 같이 시니어개발자가 다른 모든 개발자가 쓸 공통적인부분을 만들어주면 협업하는 과정에서 매우 효율적으로  개발을 진행할 수 있다.

## 8.1 extends 키워드



```js
// Base Class
class Animal {
  constructor(weight) {
    this._weight = weight; // Animal의 여러 속성중 관심이 있는 속성(체중) -> 추상화
  	// db의 column부분과 관련이 있다.  
  }

  weight() { // 이 메소드는 static을 안붙였으므로 prototype에 저장
    // 대부분의 메소드는 데이터를 조작하기위하여 작성한다.
    console.log(this._weight);
  }

  eat() { console.log('Animal eat.'); }
}

// Sub Class
class Human extends Animal { // 사람은 동물이지만  동물은 사람이 아님 -> 사람의 개념이 동물의 개념보다 복잡함 -> 동물의 개념을 일부분 가져오고 거기에 사람의 개념을 추가
  constructor(weight, language) { 
    super(weight); // 상속받는 클래스의 인스턴스를 생성하고 상속받는 객체의 constructor호출 
    //동물과 공통적인 부분을 super()로 가져와서 씀 (재사용)
    
    this._language = language; // Human 만 가지고있는 특별한 속성
  }

  // 부모 클래스의 eat 메소드를 오버라이드하였다
  eat() { console.log('Human eat.'); }

  speak() {
    console.log(`Koreans speak ${this._language}.`);
  }
}

const korean = new Human(70, 'Korean');
korean.weight(); // 70
korean.eat();    // Human eat.
korean.speak();  // Koreans speak Korean.

console.log(korean instanceof Human);  // true
console.log(korean instanceof Animal); // true
```

오버로딩(Overloading) : 



오버라이딩(Overriding) : 이미 정의되어 있는 메서드를 다시 정의하여 사용하는 것

![class-extends](http://poiemaweb.com/img/class-extends.png)

## 8.2 super 키워드

**부모 클래스의 프로퍼티를 참조(Reference)할 때 또는 부모 클래스의 constructor를 호출할 때 사용한다.**

```js
class Parent {
  constructor(x, y) {
    this._x = x;
    this._y = y;
  }

  toString() {
    return `${this._x}, ${this._y}`;
  }
}

class Child extends Parent {
  constructor(x, y, z) {
    super(x, y); // 부모 Parent의 constructor
    this._z = z;
  }

  toString() {
    // 오버라이딩됨
    return `${super.toString()}, ${this._z}`; // Parent의 메소드를 사용
  }
}

const child = new Child(1, 2, 3);
console.log(child.toString()); // 1, 2, 3
```

**자식 클래스의 constructor에서 super()를 호출하지 않으면 ReferenceError가 발생한다.**

```js
class Parent {}

class Child extends Parent {
  constructor() {} // ReferenceError: this is not defined
}
```



super()를 호출해야 this를 참조할 수 있다.

```js
class Parent {
  constructor(x) {
    this._x = x;
  }
}

class Child extends Parent {
  constructor(x, y) {
    // console.log(this); // ReferenceError: this is not defined
    super(x);
    this._y = y;
    console.log(this); // Child { _x: 1, _y: 2 }
  }
}

const child = new Child(1, 2);
```





## 8.3 static 메소드와 prototype 메소드의 상속