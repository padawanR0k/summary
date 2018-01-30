# 제어문

제어문(Control flow statement)은 조건에 따른 명령 실행(조건문)이나 반복 실행(반복문)이 필요할 때 사용된다.

***

## 1. 블록구문

구문들의 집합중의 가장 기본이 되는 구문. 

일반적으로 함수, 객체리터럴, 흐름제어구문에서 사용된다.

```javascript
// 함수 선언문 : "재사용"이 가능하다.
function foo() {
  var x = 1, y = 2; 
  console.log(x + y);  
}// 함수내 블록구문이라서 x , y 지역변수 인정됨
foo();

// 객체리터럴에 의한 객체 선언
var obj = {
  x: 1,
  y: 2
}; 
console.log(obj.x + obj.y);

// 흐름 제어 구문(control flow statement)
var x = 0;
while (x < 10) {
  x++;
}
console.log(x);
```

---

## 2. 조건문

> 데이터의 흐름을 제어한다는 것은 일정 조건에 따른 의사결정(decision)을 통해 다음 진행 흐름으로 유도(Control flow)하는 것이다. 이는 가장 원시적인 형태의 인공 지능(Artificial Intelligence)을 부여하는 것이라고 볼 수 있다. 즉 의사결정(상황판단)의 기준을 제시하고 그 결과에 따른 행위를 지시하는 것이다.

조건문은 주어진 조건식이 참(`true`)인지 거짓(`false`)인지에 따라 실행되어질 구문들의 집합이다.

---

### 2- 1. if문

```javascript
if (조건식){
  // 조건식  true일때
}else{
  // 조건식 false일때
}
```

---

### 2- 2. switch 문

switch 문의 경우, `switch`변수의 값과 일치되는 `case`문으로 실행 순서가 이동하게 된다. `switch`변수의 값과 일치되는 `case`문이 없다면 실행 순서는 `default`로 이동한다.

```javasc
var animal = 'tiger';

switch (animal){
  case 'tiger':
  	console.log('TIGER');
  	break;
  case 'lion':
  	console.log('LION');
  	break;
  case 'dog':
  	console.log('DOG');
  	break;
  case 'cat':
  	console.log('CAT');
  	break;
  defualt:
  	console.log('unknown animal');
}
```

마치 `else if`와 `else` 구문을 많이 쓴것과 유사하다.

---

## 3. 반복문 (Loop)

JavaScript는 3가지의 반복문 `for`, `while`, `do while`을 제공한다.



### 3- 1. for 문

for문의 실행순서는 아래와 같다.

```javascript
for (var i = 0; i < 2; i++) {
  console.log(i);
}
```

![for문 실행 순서](http://poiemaweb.com/img/for-statement.png)

|      | i    | i < 2 | i++       | console.log(i) |
| ---- | ---- | ----- | --------- | -------------- |
| 1    | 0    |       |           |                |
| 2    |      | true  |           |                |
| 3    |      |       |           | 0              |
| 4    |      |       | i = 0 + 1 |                |
| 5    |      | true  |           |                |
| 6    | 1    |       |           | 1              |
| 7    |      |       | i = 1+ 1  |                |
| 8    |      | false |           |                |

#### 배열을 순회하는 for문


// for-in, foreach, for-of (ES6), array.entries()

---

### 3- 2. while 문

while 문은 조건문이 참이면 코드 블럭을 계속해서 반복해서 실행하고 거짓이되면 반복문을 빠져나간다.

```javascript
var n = 0;
var x = 0;

// n이 3보다 작을 때까지 계속 반복한다.
while (n < 3) { // n: 0 1 2
  n++;          // n: 1 2 3
  x += n;       // x: 1 3 6
  console.log(x);
}

var i = 0;
// true는 항상 true이기 때문에 무한루프에 빠지게된다.
while (true) {
  console.log(i);
  i++;
}
```

### 3- 3. do while문

while 문과 비슷하나 먼저 1회 실행되고 조건문을 확인한다.

### 3- 4. continue

```javascript
for (var i = 0 ; i <10 ; i ++){
  if ( i < 5) continue;
  console.log(i);
}
// 1,2,3,4 는 출력이 안되고 5,6,7,8,9까지 출력된다.
```

