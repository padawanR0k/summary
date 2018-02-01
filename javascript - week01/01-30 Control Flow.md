# 제어문

제어문(Control flow statement)은 조건에 따른 명령 실행(조건문)이나 반복 실행(반복문)이 필요할 때 사용된다.

***



## 1. 블록구문

구문들의 집합중의 가장 기본이 되는 구문. 

일반적으로 함수, 객체리터럴, 흐름제어구문에서 사용된다.

```javascript
// 함수 선언문 : "재사용"이 가능하다.
function foo() {
  var x = 1, y = 2; 
  console.log(x + y);  
}// 함수내 블록구문이라서 x , y 지역변수 인정됨
	foo();

// 객체리터럴에 의한 객체 선언
var obj = {
  x: 1,
  y: 2
}; 
console.log(obj.x + obj.y);

// 흐름 제어 구문(control flow statement)
var x = 0;
while (x < 10) {
  x++;
}
console.log(x);
```





---



## 2. 조건문

> 데이터의 흐름을 제어한다는 것은 일정 조건에 따른 의사결정(decision)을 통해 다음 진행 흐름으로 유도(Control flow)하는 것이다. 이는 가장 원시적인 형태의 인공 지능(Artificial Intelligence)을 부여하는 것이라고 볼 수 있다. 즉 의사결정(상황판단)의 기준을 제시하고 그 결과에 따른 행위를 지시하는 것이다.

조건문은 주어진 조건식이 참(`true`)인지 거짓(`false`)인지에 따라 실행되어질 구문들의 집합이다.



---



### 2- 1. if문

```javascript
if (조건식){
  // 조건식  true일때
}else{
  // 조건식 false일때
}
```





---



### 2- 2. switch 문

switch 문의 경우, `switch`변수의 값과 일치되는 `case`문으로 실행 순서가 이동하게 된다. `switch`변수의 값과 일치되는 `case`문이 없다면 실행 순서는 `default`로 이동한다.

```javasc
var animal = 'tiger';

switch (animal){
  case 'tiger':
  	console.log('TIGER');
  	break;
  case 'lion':
  	console.log('LION');
  	break;
  case 'dog':
  	console.log('DOG');
  	break;
  case 'cat':
  	console.log('CAT');
  	break;
  defualt:
  	console.log('unknown animal');
}
```

마치 `else if`와 `else` 구문을 많이 쓴것과 유사하다.



---



## 3. 반복문 (Loop)

JavaScript는 3가지의 반복문 `for`, `while`, `do while`을 제공한다.



### 3- 1. for 문

for문의 실행순서는 아래와 같다.

```javascript
for (var i = 0; i < 2; i++) {
  console.log(i);
}
```

