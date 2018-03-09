ES6의 Class는 메서드들만 들어올수 있다. private, public 못씀

TypeScript은 Class에 멤버변수 쓸 수 있다. private, 추상클래스, 인터페이스 사용가능



# 1. 클래스 정의 (Class Definition)

```js
// person.js ES6
class Person {
  constructor(name) {
    // 멤버 변수의 선언과 초기화
    this.name = name; // ES6에서는 멤버변수는 constructor내부에 this에 선언해야해서 모든 멤버변수가 public하게 된다.
  }

  walk() {
    console.log(`${this.name} is walking.`);
  }
}
```



ES6에는 반드시 멤버변수를 먼저 선언해야한다.

```typescript
// person.ts
class Person {
  // 멤버 변수를 사전 선언하여야 한다
  name: string; 

  constructor(name: string) {
    // 멤버 변수에 값을 할당
    this.name = name;
  }

  walk() {
    console.log(`${this.name} is walking.`);
  }
}

const person = new Foo('Lee');
person.walk(); // Lee is walking
```



# 2. 접근 제한자 (Access modifier)

Java, C#과 같은 클래스 기반 객체 지향 언어가 지원하는 public, private, protected 접근 제한자를 지원하며 의미 또한 기본적으로 동일하다.

 접근 제한자를 생략한 멤버 변수와 메소드는 암묵적으로 public이 선언된다. 따라서 **public으로 지정하고자 하는 멤버 변수와 메소드는 접근 제한자를 생략**한다.

| 접근 가능성     | public | protected | private |
| --------------- | :----: | :-------: | :-----: |
| 클래스 내부     |   ◯    |     ◯     |    ◯    |
| 자식 클래스     |   ◯    |     ◯     |    ✕    |
| 클래스 인스턴스 |   ◯    |     ✕     |    ✕    |

```typescript
class Foo {
  public x: string;
  protected y: string;
  private z: string;

  constructor(x: string, y: string, z: string) {
    // public, protected, private 접근 제한자 모두 클래스 내부에서 참조 가능하다.
    this.x = x;
    this.y = y;
    this.z = z;
  }
}
// public 접근 제한자는 클래스 인스턴스를 통해 참조 가능하다.
console.log(foo.x);
console.log(foo.y); // 레퍼런스 에러
console.log(foo.z); // 레퍼런스 에러


class Bar extends Foo {
  constructor(x: string, y: string, z: string) {
    super(x, y, z);
      
    // public 접근 제한자는 자식 클래스에서 참조 가능하다.
    console.log(this.x);
    // protected 접근 제한자는 자식 클래스에서 참조 가능하다.
    console.log(this.y);
    // private 접근 제한자는 자식 클래스에서 참조할 수 없다.
    console.log(this.z); // error 
  }
}
```



# 3. 생성자 파라미터에 접근 제한자 선언

```typescript
class Foo {
  // 접근 제한자가 선언된 생성자 파라미터 x는 멤버 변수로 선언되고 초기화가 자동 수행된다
  // public이 선언되었으므로 x는 클래스 외부에서도 참조가 가능하다
  constructor(public x: string) { // 파라미터에 쓰면 위에 따로 접근제한자를 안써줘도 된다.
  }    
}

const foo = new Foo('Hello');
console.log(foo);   // Foo { x: 'Hello' }
console.log(foo.x); // Hello
```



```typescript
class Foo {
  // 만일 생성자 파라미터에 접근 제한자를 선언하지 않으면 생성자 파라미터는 생성자 내부에서만 유효한 지역 변수가 되어 생성자 외부에서 참조가 불가능하게 된다.
    
  // x는 생성자 내부에서만 유효한 지역 변수이다.
  constructor(x: string) {
    console.log(x);
  }
}

const foo = new Foo('Hello');
console.log(foo); // Foo {}
```



# 4. readonly 키워드

`readonly` 키워드를 사용할 수 있다. readonly가 선언된 프로퍼티는 선언시 또는 생성자 내부에서만 값을 할당할 수 있다. 그 외의 경우에는 값을 할당할 수 없고 오직 읽기만 가능한 상태가 된다.

`const`키워드랑 똑같다고 생각하자.

```typescript
class Foo {
  private readonly MAX_LEN: number = 5; // 상수처럼 쓴다! 재할당 불가
  private readonly MSG: string; // 상수처럼 쓴다! 재할당 불가

  constructor() {
    this.MSG = 'hello';
  }

  log() {
    // readonly가 선언된 프로퍼티는 재할당이 금지된다.
    this.MAX_LEN = 10; // Cannot assign to 'MAX_LEN' because it is a constant or a read-only property.
    this.MSG = 'Hi'; // Cannot assign to 'MSG' because it is a constant or a read-only property.

    console.log(`MAX_LEN: ${this.MAX_LEN}`); // MAX_LEN: 5
    console.log(`MSG: ${this.MSG}`); // MSG: hello
  }
}

new Foo().log();
```



# 5. static 키워드

**Typescript 클래스에서 static 키워드는 멤버 변수에도 사용할 수 있다.**	

```typescript
class Foo {
  static instanceCounter = 0;
  constructor() {
    // 생성자가 호출될 때마다 카운터를 1씩 증가시킨다.
    Foo.instanceCounter++;
  }
}

var foo1 = new Foo();
var foo2 = new Foo();

console.log(Foo.instanceCounter);  // 2
console.log(foo2.instanceCounter); // error
```



# 6. 추상 클래스 (Abstract class)

추상 메소드(Abstract method)를 포함할 수 있는 클래스로서 **직접 인스턴스를 생성할 수 없으며 상속만을 위해 사용**된다. 추상 클래스를 **상속하는 클래스는 추상 클래스의 추상 메소드를 반드시 구현**하여야 한다.



```typescript
abstract class Animal {
    
  // 추상 메소드 : 형태만 만들어놨다.
  abstract makeSound(): void;
    
  // 일반 메소드
  move(): void {
    console.log('roaming the earth...');
  }
    
}

// new Animal(); new키워드 못쓴다.
// error TS2511: Cannot create an instance of the abstract class 'Animal'.

class Dog extends Animal {
    
  // 추상 클래스의 추상 메소드를 반드시 구현해야함
  makeSound() {
    console.log('bowwow~~');
  }
    
}

const myDog = new Dog();
myDog.makeSound();
myDog.move();
```

