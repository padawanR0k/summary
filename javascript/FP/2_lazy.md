## 제너레이터 활용하여 지연평가하기
제너레이터를 활용하게 되면 실제 값이 필요할 때, 그 값을 만들어낸다. -> 불필요한 리소스를 최소화할 수 있다.

```javascript
const L = {};
L.range = function* (limit) {
	let i = 0;
	while (i < limit) {
		yield i;
		i++;
	}
}

const a = L.range(5);
console.log(a.next()); // { value: 0, done: false}
console.log(a.next()); // { value: 1, done: false}
console.log(a.next()); // { value: 2, done: false}
console.log(a.next()); // { value: 3, done: false}
console.log(a.next()); // { value: 4, done: false}
console.log(a.next()); // { done: true }

const range = (limit) => {
	const arr = []
	let i = 0;
	while (i < limit) {
		arr.push(i);
		i++;
	}
	return arr;
}
console.log(range(5)) // [0,1,2,3,4] 이처럼 모든 값이 즉시 생성됨
```
### 이럴때 유리하다.
어떤 리스트의 최대갯수는 100만개임. 리스트를 순회하면서 N개씩 어떤 작업을 하려면?
- 기존
	- 100만개짜리 리스트부터 만들고 순회하며 처리
- 제너레이터
	- N개씩 순회하면서 리스트를 만들고 처리
```javascript
const list1 = range(1000000) // 100만개 만들어짐
const list2 = L.range(1000000) // 0개만들어짐

const take = (limit, iter) => {
	const res = []
	for(const a of iter) {
		res.push(a);
		if (res.length === limit) {
			return res
		}
	}
	return res;
}
take(5, list1);
take(5, list2); // take하면서 리스트에 실제값을 생성
```

```javascript
// take함수에 커링을 더하면
const take = curry((limit, iter) => {
	const res = []
	for(const a of iter) {
		res.push(a);
		if (res.length === limit) {
			return res
		}
	}
	return res;
})

// go함수에도 사용할 수 있다. 이런것도 가능
go(
	L.range(100000),
	take(5),
	reduce((a, b) => a + b),
	console.log
)
```

### L.map, L.filter
Array.map, Array.filter랑 비슷하나 지연평가가능

```javascript
const L = {}
L.map = function *(f, iter) {
	for (const a of iter) yield f(a);
}

L.filter = function *(f, iter) {
	for (const a of iter) if (f(a)) yield a;
}
```

### 지연평가 활용 1
함수형 프로그래밍으로 작성한 코드의 특징은 지금 해야할 액션의 대상을 미리 전부 만들어 놓지 않는다는 것이다. 그래서 실행순서가 수평적이지 않고 수직적이다. 아래 코드를 보자.
```javascript
const reduce = (f, acc, iter) => {
	if (!iter) {
		iter = acc[Symbol.iterator]();
		acc = iter.next().value;
	}
	while (!((cur = iter.next()) && cur.done)) {
		acc = f(acc, cur.value);
	}
	return acc;
}

const go = (...args) => {
	return reduce((acc, f) => f(acc), args)
}

const L = {};
const curry = (f) => (a, ...args) => args.length ? f(a, ...args) : (...args) => f(a, ...args);

L.range = function* (limit) {
	let i = 0;
	while (i < limit) {
		yield i;
		i++;
	}
};

L.map = curry(function* (f, iter) {
	while (!((cur = iter.next()) && cur.done)) {
		yield f(cur.value)
	}
})

L.filter = curry(function* (f, iter) {
	iter = iter[Symbol.iterator]();
	let cur;
	while (!((cur = iter.next()).done)) {
		if (f(cur.value)) yield cur.value
	}
})

const take = curry((limit, iter) => {
	const res = []
	iter = iter[Symbol.iterator]();
	let cur;
	while (!((cur = iter.next()).done)) {
		res.push(cur.value)
		if (res.length === limit) {
			return res;
		}
	}
	return res;
})


go(L.range(5),
	L.filter(a => a % 2 === 1),
	L.map(a => a * 2),
	take(5),
	console.log
);
```
- 일반적인 프로그래밍의 경우 실행 순서
	1. `[0, 1, 2, 3, 4]` 생성
	2. 홀수만 필터링
	3. 반환
