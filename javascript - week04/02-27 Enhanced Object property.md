# 1. 프로퍼티 축약 표현

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우, **프로퍼티 이름을 생략**할 수 있다. 이때 프로퍼티 이름은 변수의 이름으로 자동 생성된다.

```js
// ES5
var x = 1, y = 2;

var obj = {
  x: x,
  y: y
};


// ES6
let x = 1, y = 2;

const obj = { x, y };

console.log(obj); // { x: 1, y: 2 }
```



# 2. 프로퍼티 이름 조합

ES6에서는 객체 리터럴 내에서 **프로퍼티 이름을 동적으로 생성**할 수 있다.

```js
// ES5
var i = 0;
var propNamePrefix = 'prop_';

var obj = {};

obj[propNamePrefix + ++i] = i;
obj[propNamePrefix + ++i] = i;
obj[propNamePrefix + ++i] = i;



// ES6
let i = 0;
const propNamePrefix = 'prop_';

const obj = {
  [propNamePrefix + ++i]: i,
  [propNamePrefix + ++i]: i,
  [propNamePrefix + ++i]: i
};
console.log(obj); // { prop_1: 1, prop_2: 2, prop_3: 3 }
```





# 3. 메소드 축약 표현

메소드를 선언하기 위해서 function 키워드를 생략한 축약 표현을 사용할 수 있다.

```js
// ES5
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};


// ES6
const obj = {
  name: 'Lee',
  // 메소드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```



# 4. \__proto__ 프로퍼티에 의한 상속

객체 리터럴 내부에서 \__proto__ 프로퍼티를 직접 설정할 수 있다. 이것은 객체 리터럴에 의해 생성된 객체의 \_\_proto__ 프로퍼티에 **다른 객체를 직접 바인딩하여 상속을 표현**할 수 있음을 의미한다.

```js
// ES5
var parent = {
  name: 'parent',
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

// 프로토타입 패턴 상속
var child = Object.create(parent); // parent를 prototype으로 지정하고 객체를 생성한다.
child.name = 'child';


// ES6
const parent = {
  name: 'parent',
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

const child = {
  // child 객체의 프로토타입 객체에 parent 객체를 가져옴.
  __proto__: parent,
  name: 'child'
};

parent.sayHi(); // Hi! parent
child.sayHi();  // Hi! child
```







ES6은 클래스 기반언어가 지원하는 기술들을 대부분 지원하지않고 extend라는 상속만 지원한다.