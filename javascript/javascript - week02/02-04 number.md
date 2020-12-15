Number 객체는 기본자료형 number를 다룰 때 유용한 프로퍼티와 메소드를 제공하는 레퍼(wrapper) 객체이다. *변수 또는 객체의 프로퍼티가 숫자를 값으로 가지고 있다면* Number 객체의 별도 생성없이 **Number 객체의 프로퍼티와 메소드**를 사용할 수 있다.

기본자료형이 warpper 객체의 메소드를 사용할 수 있는 이유는 빌트인메소드를 호출할때, **기본자료형의 값은 연관된 객체(Wrapper 객체)로 일시 변환** 되기 때문에 가능한 것이다.

```js
var num = 1.5; // 5는 number인데?
// 레퍼객체로 감싸지는 이유는 프로토타입객체를 사용하기위해서
console.log(num.toFixed()); // 2 일시적으로 wrapper객체로 변환되어서 Number.prototype.toFixed()가 가능해진것

// toFixed() 함수 선언 후 
console.log(toFixed(num)); // prototype 메서드가 아니였다면 이런식으로 썻어야했다.
```



메소드의 3가지 호출방법

1. 일반 메서드 : 생성자함수로 생성된 인스턴스객체에 this가 연결되어있다. 
2. 프로토타입 메서드 :  생성자함수로 생성된 인스턴스 객체에 직접 추가되는게 아니라 \__proto__ 에 있다.





스태틱메서드 : 인스턴스를 생성하지않아도 호출할 수 있다.

메서드 : 인스턴스를 생성하지않으면 호출할 수 없다.

메서드내부에 this가있으면 프로토타입 메서드 없으면 스태틱 메서드



---

# 1. Number Constructor

Number 객체는 Number() 생성자 함수를 통해 생성할 수 있다.

```js
var x = new Number(123);
var y = new Number('123');
var z = new Number('str');

console.log(x); // 123
console.log(y); // 123
console.log(z); // NaN  숫자로 변환될수없다면 Not a Number를 반환한다.

var x = 123;
var y = new Number(123);

console.log(typeof x); // number
console.log(typeof y); // object  생성자함수로 생성해서
```



---

# 2. Number Property (static)

정적(static) 프로퍼티로 Number 객체를 생성할 필요없이 `Number.propertyName`의 형태로 사용한다.

정적(변하지않는), 숫자에서 정해진 값(상수)들을 저장한 속성이다. 상수는 **대문자**로 표기한다.

  - 2.1 Number.EPSILON

      - 1과 2사이에는 무수히 많은 수가 있다. 하지만 컴퓨터에서는 그 숫자들을 모두 표현할 수 없으니 컴퓨터가 만들어낸 단위이다.

          - 1.1 다음에 x라는 수가 온다고치자   x- 1.1 = ?   
          - ? 가 최소단위인 EPSILON이다.

      - Number.EPSILON은 약2.2204460492503130808472633361816E-16 또는 2-52이다.

      - ```js
        console.log(0.1 + 0.2);        // 0.30000000000000004
        console.log(0.1 + 0.2 == 0.3); // false!!!
        function isEqual(a, b){
          // 즉 a와 b의 차이가 JavaScript에서 표현할 수 있는 가장 작은 수인 Number.EPSILON보다 작으면 같은 수로 인정할 수 있다.
          return Math.abs(a - b) < Number.EPSILON;
          // Math.abs는 절댓값을 반환한다. 0.00000000000000004  < 2.2204460492503130808472633361816E-16
        }

        console.log(isEqual(0.1 + 0.2, 0.3));
        ```

  - 2.2 Number.MAX_VALUE

      - 사용 가능한 가장 큰 숫자(1.7976931348623157e+308)를 반환
      - MAX_VALUE보다 큰 숫자는 `Infinity`이다.

  - 2.3 Number.MIN_VALUE

      - 사용 가능한 가장 작은 숫자(5e-324)를 반환
      - MIN_VALUE는 0에 가장 가까운 양수 값이다. MIN_VALUE보다 작은 숫자는 0으로 변환된다.

  - 2.4 Number.POSITIVE_INFINITY

      - 양의 무한대 `Infinity`를 반환

  - 2.5 Number.NEGATIVE_INFINITY

      - 음의 무한대 `-Infinity`를 반환

  - 2.6 Number.NaN

      - 숫자가 아님(Not-a-Number)을 나타내는 숫자값





