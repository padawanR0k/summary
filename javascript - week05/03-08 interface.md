# 1. Introduction

인터페이스는 일반적으로 **타입 체크를 위해 사용되며 일반 변수, 함수, 클래스에 사용**할 수 있다. 인터페이스는 여러가지 자료형을 갖는 프로퍼티로 이루어진 새로운 자료형을 정의하는 것과 유사하다. 인터페이스에 선언된 프로퍼티 또는 메소드의 구현을 강제하여 일관성을 유지할 수 있도록 하는 것이다.

**인터페이스**는 멤버변수와 메소드를 가질 수 있다는 점에서 클래스와 유사하나 **직접 인스턴스를 생성할 수는 없다.**

클래스와 인터페이스는 협업할때 매우 유용하다.

# 2. 변수와 인터페이스

인터페이스는 일반 변수의 타입으로 사용할 수 있다. 이때 인터페이스를 타입으로 선언받은 변수는 해당 인터페이스와 구성이 똑같아야 한다. 이것은 새로운 자료형을 정의하는 느낌이다.

```typescript
// 인터페이스의 정의 == 이 인터페이스를 가진 객체를 Todo 타입이라고 부르자
interface Todo {
  id: number;
  content: string;
  completed: boolean;
}

// 변수 todos의 타입은 위에 지정해놓은 인터페이스의 타입이다. == 구성이 똑같아야함
let todos: Todo[];

// 변수 todos는 Todo 인터페이스를 준수하여야 한다.
todos = [
  { id: 1, content: 'typescript', completed: false }
];



// 파라미터 todo의 타입으로 Todo 인터페이스를 선언하였다.
function addTodo(todo: Todo) {
  console.log(todo.content);
}

// 파라미터 todo는 Todo 인터페이스를 준수하여야 한다.
const newTodo: Todo = { id: 1, content: 'typescript', completed: false };
addTodo(newTodo);
```



# 3. 함수와 인터페이스

```typescript
// 함수 인터페이스의 정의
interface SquareFunc {
  (num: number): number;
 // (매개변수의 타입) : 리턴값의 타입;
}

// 함수 인테페이스를 구현하는 함수는 인터페이스를 준수하여야 한다.
const squareFunc: SquareFunc = function (num: number) {
  return num * num;
}

console.log(squareFunc(10)); // 100
```



# 4. 클래스와 인터페이스

클래스 선언문의 **implements** 뒤에 인터페이스를 선언하면 해당 클래스는 지정된 인터페이스를 반드시 구현하여야 한다. 

**인터페이스는** 프로퍼티와 메소드를 가질 수 있다는 점에서 클래스와 유사하나 **직접 인스턴스를 생성할 수는 없다.**

```typescript
// 인터페이스의 정의
interface ITodo { // 대문자 I가 인터페이스임을 암시함
  id: number;
  content: string;
  completed: boolean;
}

// Todo 클래스는 ITodo 인터페이스를 구현하여야 한다.
class Todo implements ITodo {
  constructor (
    public id: number,
    public content: string,
    public completed: boolean
  ) { }
}

const todo = new Todo(1, 'Typescript', false); // type은 Todo이다.

console.log(todo);
```



인터페이스는 멤버변수뿐만 아니라 메소드도 포함할 수 있다. **인터페이스를 구현하는 클래스**는 인터페이스에서 정의한 **멤버변수와 메소드를 반드시 구현**하여야 한다.

```typescript
// 인터페이스의 정의
interface IPerson {
  name: string;
  sayHello(): void;
}

// 인터페이스를 구현하는 클래스는 인터페이스에서 정의한 멤버변수와 메소드를 반드시 구현하여야 한다.
class Person implements IPerson {
  constructor(public name: string) {}

  sayHello() {
    console.log(`Hello ${this.name}`);
  }
}

function greeter(person: IPerson): void {
  person.sayHello();
}

const me = new Person('Lee'); // me는 Person 타입이기도하면서 IPerson타입이기도하다.
// 다만 IPerson타입이 더 추상화단계가 높다.
// IPerson --- Person --- me
greeter(me); // Hello Lee
```





# 5. 덕 타이핑 (Duck typing)

`implements` 로 IDuck 을 구현하지 않았으나 똑같이 quack()이라는 메소드를 가지고있기 때문에 

(같은 값을 가지고있다) makeNoise(duck: Iduck) 이 타입체크가 통과된다. 이런 현상을 덕 타이핑 ( 구조적 타이핑) 이라고한다.

```typescript
interface IDuck { // 1
  quack(): void;
}

class MallardDuck implements IDuck { // 3
  quack() {
    console.log('Quack!');
  }
}

class RedheadDuck { // 4
  quack() {
    console.log('q~uack!');
  }
}

function makeNoise(duck: IDuck): void { // 2
  duck.quack();
}

makeNoise(new MallardDuck()); // Quack!
makeNoise(new RedheadDuck()); // q~uack! // 5
```



```typescript
interface IPerson {
  name: string;
}

function sayHello(person: IPerson): void {
  console.log(`Hello ${person.name}`);
}

const me = { name: 'Lee', age: 18 }; // age가 있던없던 name이라는 값이 있어서 통과
sayHello(me); // Hello Lee
```

인터페이스는 개발 단계에서 도움을 주기 위해 제공되는 기능으로 자바스크립트의 표준이 아니다. 따라서 위 예제의 TypeScript 코드가 자바스크립트 코드로 컴파일되면 아래와 같이 인터페이스가 삭제된다.

```js
function sayHello(person) {
  console.log("Hello " + person.name);
}
var me = { name: 'Lee', age: 18 };
sayHello(me); // Hello Lee
```



# 6. 선택적 프로퍼티 (Optional Property)

인터페이스의 프로퍼티는 반드시 구현되어야 하지만 선택적으로 필요한 경우가 있다. 이 때 프로퍼티명 뒤에 `?`를 붙여 선택적 프로퍼티로 만들 수 있다.

```typescript
interface IUserInfo { 
  username: string;
  password: string;
  age?    : number;
  address?: string;
}

const userInfo: IUserInfo = {
  username: '123456@gmail.com',
  password: '123456'
}

console.log(userInfo);
```

