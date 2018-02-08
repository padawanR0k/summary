# 1. 전역객체 (Global Object)

- 모든객체의 실체의 아버지역할

- 전역 객체는 실행 컨텍스트에 컨트롤이 들어가기 이전에 생성이 되며 constructor가 없기 때문에 new 연산자를 이용하여 새롭게 생성할 수 없다. 즉, 개발자가 전역 객체를 생성하는 것은 불가능하다.

- 전역 객체는 전역 스코프(Global Scope)를 갖게 된다.

- 전역 객체의 자식 객체를 사용할 때 전역 객체의 기술은 생략할 수 있다. 

  ```js
  document.getElementById('foo').style.display = 'none';
  // window.document.getElementById('foo').style.display = 'none';
  ```





- 그러난 사용자가 정의한 변수와 전역객체의 자식과 이름이 충돌하는 경우, 명확히 전역 객체를 기술하여 오류를 방지할 수 있다.

  ```js
  function moveTo(url) {
  var location = {'href':'move to '};
  alert(location.href + url); // 'move to http://www.google.com' 라는 내용을 가진 얼럿창이 뜬다.
    
  // location.href = url;
  window.location.href = url; // 윈도우의 자식인 location http://www.google.com로 이동하게 된다.
  }
  moveTo('http://www.google.com');
  ```





- 글로벌 영역에 선언한 함수도 전역 객체의 프로퍼티로 접근할 수 있다. 윈도우의 메서드가 되었기 때문이다.

  ```js
  function foo() {
    console.log('invoked!');
  }
  window.foo();
  ```




## 1- 1. 전역 프로퍼티

애플리케이션 전역에서 사용하는 값들을 나타내기 위해 사용한다. 전역 프로퍼티는 간단한 값이 대부분이며 다른 프로퍼티나 메소드를 가지고 있지 않다.

- Infinity

  -  양/음의 무한대를 나타내는 숫자값 Infinity를  갖는다

  - ```js
    console.log(window.Infinity); // Infinity
    console.log(3/0);  // Infinity
    console.log(-3/0); // -Infinity
    console.log(Number.MAX_VALUE * 2); // 1.7976931348623157e+308 * 2
    console.log(typeof Infinity); // number
    ```

  ​

- NaN

  - NaN 프로퍼티는 숫자가 아님(Not-a-Number)을 나타내는 숫자값 NaN을 갖는다.

  - ```js
    console.log(window.NaN); // NaN

    console.log(Number('xyz')); // NaN
    console.log(1 * 'string');  // NaN
    console.log(typeof NaN);    // number
    ```

    ​

- undefined

  - undefined 프로퍼티는 기본자료형 undefined를 값으로 갖는다.

  - ```js
    console.log(window.undefined); // undefined

    var foo;
    console.log(foo); // undefined
    console.log(typeof undefined); // undefined
    ```

    ​

## 1- 2. 전역함수

전역 함수는 애플리케이션 전역에서 호출할 수 있는 함수로서 전역 객체의 메소드이다.



- eval()

  - 매개변수에 전달된 문자열 구문 또는 표현식을 평가 또는 실행한다.
  - 보안에 매우 취약하므로 사용은 자제하자.

- isFinite()

  - 매개변수에 전달된 값이 정상적인 유한수인지 검사하여 그 결과를 Boolean으로 반환한다. 매개변수에 전달된 값이 숫자가 아닌 경우, 숫자로 변환한 후 검사를 수행한다.

  - ```js
    console.log(isFinite(Infinity));  // false
    console.log(isFinite(NaN));       // false NaN(Not a Number)은 숫자가아닌데 무한하다고 나온다. 
    console.log(isFinite('Hello'));   // false
    console.log(isFinite('2005/12/12'));   // false

    console.log(isFinite(0));         // true
    console.log(isFinite(2e64));      // true
    console.log(isFinite(null));      // true: null->0  // NaN과 null 같은 원치않는 값이 나왔을때 걸러내기위해 함수를 호출하기전에 방어코드를 짜줘야한다.

    ```



- isNan()

  - 매개변수에 전달된 값이 NaN인지 검사하여 그 결과를 Boolean으로 리턴한다. 매개변수에 전달된 값이 숫자가 아닌 경우, 숫자로 변환한 후 검사를 수행한다.

  - ```js
    isNaN(NaN)       // true
    isNaN(undefined) // true: undefined -> NaN
    isNaN({})        // true: {} -> NaN
    isNaN('blabla')  // true: 'blabla' -> NaN

    isNaN(true)      // false: true -> 1
    isNaN(null)      // false: null -> 0
    isNaN(37)        // false

    // strings
    isNaN('37')      // false: '37' -> 37
    isNaN('37.37')   // false: '37.37' -> 37.37
    isNaN('')        // false: '' -> 0  
    isNaN(' ')       // false: ' ' -> 0

    // dates
    isNaN(new Date())             // false: new Date() -> Number
    isNaN(new Date().toString())  // true:  String -> NaN
    ```