![for문 실행 순서](http://poiemaweb.com/img/for-statement.png)

|      | i    | i < 2 | i++       | console.log(i) |
| ---- | ---- | ----- | --------- | -------------- |
| 1    | 0    |       |           |                |
| 2    |      | true  |           |                |
| 3    |      |       |           | 0              |
| 4    |      |       | i = 0 + 1 |                |
| 5    |      | true  |           |                |
| 6    | 1    |       |           | 1              |
| 7    |      |       | i = 1+ 1  |                |
| 8    |      | false |           |                |

#### 배열을 순회하는 for문


// for-in, foreach, for-of (ES6), array.entries()

---



### 3- 2. while 문

while 문은 조건문이 참이면 코드 블럭을 계속해서 반복해서 실행하고 거짓이되면 반복문을 빠져나간다.

```javascript
var n = 0;
var x = 0;

// n이 3보다 작을 때까지 계속 반복한다.
while (n < 3) { // n: 0 1 2
  n++;          // n: 1 2 3
  x += n;       // x: 1 3 6
  console.log(x);
}

var i = 0;
// true는 항상 true이기 때문에 무한루프에 빠지게된다.
while (true) {
  console.log(i);
  i++;
}
```



---



### 3- 3. do while문

while 문과 비슷하나 먼저 1회 실행되고 조건문을 확인한다.



---



### 3- 4. continue

```javascript
for (var i = 0 ; i <10 ; i ++){
  if ( i < 5) continue;
  console.log(i);
}
// 1,2,3,4 는 출력이 안되고 5,6,7,8,9까지 출력된다.
```



***



## 4. 평가

흐름제어를 위하여 조건식을 평가하여 논리적 참, 거짓을 평가 결과에 따라 의사결정을 하는것이 일반적이다.

조건식은 표현식의 일종이다. 따라서 피연산자와 연산자로 구성된 일반적 표현식뿐만 아니라 문자열이나 숫자와 같은 리터럴 값, 변수, 내장값들(null, undefined, Nan, Infinity…)등 또한 조건식으로 사용될 수 있다.

이때 자바스크립트는 **암묵적 강제 형 변환**을 통해 조건식을 평가한다.

```js
if(i % 2){ // i % 2는 0 아니면 1 값이온다. 이때 0을 false로 1을 true로 강제 형 변환한다.
  console.log("odd num")
}

if (x){ // x가 선언되지 않앗으므로 undefined == false 
  console.log("false")
}
```



---



### 4- 1. 암묵적 강제 형 변환

아래의 값이 어떻게 계산될지 생각해보고 직접 크롬 콘솔창에 쳐보자

```js
console.log('1' > 0);            // 
console.log(1 + '2');            // 
console.log(2 - '1');            // 
console.log('10' == 10);         // 
console.log('10' === 10);        // 
console.log(undefined == null);  // 
console.log(undefined === null); // 
```



위 처럼 코딩하는건 가독성이 안좋고 버그를 만들가능성이 높으므로 지양하자.



---



### 4- 2. 형 변환 테이블

|  Original Value   | Converted to Number | Converted to String | Converted to Boolean |
| :---------------: | :-----------------: | :-----------------: | :------------------: |
|       false       |        **0**        |       ‘false’       |        false         |
|       true        |        **1**        |       ‘true’        |         true         |
|         0         |          0          |         ‘0’         |      **false**       |
|         1         |          1          |         ‘1’         |         true         |
|        ‘0’        |        **0**        |         ‘0’         |       **true**       |
|        ‘1’        |        **1**        |         ‘1’         |         true         |
|        NaN        |         NaN         |        ‘NaN’        |      **false**       |
|     Infinity      |      Infinity       |     ‘Infinity’      |         true         |
|     -Infinity     |      -Infinity      |     ‘-Infinity’     |         true         |
|        ’’         |        **0**        |         ’’          |      **false**       |
|       ‘10’        |         10          |        ‘10’         |         true         |
|       ‘ten’       |         NaN         |        ‘ten’        |         true         |
|        [ ]        |        **0**        |         ’’          |         true         |
|       [10]        |       **10**        |        ‘10’         |         true         |
|     [10, 20]      |         NaN         |       ‘10,20’       |         true         |
|      [‘ten’]      |         NaN         |        ‘ten’        |         true         |
| [‘ten’, ‘twenty’] |         NaN         |    ‘ten, twenty’    |         true         |
|   function(){}    |         NaN         |   ‘function(){}’    |         true         |
|        { }        |         NaN         |  ‘[object Object]’  |         true         |
|       null        |        **0**        |       ‘null’        |      **false**       |
|     undefined     |       **NaN**       |     ‘undefined’     |      **false**       |

```js
var x = false;

// Number() , String() , Boolean() 과 같은 생성자함수앞에 new를 생략하면 인자가 형변환이된다
// 변수 x의 값을 숫자 타입으로 변환
console.log('Number : ' + Number(x));  // 0
// 변수 x의 값을 문자열 타입으로 변환
console.log('String : ' + String(x));  // 'false'
// 변수 x의 값을 불리언 타입으로 변환
console.log('Boolean: ' + Boolean(x)); // false



// + 단항연산자는 undefined,Nan을 제외한 값을 숫자형으로 변환한다.
console.log(+10);     // 10
console.log(+'10');   // 10
console.log(+true);   // 1
console.log(+null);   // 0
console.log(+undefined); // NaN
console.log(+NaN);    // NaN
```



---



### 4- 3. Data Type conversion

```js
var val = '123';
console.log(typeof val + ': ' + val); // string: 123

// sting -> number
val = +val; 
// val = parseInt(val);
// val = Number(val);
console.log(typeof val + ': ' + val); // number: 123

// number -> sting
val = val + '';
// val = val.toString();
console.log(typeof val + ': ' + val); // string: 123
```



---



### 4- 4. Checking existence



객체가 undefiend,null이 아니면 true || false 로 취급한다.

```
// DOM에서 특정 요소를 취득
var elem = document.getElementById('header');

if (elem) {
  // 요소가 존재함 : 필요한 작업을 수행
} else {
  // 요소가 존재하지 않음 : 에러 처리
  console.log("there is no element")
}

// 이 경우는 elem의 값이 true인지 평가하는것이므로 false가 나온다.
if (elem == true) // false 
```

