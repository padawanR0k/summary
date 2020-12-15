> 스코프는 범위라고 생각하면 편하다.



- 전역 변수 (Global variable)
  - 전역 Scope를 갖는 변수.
  - 모든 곳에서 사용 가능한 변수
- 지역 변수 (Local variable)
  - 지역 Scope를 갖는 변수

함수의 블록레벨에서는 변수를 선언하면 전역변수가 되지않고 지역변수가된다. 

*이 말은 곧 함수의 블록레벨이 아니라면 전역변수가 된다는 말이다.*

```js
var x = 0;  // 전역변수
{
  var x = 1; // 전역변수
  console.log(x); // 1
}
console.log(x);   // 1

// es6에서는 블록레벨에서도 지원한다.
let y = 0;
{
  let y = 1;
  console.log(y); // 1
}
console.log(y);   // 0


```

***



# 1. Global scope 

글로벌 영역에 변수를 선언하면 모든곳에서 참조가능하다.

```js
var global = 'global';

function foo() {
  var local = 'local';
  console.log(global);
  console.log(local);
}
foo();

console.log(global); // 한번 생각해보자
console.log(local); // 한번 생각해보자
```

전역변수를 쓰는 건 사람이 보기엔 편하다. 하지만 *전역변수를 남발할 경우 전역변수가 계속 메모리에 남아있기 때문에* 메모리효율이 나빠진다. 함수블록안에 선언할경우 함수가 실행될 때만 생겼다가 사라지기때문에 메모리효율이 좋아진다. 



# 2. Non block - level scope 

```js
if (true) {
   var x = 5;
}
console.log(x); // 5 전역변수이다.
```



# 3. Function scope 

```js
(function () {
  var b = 20;   // 지역변수
})();

console.log(b); // "b" is not defined 왜냐면 함수블록안에있기때문에

// ================
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x);
}

foo();          // local 함수블록내의 x
console.log(x); // global 함수블록 밖의 x

// ================
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x); // local

  function bar() {  // 내부함수
    console.log(x); //  local 블록안의 블록
  }

  bar();
}
foo(); 
console.log(x); //  global

// ==============
var x = 10;

function foo() {
  x = 100;  // 전역의 x를 참조해서 값을 변경했다. foo()안에 다시 생성한게아니라
  console.log(x);
}
foo(); // ?
console.log(x); // ?  답은뭐일지 생각해보고 콘솔창에 찍어보자
```



문제

```js
var foo = function ( ) {

  var a = 3, b = 5;

  var bar = function ( ) {
    var b = 7, c = 11;
    
	console.log(a);//?
    console.log(b);//?
    console.log(c);//?
    a += b + c;
    
    console.log(a);//?
  };
  
    console.log(a);//?
    console.log(b);//?
    console.log(c);//?	
  bar( );
};
```



---



# 4. 암묵적 전역(implied globals) 

```js
function foo() {
  x = 1;   // var키워드가 생략되서 전역변수가됬음
  var y = 2;
}

foo();

console.log(x); // 1 그래서 출력함
console.log(y); // ReferenceError: y is not defined
```



---



# 5. Lexical scoping(Static scoping) 

```js
var i = 5;

function foo() {
  var i = 10;
  bar(); // 이때 i가 10이 아닌 5를참조 선언된시점이 언제인지 생각해보자
}

function bar() { // 선언된 시점에서의 scope를 갖는다! i 는 5
  console.log(i); 
}

foo(); // ?


```





# 6. 변수명의 중복 

```js
// x.js
function foo (){
  // var i = 0;
  i = 0;
  // ...
}

// y.js
for(var i = 0; i < 5; i++){
  foo(); // foo() 가 i를 계속 0으로 만들기 때문에 무한히 반복된다.
  console.log(i);
}

```



---





# 8. 즉시실행함수를 이용한 전역변수 사용 억제 

```js
(function () {
  var MYAPP = {};

  MYAPP.student = {
    name: 'Lee',
    gender: 'male'
  };

  console.log(MYAPP.student.name); // lee
}());

console.log(MYAPP.student.name); // error
```

전역변수 사용을 최대한줄이기, 즉시 실행 함수를 사용할 수 있다. 이 방법을 사용하면 전역변수를 만들지 않으므로 라이브러리 등에 자주 사용된다. **즉시 실행 함수는 즉시 실행되고 그 후 전역에서 바로 사라진다.**