- parseFloat()

  - 매개변수에 전달된 문자열을 부동소수점 숫자로 변환하여 반환한다. 

  - 컴퓨터에서 무한한 값을 표현할때 어느정도에서 끊어버릴때 사용한다.

  - ```js
    parseFloat('3.14');     // 3.14
    parseFloat('10.00');    // 10
    parseFloat('34 45 66'); // 34
    parseFloat(' 60 ');     // 60
    parseFloat('40 years'); // 40
    parseFloat('He was 40') // NaN

    console.log(0.3 + 0. 1)  // 0.3000000004이 나온다. 이럴때 정확한 값을 원하면 parseFloat를 쓰면된다. 
    ```



- parseInt(`string, radix`)

  - 매개변수에 전달된 문자열을 정수형 숫자(Integer)로 변환하여 반환한다. 문자열의 첫 숫자만 반환되며 전후 공백은 무시된다. 그리고 첫문자를 숫자로 변환할 수 없다면 NaN을 반환한다.

  - `string`: 변환 대상 문자열

  - `radix`: 진법을 나타내는 기수(2 ~ 36, 기본값 10)

  - ```js
    parseInt('10');       // 10
    parseInt('10.33');    // 10
    parseInt('34 45 66'); // 34
    parseInt(' 60 ');     // 60
    parseInt('40 years'); // 40
    parseInt('He was 40') // NaN

    parseInt('0x20');     // 16진수 0X20 -> 10진수 32
    parseInt('10', 16);   // 16진수 10 -> 10진수 16
    parseInt('10', 8);    // 8진수 10 -> 10진수 8
    ```

