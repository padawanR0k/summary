## 동시성 프로그래밍 - 1

### Promise
- 비동기 상황을 일급값으로 다룰 수 있다는것이 Promise의 진짜 가치임
- 콜백함수로 작성된 코드는 리턴값이 없음 -> 이 코드는 일회용성임
- Promise로 작성된 코드는 리턴값이 있음 -> 이 코드가 끝났을 때, 연속적으로 다른 행위를 진행할 수 있음

### 모나드
```javascript
const g = a => a + 1;
const f = a => a * a;

console.log(f(g(1))) // 4
console.log(f(g())) // NaN
```
- 제대로 합성되지 않은 경우에는 개발자가 의도하지 않는 결과가 도출된다.
- 제대로 합성되면 이 경우를 막을 수 있다. 이때 배열은 단순히 결과를 담은 상자일 뿐 데이터 구조가 아니다.

```javascript
const g = a => a + 1;
const f = a => a * a;

[1].map(g).map(f).forEach(r => log(r)) // [4]
[].map(g).map(f).forEach(r => console.log(r)) // [] NaN이아님

Promise.resolve(2).then(g).then(g).then(r => console.log(r)) // 4
```

### Kleisli Composition
- 오류가 있을 경우를 대비하는 하나의 방법
```javascript
// f(g(x)) === f(g(x))
// f(g(x)) === g(x) // 오류가 발생한 경우 둘이 동일하게 만드는 것
```
> 뭔 개소린가?

```javascript

const users = [
	{ id: 1, name: 'A' },
	{ id: 2, name: 'B' },
	{ id: 3, name: 'C' },
]

const getUserById = id => find(u => u.id === id, users) || Promise.reject('없음')
const f = ({ name }) => name;
const g = getUserById;
const fg = id => Promise.resolve(id).then(g).then(f)

new Promise((resolve) => {
	fg(2).then(console.log)
	resolve()
}).then(() => {
	users.pop()
	users.pop()
	fg(2).then(console.log) // 둘이 같은 결과를 보여준다.
	g(2).then(console.log) // 둘이 같은 결과를 보여준다.
})
```

### go, pipe, reduce 비동기제어

```javascript
// Promise객체인 경우 then을 통해 값을 가져옴
const go1 = (a, f) => a instanceof Promise ? a.then(f) : f(a);

// reduce는 go, pipe함수에서 사용되고 있으므로 이 부분만 수정하면 됨
const reduce = curry((f, acc, iter) => {
	if (!iter) {
		iter = acc[Symbol.iterator]();
		acc = iter.next().value;
	} else {
		iter = iter[Symbol.iterator]();
	}
	// go1함수를 사용. recur재귀를 즉시 실행시키면서 마이크로 큐 콜스택을 끊음
	return go1(acc, function recur(acc) {
		let cur;
		while (!(cur = iter.next()).done) {
			const a = cur.value;
			acc = f(acc, a);
			// acc가 Promise인 경우 결과를 받아낸 후 리턴함
			if (acc instanceof Promise) return acc.then(recur);
		}
		return acc;
	});
});

go(Promise.resolve(1),
	a => a + 10,
	a => a + 1,
	a => a - 10,
	console.log,
) // 2

go(Promise.resolve(1),
	a => a + 10,
	a => a + 1,
	a => Promise.reject('error'), // 에러가 발생해도 예외처리를 할 수 있어짐
	a => a - 10,
	console.log,
).catch(e => console.log(e)) // 'error'
```
- 백엔드 프로젝트를 할 때 에러핸들러가 next함수에 에러를 전달하여 처리하는것과 비슷한 느낌?

## 동시성 프로그래밍 - 2

```javascript
const go1 = (a, f) => a instanceof Promise ? a.then(f) : f(a);

L.map = curry(function* (f, iter) {
	for (const a of iter) {
		yield go1(a, f); // Promise가 올 경우 처리가능할 수 있도록 함
	}
});

const take = curry((l, iter) => {
	let res = [];
	iter = iter[Symbol.iterator]();
	return function recur() {
		let cur;
		while (!(cur = iter.next()).done) {
			const a = cur.value;
			// Promise인 경우
			if (a instanceof Promise) {
				return a.then(a => {
					// 비동기 처리 후 res에 값 넣음
					res.push(a);
					// 다음 진행을 위해 유명함수를 재귀 호출
					return res.length === l ?  res : recur();
				})
			}
			res.push(a);
			if (res.length == l) return res;
		}
		return res;
	} ()
});

go([Promise.resolve(1), Promise.resolve(2)],
	L.map(a => a + 10),
	take(2),
	console.log)
// [ 11, 12 ]

go([Promise.resolve(1), Promise.resolve(2)],
	map(a => a + 10),
	console.log)
// [ 11, 12 ]
```
- 기존에 사용하던 `L.map`과 `take`를 Promise값도 처리할 수 있도록 바꾸면, 비동기적인 데이터를 받았을 때도 정상적으로 처리할 수 있다.

### Kleisli Composition + 지연성

