## webpack의 주요 속성들
- entry
- output
- module
- mode

### entry
```js
// webpack.config.js
module.exports = {
  entry: './src/index.js'
}
```
- 웹팩이 프로젝트를 빌드할 때, 첫 진입점이 된다. (js파일 경로를 기입한다.)
	- entry를 분리하는 경우는 MPA에 적합함
- A라는 파일에서 B라는 파일을 import하게되면 A는 B파일에 의존하게된다 -> 의존성관계 생김 == 디펜던시
	- 웹팩은 디펜던시 그래프를 통해 어떤 파일들이 사용되는지 판단하고 빌드를 진행함


### output
```js
// webpack.config.js
module.exports = {
  output: {
		filename: 'bundle.js',
		filename: '[name].bundle.js', // 결과 파일명에 entry 속성을 포함
    filename: '[id].bundle.js', // 결과 파일 이름에 웹팩 내부적으로 사용하는 모듈 ID를 포함하는 옵션

    filename: '[name].[hash].bundle.js',
    // 매 빌드시 마다 고유 해시 값을 붙이는 옵션
    /**
     * A.a13.js
     * B.a13.js
     * ------
     * A 파일만 변경 후 빌드
     * ------
     * A.b2d.js
     * B.b2d.js
     *
     */



    filename: '[contenthash].bundle.js',
    // 각 파일마다 가지고 있는 콘텐츠에 의해 계산되는 hash값을 가짐
    /**
     * A.bs1.js
     * B.as2.js
     * ------
     * A 파일만 변경 후 빌드
     * ------
     * A.2oq.js
     * B.as2.js
     *
     */

    filename: '[chunkhash].bundle.js',
    // 웹팩의 각 모듈 내용을 기준으로 생생된 해시 값을 붙이는 옵션 (webpack entry를 기반으로 정의되어 고유의 hash 값을 가짐)
    // 각 파일마다 가지고 있는 콘텐츠에 의해 계산되는 hash값을 가짐
    /**
     * A.a13.js
     * B.a13.js
     * ------
     * A 파일만 변경 후 빌드
     * ------
     * A.baq.js
     * B.a13.js
     *
     */

  }
}
```
- 빌드가 완료된 후 결과물 파일의 경로와 파일명을 지정해준다.
- `hash`, `chunckhash` 옵션을 파일명에 추가한 경우, 배포시에 생기는 캐시문제를 해결해 줄수 있다. (빌드할 때 마다 다른 해쉬가 붙기때문에 `index.html`만 invalidation되면 새로운 js파일을 불러오기 때문이다)
  - `hash`
    - 빌드를 할 때마다, 매번 새로운 값들을 파일명 뒤에 붙게되고 이로 인해 변경사항이 없는 파일도 유저는 다시 로드하게되어 비효율적이다.
  - `chunkhash`
    - webpack entry를 기반으로 정의되어 고유의 hash 값을 가짐 (변경된 파일만  변경되어 `hash`보다는 효율적)
  - 참고
    - [hash vs chunkhash vs contenthash](https://sk92.tistory.com/4)
    - [What is the purpose of webpack [hash] and [chunkhash]?](https://stackoverflow.com/questions/35176489/what-is-the-purpose-of-webpack-hash-and-chunkhash)


### loader
```js
// webpack.config.js
module.exports = {
  module: { // 엔트리나 아웃풋 속성과는 다르게 module라는 이름을 사용
    rules: []
  }
}
```
- js가 아닌 웹자원들을 변환하기 위해사용
	- sass -> css, svg -> svgr 등 다양함

```js
module: {
    rules: [
      {
        test: /\.css$/,
        use: ['css-loader']
      }
    ]
	}
```
- loader = 도구
- test = 도구를 적용시킬 대상
- 예시
	- .ts 파일을 모두 .js로 트랜스파일한다.
	```js
	{ test: /\.ts$/, use: 'ts-loader' },
	```
	- 프로젝트에 사용된 .css파일을 로드한다.
	```js
	{ test: /\.css$/, use: 'css-loader' },
	```

- https://webpack.js.org/loaders/
  - 웹팩에서 사용가능한 로더들의 리스트. 각 로더에 대한 사용법과 github 저장소의 링크를 제공한다.

#### loader 적용순서는 오른쪽->왼쪽
```js
module: {
  rules: [
    {
      test: /\.scss$/,
      use: ['style-loader', 'css-loader', 'sass-loader']
    }
  ]
}
```
1. 위에 코드는 `.scss`파일들을 찾아 sass-loader로 전처리하여 css로 변환하고
2. css-loader로 `.css`파일들을 웹팩에서 인식하게 해준다.
  - 난독화 되지않은 build 결과물을 보게되면, css코드로 보이는 문자열이 js내부에 존재하는걸 알수 있다.
  - js가 애플리케이션이 작동할 때 동적으로 html 내부에 style태그로 삽입해주는 것이다. 이런 이유로 index.html을 보면 head내부는 스타일태그가 없이 깨끗하다.
3. style-loader는 인식된 css를 js로 동적으로 로드할 수 있게끔 도와준다.

```js
module: {
  rules: [
    {
      test: /\.scss$/,
      use: ['css-loader', 'sass-loader', 'style-loader']
    }
  ]
}
```
위와 같이 설정하고 빌드를 시도하가 되면 아래 오류가 발생한다.
```sh
ERROR in ./src/base.css
Module build failed (from ./node_modules/mini-css-extract-plugin/dist/loader.js):
ModuleParseError: Module parse failed: Unexpected token (1:2)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
> p {
|       color: blue;
| }
```
- 이처럼 순서가 사용되는 로더의 순서가 제대로 배치되지 않으면 웹팩에서는 오류가 발생한다.
  - `.scss`, `.sass` 파일을 `.css`로 변환하기 위해서는 sass-loader가 선행되어야함. 그 이후 css-loader로 인식하게 한뒤 style-loader로 js내부에 css를 담도록 해야함

- 내가 사용하려는 파일에 따라 로더의 종류, 로더를 입력하는 순서가 달라진다.

### mode
- 웹팩실행시 어떤 환경을 위해 빌드하는지에 대한 구분자
- 웹팩에서 제공하는 기본값들은 `none`, `development`, `production`이 있다.

### plugins
- 웹팩의 동작에 추가적인 기능을 추가할 수 있는 기능
> loader: 파일을 해석하고 변환함 <br /> plugins: 결과물의 형태를 바꿈
- 플러그인 라이브러리의 instance를 배열형태로 받는다.
```js
// webpack.config.js
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin(),
    new webpack.ProgressPlugin()
  ]
}
```
