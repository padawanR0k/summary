# 자료형과 변수

![변수 선언과 할당구조](http://poiemaweb.com/img/memory_address.png)

메모리에는 주소(address)가 있다. 어떤 값에 접근할때 주소를 참조하여 찾아간다. 주소(address)는 16진수로 인간이 보기엔 어렵기때문에 변수가 주소(address)를 대신 기억 해준다. ~~pointer~~

32bit : 4byte 

64bit : 8byte 

왜 1byte 는 1 bit 인가 

ASCII  :  영어의 대소문자를 표현하려면 한글자당 2^8 비트가 필요해서 

```javascript
var x;
x = 10;
```

위 구문을 실행하면 어떻게되는가?

1. OS에서 x가 들어올 메모리를 확보함. 자바스크립트에서는 미리 data type을 지정하지 않기 때문에  undefiend를 넣는다. (동적타입)
2. 어떤 타입인지 알게되면 새롭게 메모리를 확보해서  그곳에 저장한다.

---

## 1. Date Type (자료형)

메모리를 효율적으로 확보하기위해 자료형이 필요하게 됐다.



- 기본 자료형 (primitive data type) : 값이 변경불가능하다.
  - `Boolean`
    - true
    - false
  - `null`
    - 값에 null을 지정하면 값을 지정하는 포인터가 사라진다.
    - 이때, 값은 아무도 참조하지않는 값이 되고 [가비지 콜렉터](http://12bme.tistory.com/57)가 찾아낸다.
  - `undefined`
    - 아직 값이 할당되지 않는 변수에 값을 초기화
  - `Number`
    - 0, 정수, 실수, 양의 무한대,음의 무한대,infinity, NaN (Not  a Number)
  - `String`
  - `Symbol` (ECMAScript 6에서 추가)
- 객체형 (Object type) : 값이 변경가능하다.
  - `Object`

Javascript의 자료형은 크게 **기본 자료형(primitive data type)**과 **객체형(참조형)**으로 구분할 수 있다. [참고자료](http://codingnuri.com/javascript-tutorial/javascript-primitive-types-and-reference-types.html)



---

### 1- 1. 기본자료형 (Primitive Data Type)

기본자료형의 값은 **변경 불가능한 값(immutable value)**이며 **pass-by-value(값으로 접근)** 이다. 또한 이들 값은 메모리의 스택 영역(Stack Segment)에 고정된 메모리 영역을 점유하고 저장된다.

- immutable value : 메모리상에서 변경이 되는게아니다. 새로운 다른 자리에 값이 추가되는것
- pass-by-value : 주소(address)를 지정하는게아니라 값을 지정하는것

|      Code : 작성한 소스코드       |
| :------------------------: |
|      Data : 전역변수를 저장       |
| Stack : 지역변수, 매개변수, 리턴값 저장 |
|     Heap  : 참조형 변수를 저장     |

Code와 Data는 컴파일타임에 정해진다.

Stack : 정해진 값을 저장 ex) 정수 

Heap : 크기가 정해지지 않은  변수를 저장 ex) 객체

#### 1- 1. 1 Boolean

논리적인 요소를 나타낸다. null, undefined, 0, 빈 문자열은 false로 간주된다.

#### 1- 1. 2 null

null타입은 `null`타입만 가질수 있다.   

null은 의도적으로 기본형(primitives) 또는 object형 변수에 값이 없다는 것을 명시한 것. 이는 메모리 address의 참조정보를 제거하는 것을 의미한다.

#### 1- 1.3 undefined

값을 할당하지않으면 변수는 undefined 값을 가진다.

#### 1- 1.4 Number

여러  숫자 자료형이  존재하는 C언어와 달리 자바스크립트는 하나의 숫자 자료형만 존재한다.

- `+/- Infinity`
- `NaN` (not-a-number)

#### 1- 1.5 String

문자열 타입은 텍스트 데이터를 나타낸다. 유니코드(16비트 부호없는 정수 값) 문자들의 집합이다.

####변경 불가능한 값(immutable value)####

```javascript
var str = 'Hello';
str = 'world';
```



아래표가 추상적인 메모리라고 치자

|                 |
| --------------- |
| ~~Hello~~ world |
|                 |
|                 |

이렇게 변경되는게아니라

|       |
| ----- |
| Hello |
|       |
| world |

위 처럼 변수를 재할당했을때 메모리는 Hellow를 지우는게아니라 다른 위치에 world를 추가한다.



```javascript
var a = 1;
var b = a;
```

위의 경우

| a,b  | 1    |
| ---- | ---- |
|      |      |
|      |      |

이 아니라

| a    | 1    |
| ---- | ---- |
| b    | 1    |
|      |      |

#### 문자열은 유사배열이다

```javascript
var str = 'string';
console.log(str[0]); // s
console.log(str[5]); // g

str[0] = 'S'; //  한번 생성된 문자열은 read only로서 수정은 불가하다. 이것을 변경 불가능(immutable)이라 한다.
console.log(str); // String
```

---

### 1. 2. 객체형 (Object Type, 참조형)

객체는 이름과 값을 가지는 데이터를 의미하는 프로퍼티(property)와 동작을 의미하는 메소드(method)를 포함할 수 있는 독립적 주체이다.

자바스크립트는 객체(object)기반의 스크립트 언어로서 자바스크립트를 이루고 있는 거의 “모든 것”이 객체이다. 기본자료형(Primitives)을 제외한 나머지 값들(배열, 함수, 정규표현식 등)은 모두 객체이다. 또한 객체는 **pass-by-reference(참조로 접근)**이며 메모리의 힙 영역(Heap Segment)에 저장된다.



***

## 2. 변수 (Variable)

변수는 주소(address)를 기억하는 저장소이다. 즉 변수란 메모리 주소(Memory address)에 접근하기 위해 사람이 이해할 수 있는 언어로 지정한 식별자(identifier)이다.

자바스크립트에서는 변수를 선언할때 `var`키워드를 사용한다.

---

### 2- 1. 변수의 중복선언

```javascript
var x = 1;
var x = 100;
```

변수의 중복선언이 문법적으로 허용된다. 중복이 허용되기 때문에 여러인원이 협업시 원치않는 변수값변경이 일어날 수 있다.

---

### 2- 2. var 키워드 생략허용

```javascript
x = 1 ;
console.log(x); // 1 
```

이 때 x는 전역변수가 된다.

---

### 2- 3. 동적타이핑 (Dynamic Typing)

변수의 타입지정이 필요없이 값이 할당되는 과정에서 자동으로 자료형이 결정(Type Inference)될 것이라는 뜻이다. 따라서 같은 변수에 여러 자료형(data type)의 값을 대입할 수 있다. 이를 동적 타이핑(Dynamic Typing)이라 한다.

```javascript
var foo;

console.log(typeof foo);  // undefined

foo = null;
console.log(typeof foo);  // object <-- 설계상 오류

foo = {};
console.log(typeof foo);  // object

foo = 3;
console.log(typeof foo);  // number

foo = 'Hi';
console.log(typeof foo);  // string

foo = true;
console.log(typeof foo);  // boolean

```

---

### 2- 4. 변수 호이스팅 (Variable Hoisting)

변수를 선언하기 이전에 참조할 수 있는것, 단 undefined

```javascript
console.log(foo); // ① undefined
var foo = 123;
console.log(foo); // ② 123
{
  var foo = 456;
  // 지역변수
}
console.log(foo); // ③ 456 

// 전역변수
```

자바스크립트는 블록범위안에서 변수를 선언해도 전역변수가 된다. (재할당됨)

함수범위안에서만 지역변수 선언이 가능하다.

---

### 2- 5. var 키워드로 선언된 변수의 문제점

1. Function-level scope
   - 전역 변수의 남발
   - for loop 초기화식에서 사용한 변수를 for loop 외부 또는 전역에서 참조할 수 있다.
2. var 키워드 생략 허용
   - 의도하지 않은 변수의 전역화
3. 중복 선언 허용
   - 의도하지 않은 변수값 변경
4. 변수 호이스팅
   - 변수를 선언하기 전에 참조가 가능하다

변수의 범위(scope)는 좁을수록 좋다.

***



## 오늘의 TIP

- 코딩컨벤션을 도와주는 툴  - ESLint
  - `" "` 말고 `''` 를 쓰자,  카멜케이스를 쓰자

