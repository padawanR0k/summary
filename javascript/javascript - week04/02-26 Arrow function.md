# 1. Arrow function의 선언

Arrow function(화살표 함수)은 **function 키워드 대신 화살표(=>)를 사용하여 간략한 방법**으로 함수를 선언할 수 있다. 하지만 모든 경우 사용할 수 있는 것은 아니다.  

기본적으로 화살표함수는 **콜백함수**일때 쓰자.

```js
// 매개변수 지정 방법
    () => { ... } // 매개변수가 없을 경우
     x => { ... } // 한개인 경우, 소괄호를 생략할 수 "있다".
(x, y) => { ... } // 여러개인 경우, 소괄호를 생략할 수 "없다".

// 함수 몸체 지정 방법
x => { return x * x }  // single line block
// 위의 코드와 같다.
function (x) { return x * x; }
           
x => x * x    // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다. 위 표현과 동일하다. 

() => { return { a: 1 }; }
() => ({ a: 1 })  // 위 표현과 동일하다. 객체 반환시 소괄호를 사용한다. 사용하지않으면 문법에러가 난다.

() => {           // multi line block.
  const x = 10;
  return x * x;
};
```



---



# 2. Arrow function의 호출

이렇게 사용 가능하나. 가독성이 떨어지므로 지양하자.

```js
// ES5
var pow = function (x) { return x * x; };
console.log(pow(10)); // 100

// ES6
const pow = x => x * x;
console.log(pow(10)); // 100
```



```js
// ES5
var arr = [1, 2, 3];
var pow = arr.map(function (x) { // x는 요소값
  return x * x;
});

console.log(pow); // [ 1, 4, 9 ]

// ES6
const arr = [1, 2, 3];
const pow = arr.map(x => x * x);
// 매개변수가 하나이므로 소괄호 생략
// 한줄이여서 중괄호 생략
console.log(pow); // [ 1, 4, 9 ]				
```

---



# 3. this

function 키워드를 사용하여 생성한 일반 함수와 Arrow function와의 가장 큰 차이점은 **this**이다.

## 3.1 일반 함수의 this

```js
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  // (A)
  return arr.map(function (x) {
    return this.prefix + ' ' + x; // (B)
  });
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));
```

(A) 지점에서의 this는 생성자 함수 Prefixer가 생성한 객체, 즉 **생성자 함수의 인스턴스**(위 예제의 경우 pre)이다.

(B) 지점에서 사용한 this는 아마도 생성자 함수 Prefixer가 생성한 객체(위 예제의 경우 pre)일 것으로 기대하였겠지만 **이곳에서 this는 전역 객체 window**를 가리킨다. 이는 생성자 함수와 객체의 메소드를 제외한 모든 함수(내부함수, 콜백함수 포함)의 내부의 this는 전역객체를 가리키기 때문이다.



콜백함수의 내부의 this가 호출한 객체를 가리키게 하는 3가지 방법

콜백함수 내부의 this가 메소드를 호출한 객체(생성자 함수의 인스턴스)를 가리키게 하기 위해서는 아래의 3가지 방법이 있다.

```js
// Solution 1: that = this
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  var that = this;  // this: Prefixer 생성자 함수의 인스턴스
  return arr.map(function (x) {
    return that.prefix + ' ' + x;
  });
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));

```

```js
// Solution 2: map(func, this)
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  return arr.map(function (x) {
    return this.prefix + ' ' + x;
  }, this); // this: Prefixer 생성자 함수의 인스턴스 // map()의 두번째인자로 this == Prefixer를 주었다.
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));

```

ES5에 추가된 Function.prototype.bind()로 this를 바인딩한다. call(), apply()도 사용 가능하다.

```js
// Solution 3: bind(this)
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  return arr.map(function (x) {
    return this.prefix + ' ' + x;;
  }.bind(this)); // this: Prefixer 생성자 함수의 인스턴스
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));
```

## 3.2 Arrow function의 this



Arrow function은 자신만의 **this를 생성하지 않고 자신을 포함하고 있는 컨텍스트로 부터 this를 계승** 받는다. 이를 **Lexical this**라 한다. Arrow function은 Solution 3의 Syntactic sugar이다.

```js
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  return arr.map(x => `${this.prefix}  ${x}`);
};

const pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));
```

---



# 4. Arrow Function을 사용해서는 안되는 경우



## 4.1 메소드

```js
// Bad
const obj = {
  name: 'Lee',
  sayHi: () => console.log(`Hi ${this.name}`) // 자신보다 한단계위의 this
};

```

이와 같은 경우는 메소드를 위한 단축 표기법인 ES6의 축약 메소드 표현을 사용하는 것이 좋다.

```js
// Good
const obj = {
  name: 'Lee',
  sayHi() { // === sayHi: function() { // 메서드는 이렇게 콜백함수는 Arrow function
    console.log(`Hi ${this.name}`);
  }
};

obj.sayHi(); // Hi Lee
```



## 4.2 prototype

Arrow Function으로 객체 메소드를 정의하였을 때와 같은 문제가 발생한다. 따라서 prototype에 메소드를 할당하는 경우, 일반 함수를 할당한다.

```js
const obj = {
  name: 'Lee',
};

// BAD
Object.prototype.sayHi = () => console.log(`Hi ${this.name}`); // this === window

// GOOD
Object.prototype.sayHi = function() {
  console.log(`Hi ${this.name}`); // this === Object
};


obj.sayHi(); // Hi undefined
```





## 4.3 생성자 함수

Arrow Function은 **prototype 프로퍼티를 가지고있지 않기 때문에**  생성자 함수로 사용할 수 없다.

```js
const Foo = () => {};
// Arrow Function은 prototype 프로퍼티가 없다
console.log(Foo.hasOwnProperty('prototype')); // false
const foo = new Foo(); // TypeError: Foo is not a constructor
```



## 4.4 addEventListener 함수의 콜백 함수

```js
// Bad
var button = document.getElementById('myButton');

button.addEventListener('click', () => {
  console.log(this === window); // => true 
  // arrow function을 쓰면 this는 한단계 위 코드블럭의 this이고
  // arrow function을 쓰지않으면 this는 currentTarget이다. 
  // currentTarget은 이벤트가 바인딩된 요소를 뜻한다. 여기서는 button
  this.innerHTML = 'Clicked button';
});

// Good
var button = document.getElementById('myButton');

button.addEventListener('click', function() {
  console.log(this === button); // => true
  this.innerHTML = 'Clicked button';
});
```

