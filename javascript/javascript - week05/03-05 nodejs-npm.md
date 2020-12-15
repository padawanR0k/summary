# 1. 모듈화와 CommonJS

모듈화는 내가 필요한것만 공개할 수 있어야하고 공개한것을 import 할 수 있어야한다.

JavaScript를 Client-side에 국한하지 않고 범용적으로 사용하고자 하는 움직임이 생기면서 모듈 기능은 반드시 해결해야하는 핵심 과제가 되었고 이런 상황에서 제안된 것이 **CommonJS와 AMD(Asynchronous Module Definition)**이다.

AMD 방식은 CommonJS에 비해 문법이 다소 까다로우며 CommonJS와는 달리 비동기 방식(asynchronous loading)으로 동작한다.

Node.js는 사실상 모듈 시스템의 **사실상 표준인 CommonJS를 채택**하였고 현재는 독자적인 진화를 거쳐 CommonJS 사양과 100% 동일하지는 않지만 기본적으로 **CommonJS 방식을 따르고 있다**. 



# 2. npm

자바스크립트 패키지 매니저로서 Node.js에서 사용할 수 있는 모듈을을 패키지화하여 다운로드가 편하게 저장소역할을 하고 설치 및 관리를 위한 CLI를 제공함

https://www.npmjs.com/

패키지를 설치할 때는 제대로된 패키지인지 확인하고 설치하자



## 2.1 설치

package.json의 모든 디펜던시 패키지 설치

```bash
$ npm install 
```



특정 package 설치

``` bash
$ npm install <package> 
```



특정버전 설치

```bash
$ npm install node-emoji@1.0.0
```





node-emoji 설치

```bash
$ mkdir emoji && cd emoji
$ npm install node-emoji
```



package.json이 안생긴이유는 `npm init`을 안했기때문임



## 2.2 지역설치와 전역설치

옵션을 별도로 지정하지 않으면 지역으로 설치되며 프로젝트 루트 디렉터리에 `node_modules` 디렉터리가 자동 생성된다.

지역으로 설치된 패키지는 해당 프로젝트 내에서만 사용할 수 있다.

전역에 패키지를 설치하려면 `-g` 옵션을 지정한다. 



전역에 설치된 패키지는 OS에 따라 설치 장소가 다르다.

- macOS의 경우

  /usr/local/lib/node_modules

- 윈도우의 경우

  c:\Users\%USERNAME%\AppData\Roaming\npm\node_modules



```bash
$ node
> var emoji = require('node-emoji').emoji;
undefined
> console.log(emoji.heart);
❤️
undefined
```



## 2.3 package.json과 dependency(의존성)관리

Node.js 프로젝트에서는 많은 의존 패키지를 사용하게 되고 패키지의 버전도 빈번하게 업데이트되기 때문에 프로젝트가 의존하고 있는 패키지를 일괄 관리할 필요가 있다. npm은 `package.json` 파일을 통해서 **프로젝트 정보와 패키지의 의존성(dependency)을 관리**한다. 이미 작성된 **package.json은 팀 내에 배포하여 동일한 개발 환경을 빠르게 구축할 수 있는 장점**을 가진다. 



package.json을 생성하기 위해서는 `npm init` 명령어를 사용한다.

`npm init -y`  

`-y` 옵션은  디폴트 설정으로 package.json을 생성해준다.

```bash
$ npm init -y
Wrote to /Users/leeungmo/Desktop/emoji/package.json:

{
  "name": "emoji", // 반드시 있어야하는 부분
  "version": "1.0.0", // 반드시 있어야하는 부분
  "description": "", // 설명이 들어갈 부분
  "main": "index.js", // 애플리케이션이 기동할때 제일 처음 실행할 진입점
  "dependencies": { // 설치한 패키지와 패키지의 버전
    "node-emoji": "^1.8.1"
  },
  "devDependencies": {}, // -dev옵션이 붙여진체로 설치된 패키지. 서버에 올려질 필요가 없는 패키지들
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
    // 실행해야할 명령어들 shell script
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```



`name`과 `version`은 패키지의 고유성을 판단하게 되기 때문에 생략할 수 없으며 필수로 입력하여야 한다.

`dependencies`에는 해당 프로젝트가 의존하는 패키지들의 이름과 버전을 명시한다. npm install 명령어에 `--save` 옵션을 사용하면 패키지 설치와 함께 package.json의 dependencies에 설치된 패키지와 버전이 기록된다.

```bash
$ npm install <package> --save-dev
```

npm install 명령어를 사용하면 package.json에 명시된 의존 패키지를 한번에 설치할 수 있다.



## 2.4 Semantic versioning

npm install 명령어의 패키명 뒤에 @버전을 추가하면 패키지 버전을 지정하여 설치할 수 있다.

```bash
$ npm install node-emoji@1.5.0

...
  "dependencies": {
    "node-emoji": "^1.5.0" 
  },
...
```

 node-emoji의 버전에 `^`(캐럿)이 추가된 것을 확인할 수 있다. 이것은 패키지 버전을 지정하였을 때 뿐만이 아니라 `--save-exact` 옵션을 지정하지 않으면 기본적으로 추가되는 것이다. 이 `^`(캐럿)은 이후 **해당 패키지의 버전이 업데이트되었을 경우**, 마이*너 버전 범위 내에서 업데이트를 허용*한다는 의미이다.

![sementic-versioning](http://poiemaweb.com/img/sementic-versioning.png)

메이저 버전 변경 : 1.0.0 과 2.0.0 이 호환 돼지않을 수 도 있다 

마이너 버전 변경 : 호환성 보장함

패치 버전 변경 : 버그가 있어서 고침

| 표기법    | Description                       |
| --------- | --------------------------------- |
| version   | 명시된 version과 일치             |
| >version  | 명시된 version보다 높은 버전      |
| >=version | 명시된 version과 같거나 높은 버전 |
| <version  | 명시된 version보다 낮은 버전      |
| <=version | 명시된 version과 같거나 낮은 버전 |
| ~version  | 명시된 version과 근사한 버전      |
| ^version  | 명시된 version과 호환되는 버전    |

`~(틸트)`와 `^(캐럿)`의 차이는 아래와 같다.

~(틸트)는 패치 버전 범위 내에서 업데이트한다. :

- ~0.0.1 : 0.0.1 <= version < 0.1.0
- ~0.1.1 : 0.1.1 <= version < 0.2.0

^(캐럿)는 마이너 버전 범위 내에서 업데이트한다. :

- ^1.0.2 : 1.0.2 <= version < 2.0

[npm semver calculator](https://semver.npmjs.com/)