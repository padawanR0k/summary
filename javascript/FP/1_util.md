## 이터러블 프로토콜을 따른 map함수의 다형성
```typescript
const map = (f, iter) => {
	const arr = [];
	for (let a of iter) {
		arr.push(f(a))
	}
	return arr;
}

document.querySelectorAll("*").map() // 에러 발생 (해당 값은 NodeList를 상속받았으므로 map메소드를 가지고 있지 않음)
map(document.querySelectorAll("*")) // 🙂

```
- reduce 만들기
```javascript
const reduce = (f, acc, iter) => {
	if (!iter) {
		iter = acc[Symbol.iterator]();
		acc = iter.next().value;
	}
	for (let a of iter) {
		acc = f(acc, a);
	}
	return acc;
}

console.log(reduce((acc, curr) => acc + curr, [1, 2, 3]));
```

- filter 만들기
```javascript
const filter = (f, iter) => {
	const arr = []
	const _iter = iter[Symbol.iterator]();
	for (let a of _iter) {
		if (f(a)) {
			arr.push(a)
		}
	}
	return arr;
}

console.log(filter(a => a%2, [1,2,3,4]));
```

- 함수형 프로그래밍을 사용하여 코드를 작성할 때 이터러블을 받는 중첩된 함수들을 사용하게되면, 다음 단계의 함수를 임시 데이터로 작성후 코드를 작성하는 것이 가능하다.
```javascript
const arr = [1,2,3,4];
const sumEven = reduce((acc, curr) => acc+curr, [2,4]) // 처음엔 이렇게 작성
const sumEven = reduce((acc, curr) => acc+curr, filter(a => a % 2 === 0, arr)) // 이후에 이렇게 작성
```

## 함수 조합을 위한 go, pipe
```javascript
const go = (...args) => {
	return reduce((acc, f) => f(acc), args)
}
go(
	1,
	a => a + 2,
	a => a + 3,
	console.log
)
// 일반적인 pipe
const pipe = (...fs) => (a) => go(a, ...fs);
// 익명함수를 받을 수 있는 pipe
const pipe = (f, ...fs) => (...as) => go(f(...as), ...fs);
```

## 커링
- 함수가 여러 매개변수를 받는 경우, 커링을 하면 매개변수를 여러번 나눠 받아 사용할 수 있다.
```javascript
const curry = (f) =>
	(a, ...args)
		=> args.length
			? f(a, ...args)
			: (...args) => f(a, ...args);
const add = curry((first, second) => first + second);
console.log(add(1)) // Function
console.log(add(1)(2)) // 3
console.log(add(1, 2)) // 3


(a, ...args)
	=> args.length
		? (a, ...args) => a+second
		: (...args) => (a, ...args) => a+second;


const sum = (f, iter) => go(
	iter,
	map(f),
	reduce((a, b) => a + b))
const products = [...]
const getSum = sum(({price}) => price)
const totalPrice = getSum(products) // sum함수의 리턴값은 함수가 평가된 값이아니라 첫번째 매개변수만 입력되어있는 함수다. 그걸 다시 실행하면서 실제 값을 전달함.
```

구조를 조금만 더 바꾸면 이런것도 가능
```javascript

const take = (limit, iter) => {
	const res = []
	if (Array.isArray(iter)) {
		for (const a of iter) {
			res.push(a);
			if (res.length === limit) {
				return res
			}
		}
	} else {
		while (true) {
			if (res.length < limit) {
				const next = iter.next();
				if (next) {
					res.push(next.value)
				} else {
					return res;
				}
			} else {
				return res;
			}
		}
	}
	return res;
}
console.log(take(5, list1));
console.log(take(5, list2)); // [ 0, 1, 2, 3, 4 ]
console.log(take(5, list2)); // [ 5, 6, 7, 8, 9 ]
```