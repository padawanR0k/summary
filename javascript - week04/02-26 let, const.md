ES6 이전의 자바스크립트는 변수를 선언하려면 무조건 `var` 키워드를 사용해야 했다.



non-block level 인 ES5의 문제점 

1. Funcition-level scope

   - 블록레벨 스코프를 지원하지 않는ㄷ.


   - 함수 블록 내의 변수가 아니면 전부 전역변수가 된다.

2. `var` 키워드 생략 허용

   - `var`키워드 생략시 의도치않게 전역변수가 된다.

3. 중복 선언 허용

   - 의도치않게 변수값이 변경된다

4. 변수 호이스팅

   - 변수를 선언하기 전에 참조가 가능하다. (실행컨텍스트단계에서 변수의 선언단계와 초기화단계가 동시에 일어나서)

ES6는 이러한 var의 단점을 보완하기 위해 let과 const 키워드를 도입하였다.



# 1. let

1. block level scope 
2. `var` 키워드 생략 허용하지 않는다.
3. 중복 선언을 허용하지않는다.
4. 변수 호이스팅이 일어나지 않는것 처럼 동작한다.



## 1.1 Block-level scope

scope가 넓을 수록 변수가 저장되는 시간이 길고,  의도치않게 변경될 수 있기 때문에 비효율적이다. 



```js
console.log(foo); // undefined
var foo = 123;
console.log(foo); // 123
{
  var foo = 456; // 중복선언이 허용되었다(재할당) 기존 ES5의 문제점.
}
console.log(foo); // 456
```



ES6의 `let`키워드는 **Block-level scope**를 지원한다.

```js
let foo = 123; 
{
  let foo = 456; // 위의 foo와 다른 지역변수 foo 이다.
  let bar = 456;
}
console.log(foo); // 123 그러므로 456이 아니라 123이다.
console.log(bar); // ReferenceError: bar is not defined
```





## 1.2 중복 선언 금지

```js
var foo = 123;
var foo = 456;  // OK

let bar = 123;
let bar = 456;  // Uncaught SyntaxError: Identifier 'bar' has already been declared
```

`let`으로 선언한 변수는 중복선언시 Syntax error 가 발생한다.



## 1.3 호이스팅(Hoisting)

호이스팅은 모든 선언문(var, let, const, function, function*, class)에서 발생한다.

하지만 `let`키워드로 선언된 변수는 호이스팅이 일어나지 않는것처럼 동작한다. . 이는 let 키워드로 선언된 변수는 스코프의 시작에서 변수의 선언까지 **일시적 사각지대(Temporal Dead Zone; TDZ)**에 빠지기 때문이다.

```js
console.log(foo); // undefined
var foo;

console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
let bar;
```

>  실행컨텍스트 상에는 있지만 메모리상에는 없다.

`let`으로 선언한 변수는 선언단계와 초기화단계가 **분리되어 진행**된다.

![let lifecycle](http://poiemaweb.com/img/let-lifecycle.png)





## 1.4 클로저

기존 ES5에서의 클로저는 이런방식으로 썻다.

```js
var funcs = [];

// 함수의 배열을 생성한다
// i는 전역 변수이다
for (var i = 0; i < 3; i++) {
  (function (index) { // index는 자유변수이다.
    funcs.push(function () { console.log(index); });
  }(i));
}

// 배열에서 함수를 꺼내어 호출한다
for (var j = 0; j < 3; j++) {
  funcs[j](); 
}
```



ES6에서는 `let`키워드로 쉽게해결할 수 있다.

```js
var funcs = [];

// 함수의 배열을 생성한다
// let으로 선언된 i는 for loop에서만 유효한 지역변수이면서 자유변수이다
for (let i = 0; i < 3; i++) { 
  funcs.push(function () { console.log(i); });
}

// 배열에서 함수를 꺼내어 호출한다
for (var j = 0; j < 3; j++) {
  funcs[j]();
}
```





## 1.5 전역 객체와 let

let 키워드로 선언된 변수를 전역 변수로 사용하는 경우, let 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉 **window.foo와 같이 접근할 수 없다**. let 전역 변수는 **보이지 않는 개념적인 블럭 내에 존재**하게 된다.

```js
let foo = 123; // 전역변수

console.log(window.foo); // undefined
```





---



# 2. const

`let`과 다른점은 재할당이 안된다는 부분이다.



## 2.1 선언과 초기화

`const`는 **반드시 선언과 동시에 초기화**가 이루어져야 한다는 것이다. `const` 도 block level scope를 갖는다.

```js
const FOO = 123; // 대문자로 쓴 이유는 상수이기 때문이다.
FOO = 456; // TypeError: Assignment to constant variable.
```



## 2.2 상수

```js
// x의 의미를 알기 어렵기 때문에 가독성이 좋지 않다.
if (x > 10) {
}

// 변수의 의미를 명확히 기술하여 가독성이 향상되었다.
// 조건문을 작성할때 항상 상수로 만들수 있는지 생각해보자.
const MAXROWS = 10;
if (x > MAXROWS) {
}
```

const는 객체에도 사용할 수 있다.

```js
const obj = { foo: 123 };
obj = { bar: 456 }; // TypeError: Assignment to constant variable.
```



## 2.3 const와 객체

const 변수의 값이 객체인 경우, 객체에 대한 **참조의 변경을 금지한다는 것을 의미**한다.

하지만 **객체의 프로퍼티는 보호되지 않는다.** 

변수가 가르키는 주소값은 달라지지않는다는 것이다.



```js
const user = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};


// user = {}; // TypeError: Assignment to constant variable.

// 프로퍼티 값의 재할당은 허용된다!
user.name = 'Kim';

console.log(user); // { name: 'Kim', address: { city: 'Seoul' } }
```

**객체 타입 변수 선언에는 const를 사용하는 것이 좋다.** 

- 일반적으로 객체에 대한 참조는 변경될 필요가 없다. 즉, **const를 사용하여 객체 참조를 변경시킬 수 없어도 객체의 프로퍼티를 변경 가능**하다. 꼭 새로운 객체에 대한 참조를 변수에 재할당해야 한다면 let을 사용한다.
- const를 사용한다 하더라도 객체의 프로퍼티를 변경할 수 있다.



---



# 3. var vs. let vs. const

- ES6를 사용한다면 **var 키워드는 사용하지 않는다.**
- 변경이 발생하지 않는(**재할당이 필요없는**) primitive형 변수와 객체형 변수에는 **const를 사용**한다. (상수를 선언할때)
- **재할당이 필요**한 primitive형 변수에는 **let를 사용**한다.