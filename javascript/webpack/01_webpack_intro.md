## 웹팩이란?
프론트엔드 개발시 여러 파일을 하나 혹은 N개의 결과물로 만들기 위한 모듈 번들러. 비슷한 도구로는 parcel, snowpack 등이 있다.
- 웹팩에서의 모듈
	- 하나의 역할을 수행할 수 있는 단위
	- 프로젝트를 구성하는 모든 자원 (html, css, js, font, image)

### 모듈을 번들링한다는게 정확히 무슨 말인가?
- `.js`, `.sass`, `.jpg` 등 프로젝트에 사용된 다수의 파일들을 빌드하면서 압축, 전처리, 최적화 등을 해주고 1개 혹은 n개의 파일로 묶어주는 도구
	- 웹팩 라이브러리를 사용하면 가능한것들의 예시
		- 개발시에 사용한 이미지를 webpack을 통해서 빌드하면 더 작은 용량으로 압축
		- `.scss`, `.sass`, `.less` 등으로 만들어진 preprocessors들을 브라우저에서 사용가능한 `.css` 파일로 변환
		- `js`결과물을 1개의 파일로 빌드
- 과거에는 `.html`파일 내부에 script태그로 라이브러리들을 불러오도록 작성했다. 이는 네트워크의 상태나 script태그의 위치에 따라 개발자가 의도하지않았던 결과를 초래할 수 있고, 코드관리에도 불편한 점이 있었다. 웹팩은 이러한 점들 또한 해결해준다.

### 웹팩으로 번들링하는 이유는?
1. 초기 웹은 간단한 구조일거라고 생각하고 js를 개발함 -> 웹이 발전해가면서 js가 복잡해짐 -> 여러 문제점 발생
	- 변수 스코프 겹침
	- 브라우저별 HTTP 요청 숫자 제약
		> 최신 브라우저들은 대부분 한번에 6개의 요청을 보낼 수 있다. ie11은 13개
	- 미사용 코드 관리 등
2. 초기에는 grunt, gulp같은 웹 테스크 매니저툴이 웹팩과 비슷한 역할을 함
3. 웹 테스크 매니저의 한계를 개선하고, 추가적인 기능이 가능해진 웹팩이 만들어지게 됨
	- 간단한 설정
	- lazy-loading
	- task를 위한 라이브러리 관리

### 명령어
- `webpack`
	- 프로젝트 빌드를 수행함.
	- 보통 cli창에서 webpack 명령을 일일이 수행하지 않고 package.json 의 `scripts`옵션에 미리 등록하여 사용함
	- 옵션을 줘보자
		- `--mode=none|development|production`
			- 현재 빌드하려는 모드를 설정한다.
			- 이 값을 사용해 상황마다 특정 웹팩 라이브러리만 빌드에 사용되게 할수 있다.
				- ex) 개발시에는 난독화 라이브러리를 적용시키지 않도록하여 에러가 발생했을 때 좀 더 디버깅하기 쉬운 환경을 만들수 있다.
		- `--entry=src/index.js`
		- `--output=public/output.js`
		-> 옵션값들을 package.json에서 수정하는 것들은 가독성, 유지보수 측면에서 매우 비효율적임. -> `webpack.config.js`파일을 만들어 js파일로 관리한다.
		```json
		...
		"scripts": {
			"build": "webpack --mode=development --entry=src/index.js --output=dist/main.js"
		},
		...
		```

		아래 처럼 js파일로 만들어 관리하는 것이 훨씬 가독성이 좋다.
		```js
		// webpack.config.js
		// `webpack` command will pick up this config setup by default
		var path = require('path');

		module.exports = {
			mode: 'none',
			entry: './src/index.js',
			output: {
				filename: 'main.js',
				path: path.resolve(__dirname, 'dist')
			}
		};
		```

### build 결과물
- webpack은 사용된 js파일을 배열로 관리한다.
- 결과물 내부는 즉시실행함수(IIFE)를 활용하여 작성된다.

#### build 결과물 - sourcemap
- 웹팩의 결과물은 난독화가 가능하다. 그러나 개발하면서 디버깅을 하기위해서는 가독성이 좋은 코드를 브라우저에서 볼수 있어야한다.
- 웹팩은 해당 부분을 설정할 수 있는 옵션을 제공한다
	```js
	...
		devtool: 'source-map'

	};
	...
	```


### 과거의 툴 gulp, grunt
- 파일에 대해 개발자가 직접 설정을 해주고, 각 파일에대해 태스크를 진행하는 방식
	- project
		- js - js - js - js
		- css - css - css - css
		- jpg - jpg - jpg - jpg

- 웹팩은?
	- project
		- js - js - jpg - css
			- css
				- woff2
				- woff
				- svg
				- css
	- 진입점이 주어지면 나머지의 연관관계를 웹팩이 해석해서 결과물을 만들어냄