#### L.filter
```javascript
L.filter = curry(function* (f, iter) {
	for (const a of iter) {
		const b = go1(a, f);
		// 비동기인지 확인
		if (b instanceof Promise) {
			// Promise를 진행시킨후 값이 true면 a를 리턴 아니면 reject,
			// reject시 코드 오류가 아닌, false 라고 알려주는 매개변수 추가
			yield b.then(b => b ? a : Promise.reject('fail'))
		}	else if (b) {
			yield b;
		}
	}
});

const take = curry((l, iter) => {
	let res = [];
	iter = iter[Symbol.iterator]();
	return function recur() {
		let cur;
		while (!(cur = iter.next()).done) {
			const a = cur.value;
			if (a instanceof Promise) {
				return a.then(a => {
					res.push(a);
					return res.length === l ?  res : recur();
				}).catch(e => {
					// catch로 reject를 받고, 오류 매개변수 확인 후 분기처리
					if (e === 'fail') {
						return recur()
					} else {
						return Promise.reject(e)
					}
				})
			}
			res.push(a);
			if (res.length == l) return res;
		}
		return res;
	} ()
});

```

#### L.reduce
```javascript

go([1,2,3,4],
	L.map(a => Promise.resolve(a*a)),
	L.filter(a => Promise.resolve(a % 2)),
	reduce((a, b)=> a+b), // reduce에서 지연성을 지원하게하기
	console.log)


const reduceF = (acc, a, f) => {
	let res;
	if (a instanceof Promise) {
		res = a.then(a => f(acc, a), e => e === 'fail' ? acc : Promise.reject(e))
	} else {
		res = f(acc, a);
	}
	return res
}

// iter에서 1개만 꺼내서 반환한다.
const head = iter => go1(take(1, iter), ([h]) => h);

const reduce = curry((f, acc, iter) => {
	// if (!iter) {
	// 	iter = acc[Symbol.iterator]();
	// 	acc = iter.next().value;
	// } else {
	// 	iter = iter[Symbol.iterator]();
	// }

	// 위 코드를 재귀호출을 통해 간단하게 변경
	if (!iter) {
		return reduce(f, head(iter = acc[Symbol.iterator]()), iter);
	}
	return go1(acc, function recur(acc) {
		let cur;
		while (!(cur = iter.next()).done) {
			// 값이 Promise인지 확인하고 처리하던 부분을 함수(reduceF)로 빼내서 가독성을 향상
			acc = reduceF(acc, cur.value, f);
			if (acc instanceof Promise) return acc.then(recur);
		}
		return acc;
	});
});
```

### 지연된 함수를 병렬적으로!

#### C.reduce
```javascript
const delay500 = a => new Promise(resolve => setTimeout(() => resolve(a), 500))

go([1,2,3,4,5],
	L.map(a => delay500(a * a)),
	L.filter(a => a % 2),
	reduce((a, b) => a + b),
	console.log)
```
- reduce -> L.filter -> L.map 순서로 기다리면서 1개씩 처리됨
	```
	1 -------------> 끝
	2 -------------> 끝
	3 -------------> 끝
	4 -------------> 끝
	5 -------------> 끝
	```

```javascript
C = {}
C.reduce = curry((f, acc, iter) => {
	// 전개연산자를 통해 모두 순회해버린다.
	return iter
		? reduce(f, acc, [...iter])
		: reduce(f, [...acc])
})

console.time('');
go([1,2,3,4,5],
	L.map(a => delay500(a * a)),
	L.filter(a => a % 2),
	C.reduce((a, b) => a + b),
	console.log)
console.timeEnd('');
```
- reduce -> L.filter -> L.map 과정을 한방에 다 실행함
	```
	1 -------------> 끝
	1 -------------> 끝
	1 -------------> 끝
	1 -------------> 끝
	1 -------------> 끝
	```

catch를 미리 처리해줌으로써 에러로그가 뜨지 않게함
```javascript
const catchNoop = (arr) => {
	arr.forEach(a => a instanceof Promise ? a.catch(() => {}) : a)
	return arr;
}
C.reduce = curry((f, acc, iter) => {
	const iter2 = catchNoop(iter ? [...iter] : [...acc]);
	return iter
		? reduce(f, acc, iter2)
		: reduce(f, iter2)
})
```

#### C.take

```javascript
C.take = curry((l, iter) => take(l, catchNoop([...iter])));
```
- `L.*` 순간적인 부하가 보다 적음
- `C.*` 병렬적으로 한번에 실행시키기 때문에 순간적인 부하가 보다 높음

#### C.map, C.filter
```javascript
C.takeAll = C.take(Infinity)
C.map = curry(pipe(L.map, C.takeAll))
C.filter = curry(pipe(L.filter, C.takeAll))
```
지연성이 없이 병렬적으로 실행

```javascript
const delay500 = a => new Promise(resolve => setTimeout(() => resolve(a), 500))

console.time('');
go([1, 2, 3, 4, 5],
	L.map(a => delay500(a * a)),
	L.filter(a => a % 2),
	C.take(2),
	C.reduce((a, b) => a + b),
	console.log)
console.timeEnd('');

console.log('vs')

console.time('');
go([1, 2, 3, 4, 5],
	C.map(a => delay500(a * a)),
	C.filter(a => a % 2),
	C.take(2),
	C.reduce((a, b) => a + b),
	console.log)
console.timeEnd('');


// : 0.794ms 
// vs
// : 0.167ms 
```

## 동시성 프로그래밍 - 3
- Promise는 async await 키워드를 통해 동기적으로 사용가능. then처리가 되지않은 Promise객체는 Promise로 남아있음. 값을 사용하고 싶으면 꼭 `then()`이나 await 키워드를 사용해야함. 최상위에서 async await키워드를 사용하고 싶으면 async 즉시 실행 함수를 사용하면됨