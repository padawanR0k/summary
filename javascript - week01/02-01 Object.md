# 1. 객체(Object)란?

자바스크립트는 객체(object)기반의 스크립트 언어이며 자바스크립트를 이루고 있는 거의 “모든 것”은 객체이다. 기본자료형을 제외한 나머지 값들(함수, 배열, 정규표현식 등)은 모두 객체이다.

기본자료형(함수, 배열, 정규표현식 등)을 제외한 모든건 객체이다.

데이터를 한 곳에 모으고 구조화 하는데 유용하다.

```js
var person = {
  name: 'lee',
  age: 20,
  speak : function(){
    
  }
  // 프로퍼티(property) : 객체의 속성 name, age 
  // 메소드(method) : 함수를 값으로 가진 프로퍼티 speak
}
```

추상화 : 객체를 표현할때 관심있는 대상만(프로퍼티)을  표현한 것

상속 : 부모의 자원을 물려받는것. 자원이란 프로퍼티와 메소드를 뜻한다.





프로토타입이란? 상속을 표현하는 방식 ; 친부모가 바로생긴다고 비유

---



## 1- 1. 프로퍼티 (Property)

객채는   프로퍼티 (name : value)를 포함하는 컨테이너 

name 는 빈문자열을 포함하는 문자열과 숫자가 올 수 있다.

value는  `undefind` 값을 제외한 모든 값이 올 수 있다.

- 프로퍼티 이름 : 빈 문자열을 포함하는 문자열
- 프로퍼티 값 : `undefined`을 제외한 모든 값


---



## 1- 2. 메소드(Method)

객체에 제한되어 있는 함수에 의미함.  프로퍼티 값이 함수이면 메소드라 칭한다.



***



# 2. 객체 생성 방법



## 2- 1. 객체 리터럴



중괄호({})를 사용하여 객체를 생성하는데 {} 내에 아무것도 기술하지 않으면 빈 객체가 생성된다.  빈 객체는 undefined가 아니다.

```js
var emptyObject = {}; // syntax sugar
console.log(typeof emptyObject); // object
```



```js
var person = {
  name: 'Lee',
  gender: 'male',
  sayHello: function () {
    console.log('Hi! My name is ' + this.name);
  }
};
```

 객체 리터럴에 의한 객체 생성 방식의 특징은 생성자 함수("new "가붙은 함수) 사용한 객체 생성 방식과는 달리 new 연산자를 사용할 필요없이 선언과 동시에 인스턴스가 생성된다는 것이다.



---

## 2- 2. Object() 생성자 함수

built-in되어있는 함수이다.  

> Q. 객체리터럴과 다른게 무었인가?
>
> A. 다를것은 없다. 객체리터럴로 객체를 생성할 경우 브라우저는 Object() 생성자 함수를 불러와 만든다. 사람의 눈에만 안보일 뿐. 자동적으로 Object함수가 실행되는것이다. 객체리터럴`{...}`은 `new Object()`의 축약법이다.

```js
// 빈 객체를 만든다.
var person = new Object();

// 프로퍼티 추가 
// 위의 객체리터럴로 만드는것과 결과가 똑같다.
person.name = 'Lee';
person.gender = 'male';
person.sayHello = function () {
  console.log('Hi! My name is ' + this.name);
};
```



---



## 2- 3. 생성자 함수

사실 생성자 함수라는 말은 없다. 편의를 위해 만들어낸 말이다.

함수는 `new`키워드를 같이쓰면 생성자 함수가 되는것이다.

함수를 생성자 함수로 쓰는 경우에는 함수명 첫글자를 대문자로하는게 일반적이다.



아래의 경우 동일한 프로퍼티를 갖는 객체임에도 불구하고 매번 같은 프로퍼티를 기술해야한다.

```js
var person1 = {
  name: 'Lee',
  sayHello: function () {
    console.log('Hi! My name is ' + this.name);
  }
};

var person2 = {
  name: 'Kim',
  sayHello: function () {
    console.log('Hi! My name is ' + this.name);
  }
};
```

이 때 생성자 함수를 사용하면 템플릿처럼 사용하여 구성이 동일한 객체를 간편하게 생성할 수 있다.



```js
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
  this.sayHello = function(){
    console.log('Hi! My name is ' + this.name);
  };
}

// 인스턴스의 생성
var person1 = new Person('Lee', 'male');
var person2 = new Person('Kim', 'female');

console.log('person1: ', typeof person1);
console.log('person2: ', typeof person2);
console.log('person1: ', person1);
console.log('person2: ', person2);

person1.sayHello();
person2.sayHello();
```

this는 생성자함수가 생성해내는 객체 자기자신을 뜻한다.



this의 이해

```js
var person1 = new Person('Lee', 'male');

// 인스턴스가 생성될때 이런식으로 할당이 된다.
function Person(name, gender) {
  person1.name = 'Lee';
  person1.gender = 'male';
  person1.sayHello = function(){
    console.log('Hi! My name is ' + person1.name);
  };
} 
```



Person()

| name     | 'Lee'                               |
| -------- | ----------------------------------- |
| gender   | 'male'                              |
| sayHello | 객체, 함수다, 참조값이 들어온다. address값이 들어온다. |

sayHello()

|      |          |
| ---- | -------- |
|      |          |
|      | sayHello |

객체를 재사용을 하고싶으면 생성자함수를 쓴다.