- encodeURI() / decodeURI()

  ![uri](http://poiemaweb.com/img/uri.png)


  - encodeURI()은 매개변수로 전달된 URI(Uniform Resource Identifier)를 인코딩한다.
  - http:// 어떤방식으로 통신할것인가
  - host : 도메인주소
  - port : 기본포트는 생략가능하다 (대부분의 기본포트는 8080)
  - Path : 경로
  - Query Parameter : 전달할 key 와 value 
  - Fragment : 아이디값으로 이동한다. (reload되지않는다)
  - ​

- edcodeURIComponent() / decodeURICOmponent()

  - 내가 원하는 부분만 encode 혹은 decode 한다

---



# 2. 표준빌트인 객체

자바스크립트는 애플리케이션 전역에서 공통적으로 필요한 기능을 사용자 각자가 일일히 작성하는 수고를 줄이기 위해 표준 빌트인 객체(Standard Built-in Objects)를 제공한다.

스태틱메서드와 프로토타입메서드



## 2- 1. Object

객체를 생성한다. 만약 생성자 인수값이 null이거나 undefined이면 빈 객체를 반환한다.

```js
o = new Object(undefined);
console.log(typeof o + ': ', o);

o = new Object(null);
console.log(typeof o + ': ', o);
```

그 이외의 경우 생성자 함수의 인수값에 따라 강제 형변환된 객체가 반환된다. 이때 반환된 객체의 [[prototype]] 프로퍼티에 바인딩된 객체 Object.prototype이 아니다.



```js
// String 객체를 반환한다
// var obj = new String('String');과 동치이다
var obj = new Object('String');
console.log(typeof obj + ': ', obj);
console.dir(obj);

var strObj = new String('String');
console.log(typeof strObj + ': ', strObj);

// Number 객체를 반환한다
// var obj = new Number(123);과 동치이다
var obj = new Object(123);
console.log(typeof obj + ': ', obj);

var numObj = new Number(123);
console.log(typeof numObj + ': ', numObj);

// Boolean 객체를 반환한다.
// var obj = new Boolean(true);과 동치이다
var obj = new Object(true);
console.log(typeof obj + ': ', obj);

var boolObj = new Boolean(123);
console.log(typeof boolObj + ': ', boolObj);
```

---



## 2- 2. Function

자바스크립트의 모든 함수는 Function 객체이다. 다른 모든 객체들처럼 Function 객체는 new 연산자을 사용해 생성할 수 있다.

```js
var adder = new Function('a', 'b', 'return a + b');

adder(2, 6);  // 8
```



---



## 2- 3. Boolean

Boolean 객체는 기본자료형 boolean을 위한 레퍼(wrapper) 객체이다. Boolean 생성자 함수로 Boolean 객체를 생성할 수 있다.

?? 래퍼?

```js
var foo = new Boolean(true);    // true
var foo = new Boolean('false'); // true

var foo = new Boolean(false); // false
var foo = new Boolean();      // false
var foo = new Boolean('');    // false
var foo = new Boolean(0);     // false
var foo = new Boolean(null);  // false
```



## 2- 4. String

기본자료형이 wrapper 객체의 메소드를 사용할 수 있는 이유는 기본자료형과 연관된 wrapper 객체로 일시적으로 변환되어 프로토타입객체를 공유하기떄문이다.

배열의 속성을 가진 객체들은 순회가능하다.

String은 유사배열객체다. 그래서 반복문을 사용해서 순회가능하다.

문자열에 문자는 얼마나많은 문자가 올지 모르기때문에 다른언어에서 문자열은 기본자료형이아니다.(Heap서 관리한다.) 하지만 자바스크립트에서는 기본자료형으로 취급한다.

### 2- 4. 1 String Property

- String.length

  - 문자열 내의 문자 갯수를 반환한다.

  - ```js
    var strObj = new String('Lee');
    console.log(strObj); // String {0: 'L', 1: 'e', 2: 'e', length: 3, [[PrimitiveValue]]: 'Lee'}

    var strObj = new String(1);
    console.log(strObj); // String {0: '1', length: 1, [[PrimitiveValue]]: '1'}

    var strObj = new String(undefined);
    console.log(strObj); // String {0: 'u', 1: 'n', 2: 'd', 3: 'e', 4: 'f', 5: 'i', 6: 'n', 7: 'e', 8: 'd', length: 9, [[PrimitiveValue]]: 'undefined'}
    ```

- 매개변수의 리스트가 arguments의 리스트보다 적으면 arguments 객체에  유사배열형태로 저장되어 있다.

### 2- 4. 2 String Method

- String.prototype.charAt()

  - 매개변수로 전달한 index 번호에 해당하는 위치의 문자를 반환한다

  - ![index](http://poiemaweb.com/img/index.png)

  - ```js
    var str = 'Hello';
    console.log()
    ```



- String.prototype.indexOf()
  - 매개변수로 전달한 문자,문자열을 대상 문자열에서 검색해서 처음 발견된위치의 index를 반환한다.



- String.prototype.lastIndexOf()

  - 매개변수로 전달된 문자 또는 문자열을 대상 문자열에서 검색하여 마지막으로 발견된 곳의 index를 반환한다. 발견하지 못한 경우 -1을 반환한다.
  - ![lastIndexOf](http://poiemaweb.com/img/lastindexof.png)

- String.prototype.replace()

  - 첫번째 인자에 전달된 문자열 또는 정규표현식을 대상 문자열에서 검색하여 두번째 인자에 전달된 문자열로 대체한다. 원본 문자열은 변경되지 않고 결과가 반영된 새로운 문자열을 반환한다.

  - ```js
    str.split([separator[, limit]])
    // separator: 구분 대상 문자열 또는 정규표현식
    // limit: 구분 대상수의 한계를 나타내는 정수
    ```

  - String의 메서드들은 값자체를 바꾸는게 아니라 값을 복사해서 리턴하는것이다. 그래서 리턴한 값을 받아줄 변수가 꼭 필요하다.

  - ```js
    var str = 'Hello Hello';

    var replacedStr = str.replace('Hello', 'Lee');

    // 결과가 반영된 새로운 문자열을 반환한다.
    console.log(replacedStr); // Lee Hello
    // 원본 문자열은 변경되지 않는다.
    console.log(str);         // Hello Hello

    replacedStr = str.replace(/hello/gi, 'Lee');
    /* 정규표현식
    i(Ignore Case): 대소문자를 구별하지 않고 검색한다.
    g(Global): 문자열 내의 모든 패턴을 검색한다.
    */

    console.log(replacedStr); // Lee Lee
    console.log(str);         // Hello Hello
    ```

    ​

- String.prototype.split()

  - 첫번째 인자에 전달된 문자열 또는 정규표현식을 대상 문자열에서 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다. **원본 문자열은 변경되지 않는다.**

  - ```js
    var str = 'How are you doing?';

    str.split([separator[, limit]])
    // separator: 구분 대상 문자열 또는 정규표현식
    // limit: 구분 대상수의 한계를 나타내는 정수
    ```


```js

  // 공백으로 구분하여 배열로 반환한다
  var splitStr = str.split(' ');
  console.log(splitStr); // [ 'How', 'are', 'you', 'doing?' ]
  // 원본 문자열은 변경되지 않는다
  console.log(str); // How are you doing?

  // 인수가 없는 경우, 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
  splitStr = str.split();
  console.log(splitStr); // [ 'How are you doing?' ]

  // 각 문자를 모두 분리한다
  splitStr = str.split('');
  console.log(splitStr); // [ 'H','o','w',' ','a','r','e',' ','y','o','u',' ','d','o','i','n','g','?' ]

  // 공백으로 구분하여 배열로 반환한다. 단 요소수는 3개까지만 허용한다
  splitStr = str.split(' ', 3);
  console.log(splitStr); // [ 'How', 'are', 'you' ]

  // 'o'으로 구분하여 배열로 반환한다.
  splitStr = str.split('o');
  console.log(splitStr); // [ 'H', 'w are y', 'u d', 'ing?' ]
```

- String.prototype.substring()

  - 첫번째 인자에 전달된 index에 해당하는 문자부터 두번째 인자에 전달된 index에 해당하는 문자의 **바로 이전 문자까지**를 모두 반환한다. 이때 첫번째 인수 < 두번째 인수의 관계가 성립된다.

  - ![substring](http://poiemaweb.com/img/substring.png)

  - ```js
    var str = 'Hello World'; // str.length == 11

    var res = str.substring(1, 4);
    console.log(res); // ell

    // 첫번째 인수 > 두번째 인수 : 두 인수는 교환된다.
    res = str.substring(4, 1);
    console.log(res); // ell

    // 두번째 인수가 생략된 경우 : 해당 문자열의 끝까지 반환한다.
    res = str.substring(4);
    console.log(res); // o World

    // 인수 < 0 또는 NaN인 경우 : 0으로 취급된다.
    res = str.substring(-2);
    console.log(res); // Hello World

    // 인수 > 문자열의 길이(str.length) : 인수는 문자열의 길이(str.length)으로 취급된다.
    res = str.substring(1, 12);
    console.log(res); // ello World

    res = str.substring(11);
    console.log(res); // ''

    res = str.substring(20);
    console.log(res); // ''

    res = str.substring(0, str.indexOf(' '));
    console.log(res); // 'Hello'

    res = str.substring(str.indexOf(' ') + 1, str.length);
    console.log(res); // 'World'
    ```
  ​

- String.prototype.toLowerCase()

  - 문자열의 문자를 소문자로 변경한다.

- String.prototype.toUpperCase()

  - 문자열의 문자를 소문자로 변경한다.

- String.prototype.trim()

  - 문자열 양쪽 끝에 있는 공백 문자를 제거한 문자열을 반환한다.

  - ```js
    var str = '   foo  ';
    var trimmedStr = str.trim();
    console.log(trimmedStr); // 'foo'
    console.log(str); // '  foo  ' 문자열 자신은 변경되지않는다!
    ```


---



## 2- 10. Error

Error 생성자는 error 객체를 생성한다. error 객체의 인스턴스는 런타임 에러가 발생하였을 때 throw된다. 

자바스크립트는 비동기함수때문에 에러를 콜러에게 던지기때문에  에러처리가 어렵다.



```js
try {
  // foo();
  throw new Error('Whoops!');
} catch (e) {
  console.log(e.name + ': ' + e.message);
}
```



# 3. 기본자료형과 래퍼객체



앞서 살펴본 바와 같이 각 표준 빌트인 객체(Standard Built-in Object)는 각자의 프로퍼티와 메소드를 가진다. 정적(static) 프로퍼티, 메소드는 해당 인스턴스를 생성하지 않아도 사용할 수 있고 *prototype에 속해있는 메소드는 해당 prototype을 **상속받은 인스턴스**가 있어야만 사용*할 수 있다.

그런데 기본자료형의 값에 대해 표준 빌트인 객체의 메소드를 호출하면 정상적으로 작동한다.

```js
var str = 'Hello world!';
var res = str.toUpperCase();
console.log(res); // 'HELLO WORLD!'

var num = 1.5;
console.log(num.toFixed()); // 2

```

이는 기본자료형의 값에 대해 표준 빌트인 객체의 메소드를 호출할 때, **기본자료형의 값은 연관된 객체(Wrapper 객체)로 일시 변환** 되기 때문에 가능한 것이다. 그리고 메소드 호출이 종료되면 객체로 변환된 기본자료형의 값은 다시 기본자료형의 값으로 복귀한다.