---

# 3. Number Method
  - 3.1 Number.isFinite()

      - Number.isFinite()는 전역 함수 isFinite()와 차이가 있다. Number.isFinite()은 인자값을 형변환하지 않는다.

      - ```js
        Number.isFinite(NaN) // false
        Number.isFinite('1') // false
        isFinite("1") // true
        ```

- 3.2Number.isInteger()

  - 매개변수에 전달된 값이 정수(Integer)인지 검사하여 그 결과를 Boolean으로 반환
  - 형변환하지 않는다.

- 3.3 Number.isNaN()

  - 매개변수에 전달된 값이 NaN인지를 검사하여 그 결과를 Boolean으로 반환
  - 형변환하지 않는다.

- 3.4 Number.isSafeInteger()

  - 쓸 수 있는 정수값인지 아닌지 Boolean으로 반환한다.
  - 형변환 하지않는다.

- 3.5 Number.prototype.toExponential()

  - 대상을 지수 표기법으로 변환하여 문자열로 반환

  -  e(Exponent) 앞에 있는 숫자에 10의 n승이 곱하는 형식으로 수를 나타내는 방식이다.

  - ```js
    // 1234 = 1.234e+3  // e는 *10을 의미함 + 는 지수
    var numObj = 77.1234;

    console.log(numObj.toExponential());  // logs 7.71234e+1
    console.log(numObj.toExponential(4)); // logs 7.7123e+1
    console.log(numObj.toExponential(2)); // logs 7.71e+1
    console.log(77.1234.toExponential()); // logs 7.71234e+1

    // 자바스크립트의 설계오류
    console.log(77.toExponential());      // SyntaxError: Invalid or unexpected token  이게 안되고
    console.log(77 .toExponential());     // logs 7.7e+1 이게된다...
    ```

  ​

- 3.6 Number.prototype.toFixed()

  - 매개변수로 지정된 소숫점자리를 반올림하여 ***문자열***로 반환

  - ```js
    numObj.toFixed([digits])
    // digits: 0~20 사이의 정수값으로 소숫점 이하 자릿수를 나타낸다. 기본값은 0이며 옵션으로 생략 가능하다.
    // 소숫점 이하 반올림
    console.log(numObj.toFixed());   // '12346'
    // 소숫점 이하 1자리수 유효, 나머지 반올림
    console.log(numObj.toFixed(1));  // '12345.7'
    ```

- 3.7 Number.prototype.toPrecision()

  - 매개변수로 지정된 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환

  - ```js
    numObj.toPrecision([precision])
    // precision: 1~21 사이의 정수값으로 전체 자릿수를 나타낸다. 옵션으로 생략 가능하다.
    var numObj = 15345.6789;

    // 전체자리수 유효
    console.log(numObj.toPrecision());   // '12345.6789'
    // 전체 1자리수 유효, 나머지 반올림
    console.log(numObj.toPrecision(1));  // '2e+4'  // 2밑으로 반올림됨
    // 전체 2자리수 유효, 나머지 반올림
    console.log(numObj.toPrecision(2));  // '1.5e+4'
    // 전체 3자리수 유효, 나머지 반올림
    console.log(numObj.toPrecision(3));  // '1.53e+4'
    // 전체 6자리수 유효, 나머지 반올림
    console.log(numObj.toPrecision(6));  // '12345.7'
    ```

    ​

- 3.8 Number.prototype.toString()

  - 숫자를 문자열로 변환하여 반환한다.

    - ```js
      // 변환하는데는 여러방법이있다.
      var x = 1;
      x+'';
      x.toString();
      String(x); // 이 방법은 알고만있고 쓰지말자
      ```

      ​

- 3.9 Number.prototype.valueOf()

  - Number 객체의 기본자료형 값(primitive value)을 반환

  - ```js
    var numObj = new Number(10);
    console.log(typeof numObj); // object
    ```