> 나의 이해:
>
> **객체리터럴**은 new Object 생성자함수의 축약법, 객체하나를 만들때 유리하다. 
>
> ex) 미술가가 독창적인 그림을 그릴때는 그 그림을 여러개 만들필요가 없다.
>
> 
>
> **생성자 함수**는 객체를 재사용하기위해서 사용하는 것이다. 
>
> ex) 붕어빵을 만들때 붕어빵틀을 생각해보자. 틀이 없다면 같은 모양의 붕어빵을 만들때 매우 불편할 것이다. 생성자함수를 이용하면 붕어빵의 앙금의 종류도 바꾸는게 편하다.
>
> 여기서 앙금은 property 앙금의 종류는 property의 value



- 생성자 함수 이름은 일반적으로 대문자로 시작한다. 이것은 생성자 함수임을 인식하도록 도움을 준다.

- 프로퍼티 또는 메소드명 앞에 기술한 `this`는 생성자 함수가 생성할 **인스턴스(instance)**를 가리킨다.

  - 생성자함수로 만들어낸 객체를 *인스턴스*라고한다.

- this에 연결(바인딩)되어 있는 프로퍼티와 메소드는 `public`(외부에서 참조 가능)하다.

- 생성자 함수 내에서 선언된 일반 변수는 `private`(외부에서 참조 불가능)하다. 즉 생성자 함수 내부에서는 자유롭게 접근이 가능하나 외부에서 접근할 수 없다.

  ​

***



# 3. 객체 프로퍼티 접근



## 3- 1. 프로퍼티 이름

**프로퍼티 이름의 문자열이므로 따옴표(‘’ 또는 ““)를 사용한다.**  연산자가 포함된 프로퍼티가 아닐 경우 따옴표를 생략가능하다.

```ㅓㄴ
var person = {
  'first-name': 'Ung-mo',
  'last-name': 'Lee',
  gender: 'male',
  1: 10,
  function: 1 // OK. 하지만 예약어는 사용하지 말아야 한다.
};

console.log(person.function);
```



---



## 3- 2. 프로퍼티 값 읽기



- 마침표(.) 표기법
- 대괄호([]) 표기법

```js
var person = {
  'first-name': 'Ung-mo',
  'last-name': 'Lee',
  gender: 'male',
  1: 10
};

console.log(person);


console.log(person[first-name]);   // ReferenceError: first is not defined
console.log(person['first-name']); // 'Ung-mo'

console.log(person.gender);    // 'male'
console.log(person[gender]);   // ReferenceError: gender is not defined
console.log(person['gender']); // 'male'

console.log(person['1']); // 10
console.log(person.1);    // SyntaxError
```

대괄호([]) 표기법을 사용하는 경우, **대괄호 내에 들어가는 프로퍼티 이름은 반드시 문자열이어야 한다.**



---



## 3- 3. 프로퍼티 값 갱신, 동적 생성, 삭제

```js
var person = {
  'first-name': 'Ung-mo',
  'last-name': 'Lee',
  gender: 'male',
};

// 값 갱신
person['first-name'] = 'Kim';
/// 이 경우 값이 바뀌는것이아니라 메모리에서 Kim을 추가하고 first-name의 포인터를 Kim으로 바꾼다.

// 동적 생성
person.age = 20;
/// 이처럼 객체는 동적으로 추가,제거가 가능(mutable)하기 때문에 Heap
/// 기본자료형은 객체처럼 동적으로 추가,제거가 불가능(immutable)하기 때문에 Stack

delete person.gender; // gender 속성을 삭제함
```



---



# 4. Pass-by-reference

값을 전달할때 참조값이 넘어간다는것.



![설명그림](http://poiemaweb.com/img/pass-by-ref.png)

```js
var foo = {
  val : 10
}

var bar = foo; // 1. bar가 foo와 객체를 공유하게된다.
console.log(foo.val, bar.val); // 10 10
console.log(foo === bar);      // true

bar.val = 20; // 2. 그래서 둘다 바뀐다.
console.log(foo.val, bar.val); // 20 20  
console.log(foo === bar);      // true
```

 변수 foo, bar 모두 동일한 객체(`val: 10`)를 참조하고 있다. 따라서 참조하고 있는 객체의 val 값이 변경되면 변수 foo, bar 모두 동일한 객체를 참조하고 있으므로 두 변수 모두 변경된(`20`) 객체의 프로퍼티 값(`val: 20`)을 **공유**하게 된다.



![pass-by-reference](http://poiemaweb.com/img/pass-by-ref.png)

위 처럼 참조가 되는현상을 사이드이펙트라고 부른다. 사이드이펙트가 없으면 순수함수를 지향한다.



다른 경우

![pass-by-reference](http://poiemaweb.com/img/pass-by-ref-2.png)

```js
var foo = { val: 10 };
var bar = { val: 10 };

console.log(foo.val, bar.val); // 10 10
console.log(foo === bar);      // false

var baz = bar;

console.log(baz.val, bar.val); // 10 10
console.log(baz === bar);      // true
```



baz와 변수 bar의 참조값(address)은 동일하나, foo와는 동일하지 않다.



***



# 5. 객체의 분류



![object](http://poiemaweb.com/img/object.png)

Built-in Object : 브라우저내장 

Host Object : 객체리터럴, 생성자 함수

Standard Built-in Object : 프로그램 전체의 영역에서 공통적으로 필요한 기능을 개발자 각자가 일일히 작성하는 수고를 줄이기 위한 표준 빌트인 객체

Native Object : 주로 도큐멘트와 관계있음

BOM : 스크롤, 브라우저 창크기와 같은것과 관계있음