- 함수형 프로그래밍의 경우 실행 순서
	1. 범위 지정
	2. 몇개를 사용할 건지 설정 `take(5)`
	3. 값을 하나씩 만듬 `0`
	4. 1개에 값에 대해 필터링진행
	5. 성공시 map에 적용한 함수 적용
	6. 모든 범위를 순회하면서 2~5 반복

> 함수형 프로그래밍같은 경우는 N-Queen문제를 풀때 사용했었던 알고리즘인 백트래킹같은 느낌으로 흘러간다.

이터레이터의 성질을 이용한 지연성의 장점과 이터러블 프로토콜만 구현하면 순회할 수 있는 장점을 섞어서 활용하게되면 코드의 효율성도 좋아지고 코드의 합성도 쉬워지는 장점이 있다!


### 지연 평가 활용 2
```javascript
const objToQuery = pipe(
	Object.entries,
	map(([k, v]) => `${k}=${v}`),
	reduce((a, b) => `${a}&${b}`),
	console.log
)
console.log(objToQuery({ limit: 10, offset: 10 })); // limit=10&offset=10 

const objToQuery2 = pipe(
	Object.entries,
	L.map(([k, v]) => `${k}=${v}`),
	join('&'),
)
console.log(objToQuery2({ limit: 10, offset: 10 })); // limit=10&offset=10 


L.entries = function *(obj) {
	for (const k in obj) yield [k, obj[k]];
}

const objToQuery3 = pipe(
	L.entries,
	L.map(([k, v]) => `${k}=${v}`),
	join('&'),
)
console.log(objToQuery3({ limit: 10, offset: 10 })); // limit=10&offset=10 
```
1. 객체를 쿼리스트링으로 만들기
2. map부분을 지연평가로 대체
3. `Object.entries`메서드를 지연평가로 대체

#### take, find
```javascript
L.filter = curry(function* (f, iter) {
	iter = iter[Symbol.iterator]();
	let cur;
	while (!((cur = iter.next()).done)) {
		if (f(cur.value)) yield cur.value
	}
})

const users = [
	{age: 32},
	{age: 31},
	{age: 33},
	{age: 34},
]

const find = curry((f, iter) => go(
	iter,
	L.filter(f),
	take(1),
	([a]) => a
));

console.log(find(u => u.age === 31, users)); // { age: 31 }
```
지연평가를 통해 모든 배열을 순회하지 않음

#### L.flatten, L.flatMap

```javascript
const isIterable = a => a && a[Symbol.iterator];
L.flatten = function* f(iter) {
	for (const a of iter) {
		if (isIterable(a)) {
			yield* L.flatten(a)
		} else {
			yield a;
		}
	}
}

const ex = L.flatten([1, 2, 3, [1, 2, 3, [1, 2, 3]]])
const takeAll = take(Infinity)
const flatten = pipe(L.flatten, takeAll)
console.log(
	flatten([1, 2, 3, [4]])
);
```
- depth상관없이 배열의 깊이를 평탄화하기

#### 2차원 배열, depth 다루기
```javascript
const arr2 = [
	[1,2],
	[3,4],
	[5,6],
	[7,8],
]

go(arr,
	L.flatten,
	L.filter(a => a%2),
	take(3),
	console.log
)
```
굳이 1부터8까지 전부 순회하지 않는다.

```javascript
const arr3 = [
	{name: 'a', age: 21, family: [
		{ name: 'a1', age: 11 },
		{ name: 'a2', age: 21 },
	]},
	{name: 'b', age: 21, family: [
		{ name: 'b1', age: 21 },
		{ name: 'b2', age: 2 },
	]},
	{name: 'c', age: 21, family: [
		{ name: 'c1', age: 21 },
		{ name: 'c2', age: 31 },
	]},
	{name: 'd', age: 21, family: [
		{ name: 'd1', age: 41 },
		{ name: 'd2', age: 21 },
	]},
]

go(arr3,
	L.map(a => a.family),
	L.flatten,
	L.filter(a => a.age < 20),
	L.map(a => a.name),
	takeAll,
	console.log
)
```
- 2차원 배열이나, depth가 있는 객체를 순회하는 것이나 코드가 크게 다르지 않다는걸 느낄 수 있다.
- 데이터가 우선시되는 객체지향 프로그래밍과 다르게 함수지향 프로그래밍은 함수가 하는 일이 우선시된다. 함수의 조합으로 내가 원하는 구조를 이끌어낸다.


