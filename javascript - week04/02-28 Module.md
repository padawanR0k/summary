# 1. Module ?

애플리케이션을 구성하는 **개별적 요소로서 구현된 세부 사항을 캡슐화**하고 공개가 필요한 **API를 외부에 노출**하여 다른 코드에서 **로드하여 사용할 수 있도록 작성된 재사용 가능한** 코드조각을 뜻한다.

자바스크립트의 단점

여러파일이 결국엔 하나의 파일이되고 전역이 1개가 된다. 모듈화를 하기위해서는 각각의 파일에 각각의 스코프가 있어야한다. 이래서 나온기능이 export&import이다.

현재 브라우저에는 지원하지않으나 표준스펙이다. babel이나 webpack의 도움으로 현재 브라우저에서 구현하는것이 가능하다.

ES6 모듈은 키워드 `export`, `import`를 제공한다.



# 2. export & import

모듈은 **독립적인 파일 스코프를 갖기** 때문에 모듈 안에 선언한 모든 것들은 기본적으로 **해당 모듈 내부에서만 참조 가능**하다.

export : 모듈을 외부로 공개

```js
// lib.js
export const pi = Math.PI;

export function square(x) {
  return x * x;
}

export class Person {
  constructor(name) {
    this.name = name;
  }
}

// export 한번에 붙여주기
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

export { pi, square, Person };  // 하나의 객체로 export
```



import :모듈을 내부소스코드로 불러들이는것

```js
// main.js
import { pi, square, Person } from './lib'; // import를 하려는 대상 경로

console.log(pi);         // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person('Lee')); // Person { name: 'Lee' }
```

```sequence
lib.js -> main.js: export{pi, square, person}
main.js -> lib.js: import{pi, square, person}
lib.js -> main.js: pi, square(), Person
```





이름을 변경하여 import할 수도 있다.

```js
// main.js
import { pi as PI, square as sq, Person as P } from './lib';

console.log(PI);    // 3.141592653589793
console.log(sq(2)); // 4
console.log(new P('Kim')); // Person { name: 'Kim' }
```



모듈에서 하나만을 export하는 경우, default 키워드를 사용할 수 있다. 

```js
// lib.js
function (x) {
  return x * x;
}

export default;

// 위 코드를 아래와 같이 축약 표현할 수 있다.
export default function (x) {
  return x * x;
}
```



default 키워드와 함께 export된 모듈은 {} 없이 임의의 이름으로 import한다.

```js
// main.js
import square from './lib';

console.log(square(3)); // 9
```



