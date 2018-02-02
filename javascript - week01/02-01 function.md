함수는 객체이다. 다른객체와 다른점은 호출이 가능하다는 점이다.

*효율성이 좋으려면 코드독해가 잘되야한다.  코드독해가 잘되려면 가독성이 좋아야한다.*



# 1. 함수 정의



- 함수선언식
- 함수표현식
- Function() 생성자 함수

> Function()생성자함수가  함수선언식, 함수표현식의 부모
>
> 함수표현식.\__proto__ === Function.prototype



---



## 1- 1. 함수선언식

함수선언식을 사용한 함수 정의는 `function` 키워드로 시작된다.

호출할때 함수명으로 한다.

```js
function square(number) {
  // 이곳은 원래는 방어구문이 들어갈 자리
  return number * number;
}

square이 함수명 number는 매개변수
```



## 1- 2. 함수표현식

자바스크립트의 함수는 **일급객체**이다.

> 1. 무명의 리터럴로 표현이 가능하다.
> 2. 변수나 자료 구조(객체, 배열…)에 저장할 수 있다.
> 3. 함수의 매개변수(파라미터)로 전달할 수 있다.
> 4. 반환값(return value)으로 사용할 수 있다.

변수명으로 호출한다.





함수는 일급객체이기 때문에 변수에 할당할 수 있는데 이 변수는 함수명이 아니라 할당된 함수를 가리키는 참조값을 저장하게 된다. 함수 호출시 함수명이 아니라 함수를 가리키는 변수명을 사용하여야 한다.

```js
var square = function(number) {
  return number * number;
};

// 함수객체를 변수에 담았다.
			function(number) {
  return number * number;
};
```

함수표현식으로 정의한 함수는 함수명을 생략(`function(number)`)할 수 있다. 이러한 함수를 **익명 함수(anonymous function)**이라 한다. 함수표현식에서는 함수명을 생략하는 것이 일반적이다.





