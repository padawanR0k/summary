# 연산자 

## 1. 산술 연산자

| Operator | Description |
| -------- | ----------- |
| +        | 덧셈          |
| -        | 뺄셈          |
| *        | 곱셈          |
| /        | 나눗셈         |
| %        | 나머지         |
| ++       | 증가          |
| --       | 감소          |

```javascript
var x = 5;
var y = 2;
var z;

z = x + y;  // 7
z = x - y;  // 3
z = x * y;  // 10
z = x / y;  // 2.5
z = x % y;  // 1
z = x++;    // 5 선대입후증가  = 먼저 할당하고 증가시킴
z = ++x;    // 7 선증가후대입  = 증가 시키고 할당
z = x--;    // 7 선대입후감소
z = --x;    // 5 선감소후대입

var str1 = '5' + 5;      // '55'
var str2 = 5 + '5';      // '55'
// 연산대상중에 문자열이있으면 자동으로 결과를 문자열로 변환한다.

var str3 = 'Hello' + 5;  // 'Hello5'
```

---

## 2. 대입연산자

| Operator | Example | Same As   |
| -------- | ------- | --------- |
| =        | x = y   | x = y     |
| +=       | x += y  | x = x + y |
| -=       | x -= y  | x = x - y |
| *=       | x *= y  | x = x * y |
| /=       | x /= y  | x = x / y |
| %=       | x %= y  | x = x % y |



---

## 3. 비교 연산자 

| Operator | Description                              |
| -------- | ---------------------------------------- |
| ==       | 동등비교 (loose equality) 형변환 후, 비교한다.       |
| ===      | 일치비교 (strict equality) 타입까지 일치하여야 true를 반환한다. |
| !=       | 부등비교                                     |
| !==      | 불일치비교                                    |
| >        | 관계비교                                     |
| <        | 관계비교                                     |
| >=       | 관계비교                                     |
| <=       | 관계비교                                     |
| ?        | 삼항 연산자                                   |

```javascript
var x = 5

x == 5    // true
x == '5'  // true // 이런 상황을 피하기 위하여
x == 8    // false

x === 5   // true // 이걸 쓰도록하자
x === '5' // false

// 삼항연산자(ternary operator)
// 조건 ? 조건이 ture일때 반환할 값 : 조건이 false일때 반환할 값
var condition = true;
var result = condition ? 'true' : 'false';
console.log(result); // 'true'

// 삼항연산자는 아래와 같은 한눈에 보기 쉬운 조건문일 경우에 쓰자
// id의 길이가 INPUT_ID_MIN_LEN보다 작으면 에러 메시지를 출력한다.
var id = 'lee';
var INPUT_ID_MIN_LEN = 5;
var errMsg = id.length < INPUT_ID_MIN_LEN ? '아이디는 5자리 이상으로 입력하세요' : '성공';
console.log(errMsg); // '아이디는 5자리 이상으로 입력하세요'
```



---



## 4. 논리 연산자

| Operator | Description |
| -------- | ----------- |
| \|\|     | or          |
| &&       | and         |
| !        | not         |

---

## 5. 단축 평가 (Short-Circuit Evaluation)



```javascript
// || (논리 합) 연산자
var o1 =  true || true;     // t || t returns true
var o2 = false || true;     // f || t returns true
var o3 =  true || false;    // t || f returns true
var o4 = false || (3 == 4); // f || f returns false
// 논리 합은 앞의값이 true면 무조건 true.

// && (논리곱) 연산자
var a1 =  true && true;     // t && t returns true
var a2 =  true && false;    // t && f returns false
var a3 = false && true;     // f && t returns false
var a4 = false && (3 == 4); // f && f returns false
// 논리곱은 뒤에 논리값이 결정한다.

// 자료형 초기화
function foo (str) {
  str = str || ''; // 이런걸 "방어코드" 라고함
  // do somethig with str
  console.log(str.length);
}


// example
var obj = {
  // foo: 'hi',
  bar: 'hey'
};
객체는 데이터, 함수 둘다 가져 올 수 있다.

console.log('obj.foo is ' + obj.foo); // obj.foo는 undefined다. undefined일때 console.log()를 실행시키지 않으려면 아래와 같이 처리하면 된다. 

if (obj && obj.foo) {
  // do somethig with obj.foo
  console.log('obj.foo is ' + obj.foo);
}
```

---

## 6. 타입 연산자 

| Operator   | Description                              |
| ---------- | ---------------------------------------- |
| typeof     | 피연산자의 데이터 타입(자료형)을 문자열로 반환한다. null과 배열의 경우 object, 함수의 경우 function를 반환하는 것에 유의하여야 한다. |
| instanceof | 객체가 동일 객체형의 인스턴스이면 `true`를 반환한다.         |

```javascript
console.log(typeof 'John');                 // string
console.log(typeof 3.14);                   // number
console.log(typeof NaN);                    // number
console.log(typeof false);                  // boolean
console.log(typeof [1, 2, 3, 4]);           // object
console.log(typeof {name:'John', age:34});  // object
console.log(typeof new Date());             // object
console.log(typeof function () {});         // function
console.log(typeof myCar);                  // undefined (설계적 결함)
console.log(typeof null);                   // object (설계적 결함)

function Person() {}
var me = new Person();
console.log(me instanceof Person); // true
```



---

## 7. !!

값이 false인지 true인지 확인할 수 있다.

```javascript
console.log(!!1);         // true
console.log(!!0);         // false
console.log(!!'string');  // true
console.log(!!'');        // false
console.log(!!null);      // false
console.log(!!undefined); // false
console.log(!!{});        // true
console.log(!![]);        // true
```

