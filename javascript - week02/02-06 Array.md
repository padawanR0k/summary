#1. 배열의 생성

배열(array)는 1개의 변수에 여러 개의 값을 순차적으로 저장할 때 사용한다.



---



##1.1 배열 리터럴

0개 이상의 값을 쉼표로 구분하여 대괄호([])로 묶는다. 첫번째 값은 인덱스 ‘0’으로 읽을 수 있다. 존재하지 않는 요소에 접근하면 `undefined`를 반환한다.

```js
var emptyArr = [];

var arr = [
  'zero', 'one', 'two', 'three', 'four',
  'five', 'six', 'seven', 'eight', 'nine'
];

console.log(emptyArr[1]); // undefined
console.log(arr[1]);      // 'one'

// 객체 리터럴로 유사하게 표현하면 이렇게 된다.
var obj = {
  '0': 'zero',  '1': 'one',   '2': 'two',
  '3': 'three', '4': 'four',  '5': 'five',
  '6': 'six',   '7': 'seven', '8': 'eight',
  '9': 'nine'
};
```

두 객체의 근본적 차이는 배열 리터럴 `arr`의 프로토타입 객체는 `Array.prototype`이지만 객체 리터럴 `obj`의 프로토타입 객체는 `Object.prototype`이라는 것이다. `Array` 객체는 다양한 메소드(e.g. `sort`)와 프로퍼티(e.g. `length`)를 제공한다.

