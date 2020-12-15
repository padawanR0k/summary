# 1. Introduction

웹페이지의 보조적인 기능을 수행하기위해 한정적인 용도로 만들어진 javascript의 문제점을 극복하기 위해 만들어진 대체언어이다. HTML5의 등장은 Plug-in에 의존하는 기존의 구축 방식을 JavaScript로 대체시켰다. 

JavaScript의 태생적 문제를 극복하기 위한 노력의 일환으로 CoffeeScript, Dart, Haxe와 같은 AltJS(JavaScript의 대체언어)가 등장하였다.

타입스크립트는 Transpile 언어이고 TypeScript는 javascript의 상위확장이다.

![typescript superset](http://poiemaweb.com/img/typescript-superset.png)

ES5의 Superset이므로 기존의 JavaScript(ES5) 문법을 그대로 사용할 수 있으며 ES6의 새로운 기능들을 사용하기 위해 Babel과 같은 별도 Transpiler를 사용하지 않아도 된다.

아직 자바스크립트는 규모가 큰 애플리케이션을 만들기엔 무리다. 그걸 보완하기위해 만들어진 언어가 typescript이다.



# 2. TypeScript를 사용하는 이유

1. 코드 자동완성
2. 디버깅이 더 쉬워짐
3. 코드를 작성하는 도중에 에러를 바로바로 알 수 있음

## 2.1 정적 타입

개발자의 **의도가 코드에** 들어난다. 이는 코드의 가독성을 향상 시키고 예측을 가능하게 하며 디버깅을 쉽게 한다.

```js
function sum(a, b) {
  return a + b;
}

sum('x', 'y'); // 'xy'  이걸 원한게 아니다.



function sum(a: number, b: number) {
  return a + b;
}

sum('x', 'y'); // error TS2345: Argument of type '"x"' is not assignable to parameter of type 'number'  /  type으로 런타임때 오류를 걸러낼 수 있다.
```



## 2.2 도구의 지원

IDE(통합개발환경)

 타입 정보를 제공함으로써 **높은 수준의 IntelliSense, 코드 어시스트, 타입 체크, 리팩토링** 등을 지원받을 수 있다.

이러한 도구의 지원은 대규모 프로젝트를 위한 필수적 요소이기도 하다.



## 2.3 강력한 OOP 지원

크고 복잡한 프로젝트의 코드 기반을 쉽게 구성할 수 있도록하여 javascript 의 장점을 합침

여러명의 개발자가 협업을할때 OOP는 중요하다.



## 2.4 ES6 / ES Next 지원

ES5와 비교할 때 개발환경 구축의 관점에서 다소 복잡해진 측면이 있으나 현재 ES6를 완전히 지원하지 않고 있는 브라우저를 고려하여 Babel등의 트랜스파일러를 사용해야 하는 현 상황에서 TypeScript 개발환경 구축에 드는 수고는 그다지 아깝지 않을 것이다.

## 2.5 Angular

구글은 angular의 정식언어로 typesciprt를 결정했다.



# 3. 개발환경 구축

SASS파일이 CSS로 컴파일되어 사용되듯 이것도마찬가지이다.

## 3.1 설치

``` bash
$ npm install -g typescript
```



## 3.2 TypeScript 컴파일러 설치 및 사용법

```typescript
// person.ts
class Person {

  private name: string; // es6에서는 class에 메서드만 써야하는데 ts를 쓰면 멤버변수와 변수타입이 선언가능하다.

  constructor(name: string) {
    this.name = name;
  }

  sayHello() {
    return "Hello, " + this.name;
  }
}

const person = new Person('Lee');

console.log(person.sayHello());
```

private, constructor =  컴파일하면 안보이는 부분들.

```js
// student.ts
import { Person } from './person';

class Student extends Person {
  study(): string {
    return `${this.name} is studying!!`;
  }
}

const student = new Student('Lee');

console.log(student.sayHello());
console.log(student.study());
```