![anonymous function](http://poiemaweb.com/img/anonymous_function.png)

```js
var foo = function(a, b) {
  return a * b;
};

var bar = foo;

console.log(foo(10, 10)); // 100
console.log(bar(10, 10)); // 100
```

함수가 할당된 변수를 사용해 함수를 호출하지 않고 기명 함수의 함수명을 사용해 호출하게 되면 에러가 발생한다. 이는 *함수표현식에서 사용한 함수명은 외부 코드에서 접근 불가능*하기 때문이다.



##  1- 3. Function() 생성자 함수

함수선언식과 함수표현식은 모두 함수 리터럴 방식으로 함수를 정의하는데 이것은 결국 내장 함수 Function() 생성자 함수로 함수를 생성하는 것을 단순화시킨 short-hand(축약법)이다.

```js
// 결국에는 이렇게 된다.
var square = new Function('number', 'return number * number');
console.log(square(10)); // 100
```



***함수선언식을 만나면 함수표현식으로 바꾼다???***



***



# 2. 변수 호이스팅과 함수 호이스팅

## 

##2- 1. 변수 호이스팅

변수 선언문이 최상단에 옮겨놓은것 같이 동작하는것.

1,2 ---> 3

> 실행 컨택스트 란?
>
> ①선언단계 : 자바스크립트는 한줄한줄읽는게아니라 한번에 훑고 그때 선언문(var선언,function)을 찾는다. **선언문을 찾아서 실행 컨텍스트의 VO에 저**장해 놓는다. 
>
> ②초기화단계 : 메모리에  **저장을해야하는데 타입을 모르니 undefined를 할당** 해놓는다.
>
> ③할당단계 : runtime에 메모리에 실제값을 넣고 포인터를 실제값이 있는곳으로 욺긴다.



---



## 2- 2. 함수 호이스팅

```js
var res = square(5);
res1(1);
function square(number) {
  return number * number;
} // 이것은 함수선언식
```



**함수선언식**에서는 선언단계,초기화단계, 할당단계가 동시에 일어난다.

1,2,3 동시에



```js
var res = square(5); // TypeError: square is not a function

var square = function(number) {
  return number * number;
} 이것은 함수표현식
```

**함수표현식**은 변수호이스팅과 같다. 

왜냐? 변수에다가 함수를 할당하기 때문에

***



# 4. 매개변수



## 4- 1. 매개변수(parameter, 인자) vs 인수(argument)

```js
var foo = function (p1, p2) {
  console.log(p1, p2);
};

foo(1); // 1 undefined  p2의 인수가 없어서 undefined가 나왔다.   ①선언단계 ②초기화단계가 진행된 상황
```

변수가 할당되는곳이 매개변수(parameter, 인자)

인수는 매개변수에 전달하는 값

`p1,p2`는 매개변수  

`1`은 인수



---



## 4- 2. Call-by-value

```js
function foo(primitive) {
  primitive += 1;
  return primitive;
}

var x = 0;

console.log(foo(x)); // 1
console.log(x);      // 0
```

매개변수도 변수이다. 그러므로 `x`의 값은 바뀌지않는다. 이런 함수를 순수함수라고 부른다.



---





## 4- 3. Call-by-reference

![call-by-value & call-by-reference](http://poiemaweb.com/img/call-by-val&ref.png)

```js
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Kim';
  obj.gender = 'female';
}

var num = 100;
var obj = {
  name: 'Lee',
  gender: 'male'
};

console.log(num); // 100
console.log(obj); // Object {name: 'Lee', gender: 'male'}

changeVal(num, obj);

console.log(num); // 100
console.log(obj); // Object {name: 'Kim', gender: 'female'}
```

체형(참조형) 인수는 **Call-by-reference**(참조에 의한 호출)로 동작한다. 함수 내에서 매개변수의 참조값이 이용하여 객체의 값을 변경했을 때 전달되어진 참조형의 인수값도 같이 변경된다.



---



## 5. 반환값

함수는 자신을 호출한 코드에게 수행한 결과를 반환(return)할 수 있다.

- `return` 키워드는 함수를 호출한 코드(caller)에게 값을 반환할 때 사용한다.
- 함수는 배열 등을 이용하여 한 번에 여러 개의 값을 리턴할 수 있다.
- 함수는 반환을 생략할 수 있다. 이때 함수는 암묵적으로 undefined를 반환한다.
- 자바스크립트 해석기는 `return` 키워드를 만나면 함수의 실행을 중단한 후, 함수를 호출한 코드로 되돌아간다. 만일 `return` 키워드 이후에 다른 구문이 존재하면 그 구문은 실행되지 않는다.

???????????????????????????????



***



# 6. 함수 객체의 프로퍼티



함수는 객체이다. 따라서 함수도 프로퍼티를 가질 수 있다.

함수는 일반 객체와는 다른 함수만의 표준 프로퍼티를 갖는다.

```js
function square(number) {
  return number * number;
}
console.dir(square);
```

![function property](http://poiemaweb.com/img/function_property.png)



---



## 6- 1. arguments 프로퍼티 

자바스크립트는 함수 호출 시 함수 정의에 따라 인수를 전달하지 않아도 에러가 발생하지 않는다.

```js
function multiply(x, y) {
  console.log(arguments); // arguments ? 실행컨택스트때 자바스크립트엔진이 만들어준다.
  return x * y;
}

// 매개변수의 갯수에 맞지않게 인수를 주면? 
multiply();        // {}
multiply(1);       // { '0': 1 } //전달되지 않은 매개변수는 undefined으로 초기화
multiply(1, 2);    // { '0': 1, '1': 2 }
multiply(1, 2, 3); // { '0': 1, '1': 2, '2': 3 } //초과된 인수는 무시
```



인자 갯수를 확인하고 이에 따라 동작을 달리 정의할 필요가 있을 수 있다. 이때 유용하게 사용되는 것이 arguments 객체이다.

arguments 객체는 매개변수 갯수가 확정되지 않은 **가변 인자 함수**를 구현할 때 유용하게 사용된다.

```js
// 들어오는 숫자 전부를 더 하고 싶을때 매번 바뀌는 매개변수를 쓸수 없기 때문에 매개변수를 지정하지않으면 가변인자함수가된다.
function sum() {
  var res = 0;

  for (var i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum());        // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3)); // 6
```

arguments 객체는 배열의 형태로 인자값 정보를 담고 있지만 실제 배열이 아닌 **유사배열객체(array-like object)**이다.

---



## 6- 2. caller 프로퍼티

caller 프로퍼티는 자신을 호출한 함수를 의미한다.

```js
function foo(func) {
  var res = func();
  return res;
}

function bar() {
  return 'caller : ' + bar.caller;
}

console.log(foo(bar)); // function foo(func) {...}
console.log(bar());    // null (browser에서의 실행 결과) // 엄밀히 말하면 전역에서 호출함
```



---



## 6- 3. length 프로퍼티

매개변수 갯수를 의미한다.

```js
function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```

arguments.length의 값과는 다를 수 있으므로 주의



## 6- 4. name 프로퍼티

함수명을 나타낸다. 기명함수의 경우 함수명을 값으로 갖고 **익명함수의 경우 빈문자열을 값**으로 갖는다.

```js
// 기명 함수표현식
var namedFunc = function multiply(a, b) {
  return a * b;
};
// 익명 함수표현식
var anonymousFunc = function(a, b) {
  return a * b;
};

console.log(namedFunc.name);     // multiply
console.log(anonymousFunc.name); // ''
```



---



## 6- 5. \__proto__ 프로퍼티

ECMAScript spec에서는 **모든 객체는 자신의 프로토타입을 가리키는 [[Prototype]]이라는 숨겨진 프로퍼티를 가진다** 라고 되어있다. 크롬, 파이어폭스 등에서는 숨겨진 [[Prototype]] 프로퍼티가 __proto__ 프로퍼티로 구현되어 있다. 즉 __proto__과 [[Prototype]]은 같은 개념이다. 글자가 다를뿐

함수의 프로토타입 객체는 `Function.prototype`이며 이것 역시 함수이다.

```js
function square(number) {
  return number * number;
}

console.log(square.__proto__ === Function.prototype);
console.log(Object.getPrototypeOf(square) === Function.prototype);
```

## 6- 6. prototype 프로퍼티

함수 객체만이 가지고 있는 프로퍼티로 자바스크립트 객체지향의 핵심이다.



주의해야 할 것은 함수 객체만이 가지고 있는 prototype 프로퍼티는 프로토타입 객체를 가리키는 \__proto__ 프로퍼티와는 다르다는 것이다.

둘은 모두 프로토타입객체를 가리키지만  관점의 차이가있다.

- [[Prototype]] 프로퍼티
  - 모든 객체가 가지고 있는 프로퍼티이다.

  - 객체의 입장에서 자신의 부모 역할을 하는 프로토타입 객체을 가리키며 함수 객체의 경우 Function.prototype를 가리킨다.

    ​

- prototype 프로퍼티
  - 함수 객체만 가지고 있는 프로퍼티이다.
  - **함수 객체가 생성자로 사용될 때 이 함수를 통해 생성된 객체의 부모 역할을 하는 객체를 가리킨다.**
  - 함수가 생성될 때 만들어 지며 `constructor` 프로퍼티를 가지는 객체를 가리킨다. 이 `constructor` 프로퍼티는 함수 객체 자신을 가리킨다.



```js
function square(number) {
  return number * number;
}

// console.dir(square);
console.dir(square.__proto__);
console.dir(square.prototype);

console.log(square.__proto__ === Function.prototype); // true ①
console.log(square.__proto__ === square.prototype);   // false
console.log(square.prototype.constructor === square); // true ②
console.log(square.__proto__.constructor === square.prototype.constructor); // false
```



![function property](http://poiemaweb.com/img/function_prototype.png)

천천히 머릿속으로 그림을 그리며 읽어보자

> \__proto__ 프로퍼티는 함수 객체의 부모 객체(Function.prototype)를 가리키며 prototype 프로퍼티는 함수객체가 생성자 함수로 사용되어 객체를 생성할 때 생성된 객체의 부모 객체 역할을 하는 객체를 가리킨다.  



***



# 7. 함수의 다양한 형태





## 7- 1. 즉시호출함수표현식 (IIFE)

Immediately Invoke Function Expression

함수의 정의와 동시에 실행되는 함수를 즉시호출함수라고 한다. 최초 한번만 호출되며 다시 호출할 수는 없다. 이러한 특징을 이용하여 **최초 한번만 실행이 필요한 초기화 처리등**에 사용할 수 있다.



```js
// 기명 즉시실행함수(named immediately-invoked function expression)
(function myFunction() {
  var a = 3;
  var b = 5;
  return a * b;
}());

// 익명 즉시실행함수(immediately-invoked function expression)
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}());
```



## 7- 2. 내부 함수

함수 내부에 정의된 함수를 내부함수라 한다.

내부함수 child는 자신을 포함하고 있는 부모함수 parent의 변수에 접근할 수 있다. 하지만 부모함수는 자식함수(내부함수)의 변수에 접근할 수 없다.

## 7- 3. 콜백 함수

콜백함수는 함수를 명시적으로 호출하는 방식이 아니라 특정 이벤트가 발생했을 때 시스템에 의해 호출되는 함수를 말한다.