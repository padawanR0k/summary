## 일급 함수
- 함수를 값으로 다룰 수 있음
- 함수를 변수에 담을 수 있음
- 함수를 인자로 사용할 수 있음
- 함수를 함수의 리턴값으로 사용할 수 있음

## 기본적인 함수 종류
- 일급 함수
	- 함수가 값으로서 다뤄짐
- 고차 함수
	- 함수를 값으로 다루는 함수
- 함수를 인자로 받아서 실행하는 함수
- 함수를 리턴하는 함수 (클로져 활용)

## 순회와 이터러블
- for문으로 순회가능하지만 인덱스로 접근못함
	- Set, Map
- 이는 `Symbole.iterator()` 메서드를 통해 하는것임

### 이터러블/이터레이터 프로토콜
- 이터러블한 값
	- `Symbole.iterator()`메소드를 가진 객체
- 이터레이터
	- `{ value: any, done: boolean }` 객체를 리턴한느 `next()`메소드를 가진 값
- 위 두가지를 만족하면 이터레이터 프로토콜을 만족한것임

```typescript
const arr = [1,2,3];
const iter1 = arr[Symbol.iterator]();

// for문이 사용하는 것은 사실 [Symbol.iterator]()의 반환값이였음!
// Map, Set, Array 동일
for (const a of iter1) {
	console.log(a)
}
// 1
// 2
// 3


// 반환된 이터레이터가 얼마나 진행되었는지는 이후에 생성되는 이터레이터한테는 상관이 없다.
const arr = [1,2,3];
const iter1 = arr[Symbol.iterator]();
console.log(iter1.next()) // {value: 1, done: false}
const iter2 = arr[Symbol.iterator]();
console.log(iter2.next()) // {value: 1, done: false}
```

- 직접 이터러블한 객체만들기
```typescript
const iterable = {
	[Symbol.iterator]() {
		let i = 3;
		return {
			next() {
				return i === 0
					? { done: true }
					: { done: false, value: i-- };
			}
		}
	}
}
const iter = iterable[Symbol.iterator]();
iter.next() // { done: false, value: 3}
iter.next() // { done: false, value: 2}
iter.next() // { done: false, value: 1}

for (const a of iter) {
	// Uncaught TypeError: iter is not iterable
}
```

well-formed 이터레이터가 되기 위해서는 추가적인 작업이 필요하다. (iterable[Symbol.iterator])

```typescript
const iterable = {
	[Symbol.iterator]() {
		let i = 3;
		return {
			next() {
				return i === 0
					? { done: true }
					: { done: false, value: i-- };
			},
			[Symbol.iterator]() {
				return this;
			}
		}
	},
}
const iter = iterable[Symbol.iterator]()
for (const a of iter) {
	// :)
}
```
- 위처럼 well-formed 이터레이터는 [스프레드 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)도 지원한다.

## 제너레이터
- 이터레이터이자 이터러블을 생성하는 함수 === well-formed 이터레이터
- 이터러블하기 때문에 for of, 스프레드 연산자, 구조 분해 할당, 전개 연산자 모두 사용가능

```typescript
function *example() {
	yield 1;
	yield 2;
	yield 3;
}
const iter = example();
console.log(iter === iter[Symbol.iterator]()) // true
console.log(iter.next()); // { done: false, value 1 }
console.log(...iter) // 2, 3

```
