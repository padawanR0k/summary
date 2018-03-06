# 1. Node.js 모듈



Node.js는 module 단위로 각 기능을 분할할 수 있다. module은 파일과 1대1의 대응 관계를 가지며 하나의 모듈은 자신만의 **독립적인 실행 영역(Scope)**를 가지게 된다. 따라서 **클라이언트 사이드 JavaScript와는 달리 전역변수의 중복 문제가 발생하지 않는다.**

모듈은 **module.exports 또는 exports 객체**를 통해 정의하고 외부로 공개한다. 그리고 공개된 모듈은 **require 함수**를 사용하여 임포트한다.



# 2. exports

모듈은 독립적인 파일 스코프를 갖기 때문에 모듈 안에 선언한 모든 것들은 기본적으로 해당 모듈 내부에서만 참조 가능하다. 

단, 공개하고 싶은 부분만 exports에 묶어서 공개할 수 있다.

```js
// circle.js
const { PI } = Math; // 외부에 공개되지않는다.

exports.area = (r) => PI * r * r; // 

exports.circumference = (r) => 2 * PI * r;
```



require 함수를 사용하여 임의의 이름으로 circle 모듈을 import한다. 모듈의 확장자는 생략할 수 있다.

```js
// app.js
const circle = require('./circle.js'); // == require('./circle')

console.log(`지름이 4인 원의 면적: ${circle.area(4)}`);
console.log(`지름이 4인 원의 둘레: ${circle.circumference(4)}`);
```

여러 객체를 반환할수 있다는 장점이있음



# 3. module.exports

```js
// circle.js
const { PI } = Math;

module.exports = function (r) { // 여기서 r은 자유변수다.
  return {
    area() { return PI * r * r; },
    circumference() { return 2 * PI * r}
  };
//circle 모듈의 module.exports에는 하나의 함수를 할당하였다.

// app.js
const circle = require('./circle');
const myCircle = circle(4); // 매개변수를 던져줘야한다. 

console.log(`지름이 4인 원의 면적: ${myCircle.area()}`); // 메서드실행
console.log(`지름이 4인 원의 둘레: ${myCircle.circumference()}`);
```



| 구분           | 모듈 정의 방식                                               | require 함수의 호출 결과                                     |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| exports        | **exports 객체에는 값을 할당할 수 없고 공개할 대상을 exports 객체에 프로퍼티 또는 메소드로 추가한다.** | exports 객체에 추가한 프로퍼티와 메소드가 담긴 객체가 전달된다. |
| module.exports | **module.exports 객체에 하나의 값(기본자료형, 함수, 객체)만을 할당한다.** | module.exports 객체에 할당한 값이 전달된다.                  |

## 3.1 module.exports에 함수를 할당하는 방식



```js
// foo.js
module.exports = function(a, b) {
  return a + b;
};

// app.js
const add = require('./foo');

const result = add(1, 2);
console.log(result); // => 3
```

module.exports는 1개의 값만을 할당할 수 있다. 모듈에서 1개의 값만을 공개하는 것은 불편할 수 있다.



## 3.2 exports에 객체를 할당하는 방식

```js
// foo.js
module.exports = {
  add (v1, v2) { return v1 + v2 },
  minus (v1, v2) { return v1 - v2 }
};

// app.js
const calc = require('./foo');

const result1 = calc.add(1, 2);
console.log(result1); // => 3

const result2 = calc.minus(1, 2);
console.log(result2); // => -1
```



# 4. require

require 함수의 인수에는 파일뿐만 아니라 디렉터리를 지정할 수도 있다. 예를 들어 다음과 같은 디렉터리 구조의 경우를 살펴보자.

```code
project/
├── app.js
└── module/
    ├── index.js
    ├── calc.js
    └── print.js
```

아래과 같이 모듈을 명시하지 않고 require 함수를 호출하면 해당 디렉터리의 index.js을 로드한다.

```js
const myModule = require('./module');
```

특별한 요청이 없으면 폴더에서 index라는 이름을 가진 파일을 먼저 살핀다.



```js
// module/index.js
module.exports = {
  calc: require('./calc'),
  print: require('./print')
};

// module/calc.js
module.exports = {
  add (v1, v2) { return v1 + v2 },
  minus (v1, v2) { return v1 - v2 }
};

// module/print.js
module.exports = {
  sayHello() { console.log('Hi!') }
};

// app.js
const myModule = require('./module');

// module/calc.js의 기능
const result = myModule.calc.add(1, 2);

console.log(result);

// module/print.js의 기능
myModule.print.sayHello();
```



# 5. 코어 모듈과 파일 모듈

Node.js는 기본으로 포함하고 있는 모듈이 있다. 이를 코어 모듈이라 한다. **코어 모듈을 로딩할 때에는 패스를 명시하지 않아도 됀다**.

```js
const http = require('http');
```

npm을 통해 설치한 외부 패키지 또한 패스를 명시하지 않아도 무방하다.

```js
const mongoose = require('mongoose');
```

코어 모듈과 외부 패키지 이외는 모두 파일 모듈이다. 파일 모듈을 로딩할 때에는 패스를 명시하여야 한다.

```js
const foo = require('./lib/foo');
```





> 프레임워크나 라이브러리는 사전적으로 공부하지말고 기본적인 부분만 공부한 후 직접 프로젝트를 해보면서 모르는게 나올때마다 찾아가면서 공부하는게 이득이다.

