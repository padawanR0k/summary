# 1. 기본 파라미터 초기값 (Default Parameter value)

ES5에서는 파라미터에 초기값을 설정할 수 없다. 그래서 함수 내부에 받는 인수가 자신이 원하는 타입의 데이터인지 확인해야하는 번거로움이 있엇다.

```js
// ES5
function plus(x, y) {
  x = x || 0; // 이런 방어코드
  y = y || 0;
  return x + y;
}

console.log(plus(1, 2)); // 3
```

ES6에서는 파라미터에 초기값을 설정하여 자료형을 초기화 할 수 있다.

```js
// ES6
function plus(x = 0, y = 0) {
  // 인수를 전달하지않으면 = 0 이 실행된다.
  return x + y;
}

console.log(plus());     // 0
console.log(plus(1, 2)); // 3
```



---

# 2. Rest 파라미터 (Rest Parameter)



## 2.1 Syntax

Spread 연산자(`...`)를 사용하여 파라미터를 작성한 형태를 말한다.

```js
function foo(...rest) { // 매개변수자리에 ... 연산자를 쓰면 인수 합치고 여기서 rest는 [1, 2, 3, 4, 5]
  console.log(Array.isArray(rest)); // true
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);

function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest);  // [ 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest);   // [ 3, 4, 5 ]
}

bar(1, 2, 3, 4, 5);
```



## 2.2 arguments와 rest 파라미터

```js
// ES5
var foo = function () {
  console.log(arguments);
};

foo(1, 2); // { '0': 1, '1': 2 }  타언어는 매개변수의 수와 던져주는 인수의 수의 갯수가 맞지않으면 에러지만 자바스크립트는 그렇지않다.
```

ES5에서는 인자의 갯수를 사전에 알 수 없는 가변 인자 함수의 경우, **arguments 객체를 통해 인자값을 확인**한다. arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는  유사 배열 객체이다



```js
var es5 = function () {};
console.log(es5.hasOwnProperty('arguments')); // true

const es6 = () => {};
console.log(es6.hasOwnProperty('arguments')); // false
```

ES6의 화살표함수는 arguments 객체가 없다!





---

# 3. Spread 연산자 (Spread Operator)

Spread 연산자(`...`)는 연산자의 대상 배열 또는 **이터러블**을 개별 요소로 분리한다.

>  **이터러블**: 객체를 순회가능한(순서가 보장되는) 객체로 만들수 있는 방법을 제공한 것
>
> ex) 유사배열 객체,  String 객체, getElements....로 받아온 컬렉션, querySelector로 받아온 nodeList

이때 for-of 반복문으로 순회할 수 있다.

```js
// ...[1, 2, 3]는 [1, 2, 3]을 개별 요소로 분리한다(-> 1, 2, 3)
console.log(...[1, 2, 3]) // -> 1, 2, 3

// 문자열은 이터러블이다.
console.log(...'Helllo');  // H e l l l o  문자는 유사배열객체라 ...연산자로 배열이되었다.

// Map과 Set은 이터러블이다.
console.log(...new Map([['a', '1'], ['b', '2']]));  // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3]));  // 1 2 3
```



## 3.1 함수의 인자로 사용하는 경우

```js
// ES5에서는 배열을 함수의 인자로 사용하고, 배열의 각 요소를 개별적인 파라미터로 전달하고 싶은 경우, Function.prototype.apply를 사용하는 것이 일반적이다.
function foo(x, y, z) {
  console.log(x); // 1
  console.log(y); // 2
  console.log(z); // 3
}

// 배열을 foo 함수의 인자로 전달하려고 한다.
const arr = [1, 2, 3];

// apply 함수의 2번째 인자(배열)는 호출하려는 함수(foo)의 개별 인자로 전달된다.
foo.apply(null, arr); // apply(this로 쓰려고하는것, 함수에 주려는 매개변수)
foo.call(null, 1, 2, 3); // .call() .apply()의 차이는 객체로 주느냐 리스트로 주느냐의 차이다.
```



ES6의 Spread 연산자(…)를 사용한 배열을 함수의 인수로 사용하면 배열의 요소를 개별적으로 분리하여 순차적으로 파라미터에 할당한다.

