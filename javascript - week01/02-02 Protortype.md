# 1. 프로토타입 객체

모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있다. (\__proto__) 이것은 마치 객체 지향의 상속개념과 같이 부모객체의 프로퍼티 또는 메소드를 상속받아 사용할 수있게한다. 이러한 부모역할을 하는 객체를 **ptototype**이라한다.

*Prototype 객체는 생성자 함수에 의해 생성된 각각의 객체에 **공유 프로퍼티**를 제공하기 위해 사용*

```js
var student = {
  name: 'Lee',
  score: 90
};

// student에는 hasOwnProperty 메소드가 없지만 아래 구문은 동작한다.

// 생성자 함수 new Object()로 만들어진 객체리터럴이기 땜누에 student의 __proto__를 따라가면 Object.prototype이나오고 Object.prototype에 built-in 되어있는 hasOwnProperty() 메소드가 실행되었기 때문이다.
console.log(student.hasOwnProperty('name')); // true

console.dir(student);
```

---


#2. \__proto__ 프로퍼티 vs prototype 프로퍼티

- \__proto__ 
  - 모든객체가 가지고있음
  - 부모역할을 하는 객체를 가르킴
- prototype 
  - 생성자함수객체가 가지고있음
  - 생성자함수의 프로토타입을 가르킴


---



# 3. constructor 프로퍼티

프로토타입 객체는 constructor 프로퍼티를 갖는다. 이 constructor프로퍼티는 객체의 입장에서 자신을 생성한 객체를 가리킨다. 



---


# 4. Prototype chain

객체를 생성하는 방식은 3가지가 있다. 3가지 객체 생성 방식에 의해 생성된 객체의 prototype 객체를 정리해 보면 아래와 같다.

| 객체 생성 방식        | 엔진의 객체 생성       | 인스턴스의 prototype 객체  |
| --------------- | --------------- | ------------------- |
| 객체 리터럴          | Object() 생성자 함수 | Object.prototype    |
| Object() 생성자 함수 | Object() 생성자 함수 | Object.prototype    |
| 생성자 함수          | 생성자 함수          | 생성자 함수 이름.prototype |



![Object literal Prototype chaining](http://poiemaweb.com/img/object_literal_prototype_chaining.png)

객체 리터럴은 Object.prototype에 올리지못한다. 그러므로

 중복되지않는 객체를 생성할때만 객체리터럴을 사용하자.





---





# 5. 프로토타입 객체의 확장

프로토타입 객체도 객체이므로 일반 객체와 같이 프로퍼티를 추가/삭제할 수 있다.

```js
function Person(name) {
  this.name = name;
}

var foo = new Person('Lee');

Person.prototype.sayHello = function(){
  console.log('Hi! my name is ' + this.name);
};

foo.sayHello(); // Hi! my name is Lee
```

![extension of prototype](http://poiemaweb.com/img/extension_prototype.png)



1. foo()는 sayHello()가 없는데? 
2. 부모의 역할인 Person.prototype에는 sayHello()가 있네?
3. 그럼 가져다 쓰자 
4. Hi! my name is Lee


---




# 6. 기본자료형(Primitive data type)의 확장

자바스크립트는 기본자료형을 참조형으로 쓰려고하면 그때만 기본자료형을 참조형으로 바꾼다. 그래서 객체처럼 볼 수 있는것이다.

```js
var str = 'test';
console.log(typeof str);                 // string
console.log(str.constructor === String); // true 이 구문이 실행되는 이유가 잠시 객체화되기 때문이다.
console.dir(str); // test   객체화가 끝나서 test라는 구문만 나온다.

var strObj = new String('test');
console.log(typeof strObj);                 // object
console.log(strObj.constructor === String); // true
console.dir(strObj);

console.log(str.toUpperCase());    // TEST
console.log(strObj.toUpperCase()); // TEST

// ===================
var str = 'test';

// 객체처럼 행동해서 에러가 발생하지 않는다.
str.myMethod = function () {
  console.log('str.myMethod');
};

str.myMethod(); // Uncaught TypeError: str.myMethod is not a function 기본자료형이라서 메소드 추가는 불가능하다.  마치 Read는 가능한데, write가 안되는느낌

```



String 객체의 프로토타입 객체 String.prototype에 메소드를 추가하면 기본자료형, 객체 모두 메소드를 사용할 수 있다.

```js
var str = 'test';

String.prototype.myMethod = function () {
  return 'myMethod';
};

console.log(str.myMethod());      // myMethod 이제 모든 문자열객체에 mymMethod()를 사용가능해졌다.
console.log('string'.myMethod()); // myMethod
```



![String constructor function prototype chaining](http://poiemaweb.com/img/string_constructor_function_prototype_chaining.png)



---



# 7. 프로토타입 객체의 변경

자바스크립트의 상속은 결국 프로토타입체이닝이다.

객체를 생성할 때 프로토타입은 결정된다. 결정된 프로토타입 객체는 임의의 객체로 변경할 수 있다. 이것은 부모 객체인 프로토타입을 **동적으로 변경할 수 있다는 것**을 의미한다. 

```js
function Person(name) {
  this.name = name;
}

var foo = new Person('Lee');

// 프로토타입 객체의 변경
Person.prototype = { gender: 'male' };

var bar = new Person('Kim');

console.log(foo.gender); // undefined 
console.log(bar.gender); // 'male'   // 객체리터럴로 생성했기 때문에 체이닝이 끊겻다.

console.log(foo.constructor); // ① Person(name)
console.log(bar.constructor); // ② Object()
```



객체를 변경할 때 객체리터럴로 변경하면 constructor가 깨진다.

![changing prototype](http://poiemaweb.com/img/changing_prototype.png)