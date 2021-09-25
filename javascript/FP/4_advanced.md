## 응용편
- [fxjs](https://github.com/marpple/FxJS)
- [깃허브주소](https://github.com/indongyoo/functional-javascript-02)

#### _.each
특정 효과(이펙트)를 일으키는 부분이라는것을 명시적으로 보여주는 함수. `Array.forEach`마냥 리턴값이 없는듯함
```javascript
_.go(
	L.range(10),
	_.each(console.log)
)
console.log('--');
_.go(
	L.range(10),
	L.map(console.log),
	_.takeAll
)

_.go(
	_.range(10),
	_.each(a => a*2),
	_.each(console.log)
)
// 1
// 2
..
// 9
```

#### _.go를 여러번 사용하기
별찍어보기
```javascript
const join = (sep) => _.reduce((a,b) => `${a}${sep}${b}`)
_.go(
	_.range(1, 6),
	_.map(_.range),
	_.map(_.map(a => "*")),
	_.map(join('')),
	join('\n'),
	console.log
)
// *
// **
// ***
// ****
// *****

const join = (sep) => _.reduce((a,b) => `${a}${sep}${b}`)
_.go(
	_.range(1, 10, 2),
	_.map(_.range),
	_.map(_.map(a => "*")),
	_.map(join('')),
	_.map((a) => _.go(
		_.range(5-(a.length/2)),
		_.map(_ => ''),
		join(' ')
	) + a),
	join('\n'),
	console.log
)
// 		 *
//    ***
//   *****
//  *******
// *********
```

#### reduce하나보단 map, filter, reduce를 섞어쓰자
```javascript
_.reduce((total, u) => total + u.age, 0, users));

const add = (a, b) => a+b;
const ages = L.map(u => u.age);
_.reduce(add, ages(users));
```
코드의 길이는 더 길어졌으나, 코드를 더 재사용하기 쉽도록 분리하면서 가독성은 조금더 나아졌다.

```javascript
const split = _.curry((sep, str) => str.split(sep));
const queryToObject = _.pipe(
split('&'),
_.map(split('=')),
_.map(([k,v])=>({[k]:v})),
_.reduce(Object.assign)
)

console.log(queryToObject('a=1&c=CC&d=DD')); /// {a: "1" c: "CC" d: "DD"}
```
#### 함수 합성하기
```javascript
const f = x => x + 10;
const g = x => x - 5;
const fg = x => f(g(x));
_.go(
	10,
	fg,
	console.log
)

_.go(
	[10, null],
	L.map(fg),
	_.each(console.log)
)
```
모나드를 이용한 합성함수를 안전하게 사용하기 위해 배열을 데이터 담는 도구로 사용.

```javascript
const users = [
	{name: 'a', age: 11},
	{name: 'b', age: 21},
	{name: 'c', age: 31},
];

// _.find는 못찾으면 undefined리턴

const getUser1 = (name) => {
	return _.find(u => u.name === name, users);
}

const getUser2 = (name) => {
	return _.go(users,
		L.filter(a => a.name === name),
		L.take(1),
		L.map(a => a.age),
		_.each(console.log))
}

var user = getUser1('z');
console.log(user);
if (user) {
	console.log(user.age)
}
getUser2('z')
```

- 함수형으로 작성된 경우, 진행하려는 행위를 함수합성내부에 넣게된다.
- 앞서 작성한 부분들에 대해서 만족하지 못해 필터로 값이 걸려진 경우에는 그 뒤에있는 "행위"가 실행되지 않는다.
- 그러므로 if같은 부분이 추가적으로 필요하지 않게된다.

### 객체를 이터러블로 다루기
객체들 또한 배열처럼 이터러블로 다룰 수 있다.

#### L.values, L.entries
```javascript
L.values = function *(obj) {
	for (const k in obj) yield obj[k];
}

_.go(
	obj1,
	// Object.values,
	L.values,
	L.each(console.log)
)
```
`Object.values()`대신 이터러블 프로토콜을 지원하게끔하여 지연평가도 할 수 있어 더 효율적이다.

```javascript
L.entires = function *(obj) {
	for (const k in obj) yield [k, obj[k]];
}

const obj = {
	a: 1,
	b: 2,
	c: 3,
}

_.go(
	obj,
	L.entries,
	L.filter(([k,v]) => v%2),
	L.map(([k,v]) => ({[v]: k})),
	_.reduce(Object.assign),
	console.log
)
```

#### 어떠한 값이든 이터러블로 다루기
- 이터러블을 이터러블로
- 객체를 이터러블로
- 제너레이터를 이터러블로...


```javascript
const object = entries =>
	_.reduce((obj, [k,v]) => (obj[k]=v, obj), {}, entries);
console.log(object(L.entries({b:2, c:3})));

let m = new Map();
m.set('a', 1);
m.set('b', 2);
m.set('c', 3);
console.log(object(m));
```
일반객체, Map같은 특수하지만 이터레이터를 지원하는 개체들을 모두 `object`함수로 다룰 수 있다.

#### 객체 순회하기
```javascript
const object = entries =>
	_.reduce((obj, [k,v]) => (obj[k]=v, obj), {}, entries);
const mapObject = (f, obj) =>
	_.go(
		obj,
		L.entries,
		_.map(([k, v]) => [k, f(v)]),
		object,
	);
console.log(mapObject(a => a + 30, {a: 1, b: 2, c: 3}));
```

#### 객체 필터
```javascript
const obj = {a: 1, b: 2, c: 3};
const pick = (keys, obj) => _.go(
	obj,
	_.entries,
	_.filter(([k]) => keys.includes(k)),
	_.object
)
console.log(pick(['a'], obj)); // { "a": 1 }
```

#### pk를 가진 배열을 객체로
```javascript
const users = [
	{id: 1, name: 'a', age: 11},
	{id: 2, name: 'b', age: 21},
	{id: 3, name: 'c', age: 31},
	{id: 4, name: 'd', age: 41},
];
const indexBy = (f, iter) =>
	_.go(
		_.reduce((obj, a) => (obj[f(a)] = a, obj), {}, iter)
	)

console.log(indexBy(u => u.id, users)) // {"1":{"id":1,"name":"a","age":11},"2":{"id":2,"name":"b","age":21},"3":{"id":3,"name":"c","age":31},"4":{"id":4,"name":"d","age":41}}
```
객체로 바꾸게되면 원하는 값을 가져올 때, O(1)번만 조회하면 된다는 이점이 있다.

```javascript
const users = [
	{id: 1, name: 'a', age: 11},
	{id: 2, name: 'b', age: 21},
	{id: 3, name: 'c', age: 31},
	{id: 4, name: 'd', age: 41},
];
const indexByFilter = (f, iter) =>
	_.go(
		iter,
		L.entries,
		_.filter(([k,v]) => f(v)),
		_.object,
		// console.log
	)
const indexed = _.indexBy(u => u.id, users);
console.log(indexByFilter(a => a.age > 20, indexed))
```
인덱스된 객체를 필터링하기