정적타입언어는 함수 또는 클래스를 선언하는 시점에 매개변수나 반환값의 타입을 명시하여야 한다.

 FIFO(First In First Out) 구조로 데이터를 저장하는 큐를 표현한 것이다.

```typescript
class Queue {
  protected data = []; // data의 타입이 any[]가 되었다.

  push(item) {
    this.data.push(item);
  }

  pop() {
    return this.data.shift();
  }
}

const queue = new Queue();

queue.push(0);
queue.push('1'); // 의도하지 않은 실수  제네릭을 사용하여 이 문제를 해결하여 보자.

console.log(queue.pop().toFixed()); // 0
console.log(queue.pop().toFixed()); // Runtime error
```



```typescript
class Queue<T> { // 선언 시점에는 타입이 뭔지 모른다.
  protected data = [];
  push(item: T) {
    this.data.push(item);
  }
  pop(): T {
    return this.data.shift();
  }
}

// number 전용 Queue
const numberQueue = new Queue<number>(); // 호출되면서 generic을 number로 줌.

numberQueue.push(0);
// numberQueue.push('1'); // 의도하지 않은 실수를 사전 검출 가능
numberQueue.push(+'1');   // 실수를 사전 인지하고 수정할 수 있다

console.log(numberQueue.pop().toFixed()); // 0
console.log(numberQueue.pop().toFixed()); // 1

// string 전용 Queue
const stringQueue = new Queue<string>();

stringQueue.push('Hello');
stringQueue.push('World');

console.log(stringQueue.pop().toUpperCase()); // HELLO
console.log(stringQueue.pop().toUpperCase()); // WORLD

// 커스텀 객체 전용 Queue
const myQueue = new Queue<{name: string, age: number}>();
myQueue.push({name: 'Lee', age: 10});
myQueue.push({name: 'Kim', age: 20});

console.log(myQueue.pop()); // { name: 'Lee', age: 10 }
console.log(myQueue.pop()); // { name: 'Kim', age: 20 }
```

**제네릭**은 선언 시점이 아니라 **생성 시점에 타입을 명시**하여 하나의 타입만이 아닌 **다양한 타입을 사용**할 수 있도록 하는 기법이다. **한번의 선언**으로 다양한 타입에 **재사용이 가능하다는 장점**이 있다.

**T는 제네릭을 선언할 때 관용적으로 사용되는 식별자로 타입 파라미터(Type parameter)라 한다.**



```typescript
function reverse<T>(items: T[]): T[] {
  return items.reverse();
}
			// 매개변수가 들어 가면서 타입추론에 의해 타입이 결정된다.
const arg = [{name: 'Lee'}, {name: 'Kim'}, {name: 'Park'}];

// 인수에 의해 타입매개변수가 결정된다
const reversed = reverse(arg); // reversed: {name: string}[]
console.log(reversed);

reversed.push({name: 100}); // Error
console.log(reversed);
```

