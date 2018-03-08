대규모 애플리케이션을 만들때는 동적타입보다는 정적타입이 유리하다

자바스크립트는 타입을 명시하지않았다. 그리고 느슨한 타입을 가지고 있어서 하나의 변수에 여러 자료형을 할당할 수 있엇다.

# 1. 타입선언

```typescript
let foo: string = 'hello';
```

선언한 타입에 맞지 않는 값을 할당하면 컴파일 시점에 에러가 발생한다.

```typescript
let bar: number = true; // error TS2322: Type 'true' is not assignable to type 'number'.
```

![type-error](http://poiemaweb.com/img/type-error.png)

 VSCode와 같은 툴을 사용하면 코드 작성 시점에 에러를 검출할 수 있어서 개발효율이 대폭 향상된다.



```typescript
// 함수선언식
function multiply1(x: number, y: number): number { // ()에는 매개변수의 타입 ()뒤에는 return값의 타입  ! return 값의 타입은 필수가 아니다.
  return x * y;
}

// 함수표현식
const multiply2 = (x: number, y: number): number => x * y;
```

매개변수에 타입을 지정하기때문에 방어코드를 작성하지 않아도 된다.



TypeScript는 ES5, ES6의 Superset(상위확장)이므로 JavaScript의 타입을 그대로 사용할 수 있다. JavaScript의 타입 이외에도 TypeScript 고유의 타입이 추가로 제공된다.

|   Type    |  JS  |  TS  | Description                                                  |
| :-------: | :--: | :--: | ------------------------------------------------------------ |
|  boolean  |  ◯   |      | true와 false                                                 |
|   null    |  ◯   |      | primitives 또는 object형 변수에 값이 없다는 것을 명시        |
| undefined |  ◯   |      | 값을 할당하지 않은 변수의 초기값                             |
|  number   |  ◯   |      | 숫자(정수와 실수, Infinity, NaN)                             |
|  string   |  ◯   |      | 문자열                                                       |
|  symbol   |  ◯   |      | 고유하고 수정 불가능한 데이터 타입이며 주로 객체 프로퍼티들의 식별자로 사용(ES6에서 추가) |
|  object   |  ◯   |      | 객체형, 참조형                                               |
|   array   |      |  ◯   | 배열                                                         |
|   tuple   |      |  ◯   | 고정된 요소수 만큼의 자료형을 미리 선언후 배열을 표현        |
|   enum    |      |  ◯   | 열거형. 숫자값 집합에 이름을 지정한 것이다.                  |
|    any    |      |  ◯   | 타입 추론(type inference)할 수 없거나 타입 체크가 필요없는 변수는 any 타입으로 선언한다. |
|   void    |      |  ◯   | 일반적으로 함수에서 반환값이 없을 경우 사용한다.             |
|   never   |      |  ◯   | 결코 발생하지 않는 값                                        |

```typescript
let isDone: boolean = false; // 실무에서는 선언과 동시에 할당할땐 굳이 타입을 써 줄필요가 없다.

// array
let list1: any[] = [1, 'two', true]; // 요소들의 타입을 지정할 수 있다.
let list2: number[] = [1, 2, 3];
let list3: Array<number> = [1, 2, 3]; // Generic array type

// tuple : 고정된 요소수 만큼의 타입을 미리 선언후 배열을 표현
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error

// enum : 열거형은 숫자값 집합에 이름을 지정한 것. 상수들의 집합체를 만들때 사용한다.
enum Color1 {Red, Green, Blue};
let c1: Color1 = Color1.Green;

enum TodoStatus {'All','Actvie','Completed'};
let status = TodoStatus.All

// void : 일반적으로 함수에서 반환값이 없을 경우 사용한다.
function warnUser(): void {
  console.log("This is my warning message");
}

// never : 결코 발생하지 않는 값
function infiniteLoop(): never {
  while (true) {}
}

function error(message: string): never {
  throw new Error(message);
}
```



# 2. 정적 타이핑 (Static Typing)

| 정적 타이핑                                                  | 동적 타이핑                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| C나 Java같은 C-family 언어는 변수 선언 시 변수에 저장할 값의 종류에 따라 사전에 타입을 선언(Type declaration)하여야 하며 선언한 타입에 맞는 값을 할당하여 한다. | JavaScript는 동적 타입(dynamic typed) 언어 혹은 느슨한 타입(loosely typed) 언어이다. 이것은 변수의 타입 선언없이 값이 할당되는 과정에서 동적으로 타입이 추론(타입추론 Type Inference)될 것이라는 뜻이다. 따라서 같은 변수에 여러 타입의 값을 교차하여 대입할 수 있다. 이를 동적 타이핑(Dynamic Typing)이라 한다. |

TypeScript는 정작타이핑을 지원힌다.

```typescript
let foo: string,   // 문자열 타입
    bar: number,   // 숫자 타입
    baz: boolean;  // 불리언 타입

foo = 'Hello'; 
bar = 123;
baz = 'true'; // error: Type '"true"' is not assignable to type 'boolean'.

let bar = 123; // left bar: number와 같다.
let = 'bye'; // error: Type '"hi"' is not assignable to type 'number'.

let a; // let a: any와 같다. 어떤 것도 재할당가능하다.


//정적 타이핑은 변수는 물론 함수의 매개변수와 리턴값에도 사용할 수 있다.

function add(x: number, y: number): number {
  return x + y;
}

console.log(add(10, 10)); // 20
```

정적 타이팅의 장점은 **코드 가독성, 예측성, 안정성의 향상**이라고 볼 수 있는데 이는 대규모 프로젝트에 매우 적합하다.