![object prototype & array prototype](http://poiemaweb.com/img/object_array_prototype.png)



---



##1.2 Array() 생성자 함수

배열은 일반적으로 배열 리터럴 방식으로 생성하지만 배열 리터럴 방식도 *결국 내장 함수 Array() 생성자 함수로 배열을 생성하는 것을 단순화시킨 것*이다. 

```js
var arr = new Array(2);
console.log(arr.length, arr); // 2 [undefined, undefined]
```



***



#2. 배열 요소의 추가와 삭제



##2.1 배열 요소의 추가

객체가 동적으로 프로퍼티를 추가할 수 있는 것처럼 배열도 동적으로 요소를 추가할 수 있다. 

```js
var arr = [];
console.log(arr[0]); // undefined

arr[0] = 'one';
arr[3] = 'three';
arr[7] = 'seven';
// 이렇게  뛰어넘으면서 할당하면 그 중간의 값들은 undefined가 된다.

console.log(arr); // ["one", undefined ,undefined, "three", undefined, undefined,  undefined, "seven"]
```



---





##2.2 배열 요소의 삭제

배열은 객체이기 때문에 배열의 요소를 삭제하기 위해 `delete` 연산자를 사용할 수 있다. 이때 해당 요소가 삭제되는 것이 아니라 요소 값이 삭제되어 undefined가 된다.

```js
var numbersArr = ['zero', 'one', 'two', 'three'];

// 요소의 값만 삭제된다. 이거 쓰지말고
delete numbersArr[2]; // ['zero', 'one', undefined, 'three']
console.log(numbersArr);

// 요소 일부를 삭제 (시작 인덱스, 삭제할 요소수) 이거 쓰자
numbersArr.splice(2, 1); // ['zero', 'one', 'three']
console.log(numbersArr);
```



***



#3. 배열 요소의 열거

객체의 프로퍼티를 열거할 때 for in 문을 사용한다. 배열 역시 객체이므로 for in 문을 사용할 수 있다

그러나 배열은 객체이기 때문에 프로퍼티를 가질 수 있다. for in 문을 사용하면 *불필요한 프로퍼티까지 출력될 수 있고 요소들의 순서를 보장하지 않으므로* 배열을 열거하는데 적합하지 않다.

***



#4. Array Property



##4.1 Array.length

length 프로퍼티는 요소의 갯수(배열의 길이)를 나타낸다. Array.length는 양의 정수이며 2^32미만이다.

```js
var arr = [
  'zero', 'one', 'two', 'three', 'four',
  'five', 'six', 'seven', 'eight', 'nine'
];

// 배열 길이의 명시적 설정
arr.length = 3; // [ 'zero', 'one', 'two' ]  length를 줄이면 그 다음 값들은 삭제된다.

// 배열 끝에 새 요소 추가
arr[arr.length] = 'san'; // [ 'zero', 'one', 'two', 'san' ]

arr.length = 5; // [ 'zero', 'one', 'two', 'san', undefined ]

// 배열 끝에 새 요소 추가
arr.push('go'); // [ 'zero', 'one', 'two', 'san', undefined, 'go' ]
```

Array.prototype.push() 메소드는 매개변수로 전달된 값들을 배열의 마지막에 추가한다. 

length-1 에다가 값을 할당하는것과 같다.

#5. Array Method

- Array.isArray()

  - 객체가 배열이면 true, 배열이 아니면 false를 반환한다.

  - ```js
    // true
    Array.isArray([]);
    Array.isArray([1, 2]);
    Array.isArray(new Array());

    // false
    Array.isArray();
    Array.isArray({});
    Array.isArray(null);
    Array.isArray(undefined);
    Array.isArray(1);
    Array.isArray('Array');
    Array.isArray(true);
    Array.isArray(false);
    ```

  ​

- Array.prototype.indexOf()

  - indexOf 메소드의 인자로 지정된 요소를 배열에서 검색하여 인덱스를 반환한다. 

  - ```js
    var arr = [1, 2, 2, 3];
    arr.indexOf(2); // 1
    arr.indexOf(4); // -1 해당요소가 없어서 C언어처럼 -1을 반환했음
    ```

  ​

- Array.prototype.concat(item…)

  - concat 메소드의 인수로 넘어온 값들(배열 또는 값)을 자신의 복사본에 요소로 추가하고 반환한다. 이때 **원본 배열은 변경되지 않는다.**

    ```js
    var a = ['a', 'b', 'c'];
    var b = ['x', 'y', 'z'];

    var c = a.concat(b);
    console.log(c); // ['a', 'b', 'c', 'x', 'y', 'z']

    var d = a.concat('String'); // 값을 주면 그 값이 요소가 된다.
    console.log(d); // ['a', 'b', 'c', 'String']

    var e = a.concat(b, true); // Data Type이 맞지않아도 추가된다....
    console.log(e); // ['a', 'b', 'c', 'x', 'y', 'z', true]

    // 원본 배열은 변하지 않는다.
    console.log(a); // [ 'a', 'b', 'c' ]
    ```

  ​

- Array.prototype.join()

  - 배열 요소 전체를 연결하여 생성한 문자열을 반환한다. 기본구분자는 `,`이다.

  - ```js
    str = arr.join([기본구분자 = ','])

    var arr = ['a', 'b', 'c', 'd'];

    var x = arr.join();
    console.log(x);  // 'a,b,c,d'

    var y = arr.join(''); // 빈문자열을 주면 ,를 빼고 반환한다.
    console.log(y);  // 'abcd' 

    var z = arr.join(':'); 
    console.log(z);  // 'a:b:c:d' 
    ```

  ​

- Array.prototype.pop()

  - `pop`메서드는 배열의마지막 요소를 제거하고 그 요소를 반환한다.

  - `pop`은 `push`와 함께 배열을 스택처럼 동작하게 한다.`

  - ```js
    var a = ['a', 'b', 'c'];
    var c = a.pop();

    // 원본 배열이 변경된다.
    console.log(a); // a --> ['a', 'b']
    console.log(c); // c --> 'c'
    ```

- Array.prototype.push()

  - 인자로 전달된 항목을 배열의 끝에 추가한다. `concat` 메소드와 다르게 인수에 넘어온 배열자체를 배열에 추가한다. 반환값은 배열의 새로운 `length` 값이다.

  - ```js
    var a = ['a', 'b', 'c'];
    var b = ['x', 'y', 'z'];

    // push는 원본 배열을 직접 변경하고 변경된 배열의 length를 반환한다.
    var c = a.push(b);
    console.log(a); // a --> ['a', 'b', 'c', ['x', 'y', 'z']]
    console.log(c); // c --> 4;

    // concat은 원본 배열을 직접 변경하지 않고 복사본을 반환한다.
    console.log([1, 2].concat([3, 4])); // [ 1, 2, 3, 4 ]
    ```

- Array.prototype.reverse()

  - 배열요소의 순서를 거꾸로 변경한다. 반환값은 변경된 배열이다.

  - ```js
    var a = ['a', 'b', 'c'];
    var b = a.reverse();

    // 원본 배열이 변경된다
    console.log(a); // [ 'c', 'b', 'a' ]
    console.log(b); // [ 'c', 'b', 'a' ]
    ```

- Array.prototype.shift()

  - 배열에서 첫요소를 제거하고 제거한 요소를 반환한다. 만약 빈 배열일 경우 `undefined`를 반환한다.

  - ```js
    var a = ['a', 'b', 'c'];
    var c = a.shift();

    // 원본 배열이 변경된다.
    console.log(a); // a --> [ 'b', 'c' ]
    console.log(c); // c --> 'a'
    ```

- Array.prototype.slice(start,end)

  - 배열의 특정 부분에 대한 복사본을 생성한다.

    첫번째 매개변수 start에 해당하는 인덱스를 갖는 요소부터 매개변수 end에 **해당하는 인덱스를 가진 *요소 전까지*** 복사된다.

  - end 매개변수의 기본값은 length이다.

  - ```js
    var items = ['a', 'b', 'c'];// 원본은 변경돼지 않는다.

    // items[0]부터 items[1] 이전(items[1] 미포함)까지 반환
    var res1 = items.slice(0, 1);
    console.log(res1);  // [ 'a' ]
    // items[1]부터 이후의 모든 요소 반환
    var res3 = items.slice(1);
    console.log(res3);  // [ 'b', 'c' ]

    ```

- Array.prototype.splice(start, deleteCount, item…)

  - 기존의 배열의 요소를 제거하고 그 위치에 새로운 요소를 추가한다. 배열 중간에 새로운 요소를 추가할 때도 사용된다.

    - item매개변수는 삭제한 위치에 추가될 요소들을 넣으며 , 옵션값qsaaasd															이다.

  - ```js
    var items = ['one', 'two', 'three', 'four'];

    // items[1]부터 2개의 요소를 제거하고 제거된 요소를 배열로 반환
    var res = items.splice(1, 2);

    // 원본 배열이 변경된다.
    console.log(items); // [ 'one', 'four' ]
    // 제거한 요소가 배열로 반환된다.
    console.log(res);   // [ 'two', 'three' ]

    res = item.splice(1,2,'x','y'); 
    console.log(items) // ['one','x','y','four'] 배열이 변경된다.
    console.log(res) // ['two','thrre'] 어떤 요소가 바뀐지 반환한다.
    res = items.splice(1, 0, 'x'); //[1] 부터 0개의 요소를 제거, 'x' 추가 == 제거안하고 추가만함
    console.log(items) // ['one','x','x','y','four']

    ```

- Array.prototype.sort(comparefunc)

  - 배열의 내용을 적절하게 정렬한다.

  - ```js
    var fruits = ['Banana', 'Orange', 'Apple', 'Mango'];

    // ascending(오름차순)
    fruits.sort();
    console.log(fruits); // [ 'Apple', 'Banana', 'Mango', 'Orange' ]
    // 숫자 배열 오름차순 정렬
    // compareFunction의 반환값이 0보다 작은 경우, a를 우선한다.
    points.sort(function (a, b) { return a - b; });
    console.log(points); // [ 1, 5, 10, 25, 40, 100 ]

    // 숫자 배열에서 최소값 취득
    console.log(points[0]); // 1

    // 숫자 배열 내림차순 정렬
    // compareFunction의 반환값이 0보다 큰 경우, b를 우선한다.
    points.sort(function (a, b) { return b - a; });
    console.log(points); // [ 100, 40, 25, 10, 5, 1 ]

    // 숫자 배열에서 최대값 취득
    console.log(points[0]); // 100

    ```

  - ​

pop()과 shift() 는 반대의 성향