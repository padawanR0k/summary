**디스트럭처링**(Destructuring)은 구조화된 객체(배열 또는 객체)를 *Destructuring(비구조화, 파괴)하여* 개별적인 변수에 할당하는 것이다. 





# 1. 배열 디스트럭처링 (Array destructuring)

```js
// ES5
var arr = [1, 2, 3];

var one   = arr[0];
var two   = arr[1];
var three = arr[2];

console.log(one, two, three); // 1 2 3


// ES6 Destructuring
const arr = [1, 2, 3];

// 인덱스를 기준으로 배열로부터 요소를 추출하여 변수에 할당
const [one, two, three] = arr; 
// 이것과 같음 var one = 1, var two = 2, var three = 3

console.log(one, two, three); // 1 2 3
```





배열 디스트럭처링은 배열에서 필요한 요소만 추출하여 변수에 할당하고 싶은 경우에 유용하다.

```js
let x, y, z;
[x, y] = [1, 2, 3];
console.log(x, y); // 1 2

[x, , z] = [1, 2, 3];
console.log(x, z); // 1 3

// default value
[x, y, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

[x, y = 10, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

// spread operator
[x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [ 2, 3 ]

// 필요한 부분만 뽑아쓰기
const arr = [1, 2, 3, 4];
const [one, , three] = arr;

console.log(one, three); // 1 3

```



# 2. 객체 디스트럭처링 (Object destructuring)

객체 디스트럭처링은 객체의 각 프로퍼티를 객체로부터 추출하여 변수 리스트에 할당한다. 이때 할당 기준은 **프로퍼티 이름(키)**이다.



```js
// ES5
var obj = { firstName: 'Ungmo', lastName: 'Lee' };
var name = {};

name.firstName = obj.firstName;
name.lastName  = obj.lastName;

console.log(name); // { firstName: 'Ungmo', lastName: 'Lee' }


// ES6 Destructuring 프로퍼티명을 기준으로 
const obj = { firstName: 'Ungmo', lastName: 'Lee' };

const { firstName, lastName } = obj;

console.log(firstName, lastName); // Ungmo Lee
```



응용하여 써보자

```js
function margin() {
  const left = 1, right = 2, top = 3, bottom = 4;
  return { left, right, top, bottom }; // 변수를 객체화해서 리턴함.
}
const { left, bottom } = margin(); // margin()에서 리턴된 객체의 프로퍼티명을 기준으로 값을 가져옴
console.log(left, bottom); // 1 4 
```

