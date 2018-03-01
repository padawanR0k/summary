현재 브라우저는 ES6를 완전하게 지원하지 않는다.

Babel과 Webpack의 버전은 아래와 같다.

- babel-cli : 6.26.0
  - babel-loader : 7.1.3
  - babel-preset-env : 1.6.1
- webpack : 4.0.1
  - webpack-cli : 2.0.9
- npm : 5.6.0

# 1. Babel CLI 설치

Babel은 ES6를 ES5 이하의 버전으로 트랜스파일링한다.

```bash
$ mkdir babel-project && cd babel-project
$ npm init -y
$ npm install babel-cli --save-dev  
```

--save-dev를 붙이면 개발에만 필요한, 서버에 올리지않는 패키지를 다운받을때 붙여준다.

# 2. .babelrc 설정 파일 작성

Babel의 사용을 위해서는 먼저 preset을 설치가 필요하다. env preset은 설정된 환경에 적합한 플러그인을 자동으로 설정해준다.

```bash
$ npm install babel-preset-env --save-dev
```

```json
//package.json
{
  "name": "babel-project",
  "version": "1.0.0",
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-env": "^1.6.1"
  }
}
// .babelrc
{
  "presets": ["env"]
}
```



# 3. 트랜스파일링

npm scripts를 사용하여 트랜스파일링하는법

```json
// package.json
{
  "name": "babel-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-env": "^1.6.1"
  }
}
```

src/js 폴더의 ES6를 트랜스파일링한 후, 결과물을 dist/js 폴더에 저장

```js
// src/js/main.js
import { pi, square, Person } from './lib';

console.log(pi);
console.log(square(10));
console.log(new Person('Lee'));
// src/js/lib.js
export const pi = Math.PI;

export function square(x) {
  return x * x;
}

export class Person {
  constructor(name) {
    this.name = name;
  }
}
```

```bash
$ npm run build
```



# 4. ES6 개발 환경 구축

 node.js 환경에서 실행기에 main.js가 아무 탈 없이 실행 되었다.   현재 대부분의 브라우저는 ES6의 모듈을 지원하지 않고 있다. 따라서 추가적인 모듈로더가 필요하다.

Webpack과 Babel을 이용하여 ES6 환경을 구축하여 보자.

![Webpack](http://poiemaweb.com/img/webpack.png)

hello.js, world.js, entry.js 모듈을 작성한다. hello.js와 world.js는 entry.js에 의해 import되는 의존 모듈이다.

[poiemaweb](http://poiemaweb.com/es6-babel#4-es6-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95)