```js
// ES6
function foo(x, y, z) {
  console.log(x); // 1
  console.log(y); // 2
  console.log(z); // 3
}

// 배열을 foo 함수의 인자로 전달하려고 한다.
const arr = [1, 2, 3];

// ...[1, 2, 3]는 [1, 2, 3]을 개별 요소로 분리한다(-> 1, 2, 3)
// spread 연산자에 의해 분리된 배열의 요소는 개별적인 인자로서 각각의 매개변수에 전달된다.
foo(...arr);	
```



Rest 파라미터는 반드시 마지막 파라미터이어야 하지만 Spread 연산자를 사용한 인수는 자유롭게 사용할 수 있다.



```js
// ES6
function foo(v, w, x, y, z) {
  console.log(v); // 1
  console.log(w); // 2
  console.log(x); // 3
  console.log(y); // 4
  console.log(z); // 5
}

// ...[2, 3]는 [2, 3]을 개별 요소로 분리한다(-> 2, 3)
// spread 연산자에 의해 분리된 배열의 요소는 개별적인 인자로서 각각의 매개변수에 전달된다.
foo(1, ...[2, 3], 4, ...[5]);
```


> **rest파라미터는 풀어진 것을 모으고 spread연산자는 모아져있는 것을 풀어준다.**



## 3.2 배열에서 사용하는 경우

## 3.2.1 concat

```js
// ES5
var arr = [1, 2, 3];
console.log(arr.concat([4, 5, 6])); // [ 1, 2, 3, 4, 5, 6 ]

```

Spread 연산자를 배열에서 사용하는 경우, 배열 리터럴 구문만으로 기존 배열의 요소를 새로운 배열 요소의 일부로 만들 수 있다.

```js
// ES6
const arr = [1, 2, 3];
// ...arr은 [1, 2, 3]을 개별 요소로 분리한다
console.log([...arr, 4, 5, 6]); // [ 1, 2, 3, 4, 5, 6 ] ...arr (스프레드 연산자로 arr배열을 풀었다)
```



## 3.2.2 push

```js
// ES5
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];

// apply 메소드의 2번째 인자는 배열. 이것은 개별 인자로 push 메소드에 전달된다.
Array.prototype.push.apply(arr1, arr2);

console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]


// ES6
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// ...arr2는 [4, 5, 6]을 개별 요소로 분리한다
arr1.push(...arr2); // == arr1.push(4, 5, 6);

console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]
```





## 3.2.3 splice

```js
// ES5
var arr1 = [1, 2, 3, 6];
var arr2 = [4, 5];

// apply 메소드의 2번째 인자는 배열. 이것은 개별 인자로 push 메소드에 전달된다.
// [3, 0].concat(arr2) => [3, 0, 4, 5]
// arr1.splice(3, 0, 4, 5) => arr1[3]부터 0개의 요소를 제거하고 그자리(arr1[3])에 새로운 요소(4, 5)를 추가한다.
Array.prototype.splice.apply(arr1, [3, 0].concat(arr2));

console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]

// ES6
const arr1 = [1, 2, 3, 6];
const arr2 = [4, 5];

// ...arr2는 [4, 5]을 개별 요소로 분리한다
arr1.splice(3, 0, ...arr2); // == arr1.splice(3, 0, 4, 5);

console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]
```





## 3.2.4 copy

```js
// ES5
var arr  = [1, 2, 3];
var copy = arr.slice();  // 인자를 주지않으면 배열을 복사한다.

console.log(copy); // [ 1, 2, 3 ]

// copy를 변경한다.
copy.push(4);

console.log(copy); // [ 1, 2, 3, 4 ]
// arr은 변경되지 않는다.
console.log(arr);  // [ 1, 2, 3 ]


// ES6
const arr = [1, 2, 3];
// ...arr은 [1, 2, 3]을 개별 요소로 분리한다
const copy = [...arr];

console.log(copy); // [ 1, 2, 3 ]

// copy를 변경한다.
copy.push(4);

console.log(copy); // [ 1, 2, 3, 4 ]
// arr은 변경되지 않는다.
console.log(arr);  // [ 1, 2, 3 ]
```





## 3.3 객체에서 사용하는 경우

```js
const o1 = { x: 1, y: 2 };
const o2 = { ...o1, z: 3 };

console.log(o2); // { x: 1, y: 2, z: 3 }

const target = { x: 1, y: 2 };
const source = { z: 3 };

// Object.assign를 사용하여도 동일한 작업을 할 수 있다. Object.assign은 타깃 객체를 반환한다
console.log(Object.assign(target, source)); // { x: 1, y: 2, z: 3 }
```

