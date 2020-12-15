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

  - 새로운 배열을 만들고 반환하므로 항상 리턴값을 받아줘야한다.

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

  - 배열 요소 전체를 연결하여 **생성한 문자열을 반환**한다. 기본구분자는 `,`이다.

  - ```js
    // str = arr.join([기본구분자 = ','])

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

  - `pop`메서드는 배열의마지막 요소를 **제거하고 그 요소를 반환**한다.

  - `pop`은 `push`와 함께 배열을 스택처럼 동작하게 한다.`

  - ```js
    var a = ['a', 'b', 'c'];
    var c = a.pop();

    // 원본 배열이 변경된다.
    console.log(a); // a --> ['a', 'b']
    console.log(c); // c --> 'c'
    ```

- Array.prototype.push()

  - 인자로 전달된 항목을 배열의 끝에 추가한다. `concat` 메소드와 다르게 인수에 넘어온 배열자체를 배열에 추가한다. **리턴값**은 배열의 새로운 `length` 값이다.

  - concat()과 다르게  값자체가 배열의 요소가 된다.

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

  ![array-method](http://poiemaweb.com/img/array-method.png)



- Array.prototype.slice(start,end)

  - 배열의 특정 부분에 대한 **복사본을 생성하여 리턴**한다.

    첫번째 매개변수 start에 해당하는 인덱스를 갖는 요소부터 매개변수 end에 **해당하는 인덱스를 가진 *요소 전까지*** 복사된다.

  - end 매개변수의 기본값은 length이다.

  - 유사배열을 배열로 변경할떼 쓴단

  - ```js
    var items = ['a', 'b', 'c'];// 원본은 변경돼지 않는다.

    // items[0]부터 items[1] 이전(items[1] 미포함)까지 반환
    var res1 = items.slice(0, 1);
    console.log(res1);  // [ 'a' ]


    // items[1]부터 이후의 모든 요소 반환
    var res3 = items.slice(1);
    console.log(res3);  // [ 'b', 'c' ]

    // 인자가 음수인 경우 배열의 끝에서 2개의 요소를 반환
    // 지양해야하는 방법
    var res4 = items.slice(-2);
    console.log(res4);  // [ 'b', 'c' ]
    ```

- Array.prototype.splice(start, deleteCount, item…)

  - 기존의 배열의 요소를 제거하고 그 위치에 새로운 요소를 추가한다. 배열 중간에 새로운 요소를 추가할 때도 사용된다.

    - item매개변수는 삭제한 위치에 추가될 요소들을 넣으며 , 옵션값qsaaasd															이다.

  - 새로운 **리턴값은 제거한요소**이며  **원본 배열을 수정**한다.

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
    var items = ['one', 'four'];


    // items[1]부터 0개의 요소를 제거하고 그자리(items[1])에 새로운 배열를 추가한다. 제거된 요소가 반환된다.
    // items.splice(1, 0, ['two', 'three']); // [ 'one', [ 'two', 'three' ], 'four' ]
    Array.prototype.splice.apply(items, [1, 0].concat(['two', 'three']));
    // ES6
    // items.splice(1, 0, ...['two', 'three']);

    console.log(items); // [ 'one', 'two', 'three', 'four' ]
    ```

- Array.prototype.sort(comparefunc)

  - 배열의 내용을 적절하게 정렬한다.

  - ```js
    var fruits = ['Banana', 'Orange', 'Apple', 'Mango'];

    // ascending(오름차순)
    fruits.sort();
    console.log(fruits); // [ 'Apple', 'Banana', 'Mango', 'Orange' ]
    fruits.reverse().sort();
    console.log(fruits); // [ , 'Banana','Orange','Mango','Apple' ]


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

  - pop()과 shift() 는 반대의 성향

  ​

- Array.prototype.forEach()

  - 배열을 순회하며 **배열의 각 요소에 대하여 인자로 주어진 콜백함수를 실행**한다. 콜백함수의 매개변수를 통해 배열 요소의 값, 요소 인덱스, 순회할 배열을 전달 받을 수 있다. **반환값은 undefined**이다.
  - forEach 메소드는 for 문과는 달리 **break 문을 사용할 수 없다.**
  - for문이 forEach()보다 빠르지만 forEach()를 사용하면 쓸데없는 변수선언을 줄일 수 있고 가독성을 높일 수 있다.
  - 하나하나의 요소에 로직을 수행할 때 forEach()를 사용

   ```js
   var total = 0;
  var testArray = [1, 3, 5, 7, 9];

  // forEach 메소드는 원본 배열을 변경하지 않는다.
  // item: 요소의 값, index: 요소 인덱스, array: 순회할 배열
  // 
  testArray.forEach(function (item, index, array) {
    console.log('[' + index + '] = ' + item);
    total += item;
  });

  console.log(total); // 25
  console.log(testArray); // [ 1, 3, 5, 7, 9 ]



  testArray = [1, 2, 3, 4];

  // 원본 배열을 변경하려면 콜백 함수의 3번째 인자를 사용한다.
  testArray.forEach(function (item, index, array) {
    array[index] = Math.pow(item, 2);
  });
  console.log(testArray); // [ 1, 4, 9, 16 ] // 원본이 변경되었다. forEach()의 3번째 인자가 있기 때문이다.

   ```

- 두번째 인자로 this를 전달 할 수 있다.

  ```js
  function Counter() {
    this.sum = 0;
    this.count = 0;
  }

  Counter.prototype.add = function (array) {
    // entry는 array의 배열 요소의 값
    // this == counter
    array.forEach(function (entry) {
      this.sum += entry; // 2번째 인자 "this를 전달하지 않으면" this === window
      this.count++;
    }, this); // 여기서의 this는 counter
  };

  var counter = new Counter();
  counter.add([2, 5, 9]);
  console.log(counter.count); // 3
  console.log(counter.sum);   // 16
  ```



- Array.prototype.map()

  - 배열을 순회하며 각 요소에 대하여 인자로 주어진 **콜백함수의 반환값(결과값)으로 새로운 배열을 생성하여 반환한다.** 이때 원본 배열은 변경되지 않는다. IE 9 이상에서 정상 동작한다.
  - 콜백함수의 매개변수를 통해 배열 요소의 값, 요소 인덱스, 순회할 배열을 전달 받을 수 있다.
  - map()은 항상 원본배열과 같은 길이의 새로운 배열을 만듦

  ```js
  var numbers = [1, 4, 9];

  // 배열을 순회하며 각 요소에 대하여 인자로 주어진 콜백함수를 실행
  var roots = numbers.map(function (item) { // forEach처럼 (item, index, array) 이 있다고 상상하자
    return Math.sqrt(item); // return 이 반드시 있어야 한다. 왜? 새로운 배열을 만들어야하니까
  });

  // map 메소드는 새로운 배열을 반환한다
  console.log(roots);   // [ 1, 2, 3 ]
  // map 메소드는 원본 배열은 변경하지 않는다
  console.log(numbers); // [ 1, 4, 9 ]

  numbers = [1, 4, 9];

  // 배열을 순회하며 각 요소에 대하여 인자로 주어진 콜백함수를 실행
  roots = numbers.map(function (item) {
    return ++item;  // return하지 않으면 새로운 배열에 반영되지 않는다.
  });

  // map 메소드는 새로운 배열을 반환한다
  console.log(roots);   // [ 2, 5, 10 ]
  // map 메소드는 원본 배열은 변경하지 않는다
  console.log(numbers); // [ 1, 4, 9 ]
  ```

  ​

- Array.prototype.filter()

  - 원본배열에서 원하는 요소를 뽑아낸다. 

  - 배열을 순회하며 각각의 요소를 인자로 콜백함수를 실행한다. 단,  리턴값이 true인 배열 요소의 값으로 만든 새로운 배열을 반환한다.

  -  IE 9 이상에서 정상 동작한다.

  - filter()도 map(), forEach()와 같이 두번째 인자로 this를 전달할 수 있다.

    ```js
    var result = [1, 2, 3, 4, 5].filter(function (item, index, array) {
      console.log('[' + index + '] = ' + item);
      return item % 2; // 홀수만을 필터링한다 (1은 true 0은 false로 평가된다)
    });

    console.log(result); // [ 1, 3, 5 ]
    ```

    ​

- Array.prototype.reduce()

  - 배열을 순회하며 각 요소에 대하여 **이전의 콜백함수 실행 반환값을 전달**하여 콜백함수를 실행하고 **그 결과를 반환**한다. IE 9 이상에서 정상 동작한다.

  ```js
  /*
  previousValue: 이전 콜백의 반환값
  currentValue : 배열 요소의 값
  currentIndex : 인덱스
  array        : 순회할 배열
  */
  var result = [1, 2, 3, 4, 5].reduce(function (previousValue, currentValue, currentIndex, array) {
    console.log(previousValue + '+' + currentValue + '=' + (previousValue + currentValue));
    return previousValue + currentValue; // 결과는 다음 콜백의 첫번째 인자로 전달된다
  });

  console.log(result); // 15: 1~5까지의 합
  /*
  1: 1+2=3
  2: 3+3=6
  3: 6+4=10
  4: 10+5=15
  15
  */
  ```

  ![reduce](http://poiemaweb.com/img/reduce.png)

  피보나치와 유사



- Array.prototype.some()

  - 배열 내 일부 요소가 **콜백함수의 테스트를 통과하는지 확인**하여 그 결과를 boolean으로 반환한다. **하나라도 통과하면 true**

  - IE 9 이상에서 정상 동작한다.

    ```js
    // 배열 내 요소 중 10보다 큰 값이 1개 이상 존재하는지 확인
    var res = [2, 5, 8, 1, 4].some(function (item) {
      return item > 10;
    });
    console.log(res); // false

    res = [12, 5, 8, 1, 4].some(function (item) {
      return item > 10;
    });
    console.log(res); // true

    // 배열 내 요소 중 특정 값이 1개 이상 존재하는지 확인
    res = ['apple', 'banana', 'mango'].some(function (item) {
      return item === 'banana';
    });
    console.log(res); // true
    ```

    ​



- Array.prototype.every()

  - 배열 내 모든 요소가 **콜백함수의 테스트를 통과하는지 확인**하여 그 결과를 boolean으로 반환한다. **모두 통과하면 true**

  - IE 9 이상에서 정상 동작한다.

    ```js
    // 배열 내 모든 요소가 10보다 큰 값인지 확인
    var res = [21, 15, 89, 1, 44].every(function (item) {
      return item > 10;
    });
    console.log(res); // false

    res = [21, 15, 89, 100, 44].every(function (item) {
      return item > 10;
    });
    console.log(res); // true
    ```

    ​


- Array.prototype.find()

  - 배열을 순회하며 각 요소에 대하여 인자로 주어진 **콜백함수를 실행하여 그 결과가 참인 첫번째 요소를 반환한다.**

  - ES6에서 새롭게 도입된 메소드로 Internet Explorer에서는 지원하지 않는다.

    ```js
    var array = [
      { id: 1, name: 'Lee' },
      { id: 2, name: 'Kim' },
      { id: 2, name: 'Choi' },
      { id: 3, name: 'Park' }
    ];

    // 콜백함수를 실행하여 그 결과가 참인 첫번째 요소를 반환한다.
    var result = array.find(function (item) {
      return item.id === 2;
    });

    // ES6
    // const result = array.find(item => item.id === 2;);

    console.log(result); // { id: 2, name: 'Kim' }

    // filter는 콜백함수의 실행 결과가 true인 배열 요소의 값만을 추출한 새로운 배열을 반환한다.
    result = array.filter(function (item) {
      return item.id === 2;
    });

    console.log(result); // [ { id: 2, name: 'Kim' },{ id: 2, name: 'Choi' } ]
    ```

    ​

