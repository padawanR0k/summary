## 웹팩 설정 파일 분석해보기
```js
var path = require('path')
var webpack = require('webpack')

module.exports = {
  mode: 'production', // 해당 웹팩 설정파일은 배포를 위한 모드이다.
  entry: './src/main.js', // 웹팩이 빌드를 시도할 때, 첫 진입점 파일을 의미한다.
  output: {
    path: path.resolve(__dirname, './dist'), // 빌드의 결과물이 저장될 폴더명
    publicPath: '/dist/', // 1) 아래 작성
    filename: 'build.js' // 빌드 결과물의 이름
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ],
			},
			// .css 확장자를 가진 파일 모두, css-loader, vue-style-loader를 적용시킨다.

      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
          }
          // other vue-loader options go here
        }
			},
			// .vue 확장자를 가진 파일 모두, vue-loader를 적용시킨다.

      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
			},
			// .js 확장자를 가진 파일 모두, babel-loader를 적용시킨다. 단, node_modules 폴더는 제외시킨다.

      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
			}
			// .png|jpg|gif|svg 확장자를 가진 파일 모두, file-loader를 적용시킨다. 이때 파일명 뒤에 해쉬를 붙인다. (이미지가 캐싱되어도 해쉬값이 변경되면 이미지 업데이트시 다시 새로운 이미지를 불러오게하기 위함)

    ]
	},

  resolve: {
		// 2
    alias: {
			'vue$': 'vue/dist/vue.esm.js'
    },
		// 3
    extensions: ['*', '.js', '.vue', '.json']
  },

	// 데브 서버 실행시, 옵션지정 참고 - https://webpack.js.org/configuration/dev-server/
	devServer: {
    historyApiFallback: true,
    noInfo: true,
    overlay: true
	},

	// 빌드결과물 크기에 대한 경고를 띄울수있는 옵션 - https://webpack.js.org/configuration/performance/
  performance: {
    hints: false
	},

	// 소스매핑스타일에 대한 옵션 (빌드에 대한 소요시간에 영향을 끼칠 수도 있다) - https://webpack.js.org/configuration/devtool/
  devtool: '#eval-source-map'
}
```

### 모르는 부분 검색결과
1. `output.publicPath`
	- `path`와는 다르게 빌드된 결과물이 배포되었을 때 해당 파일이 존재할 디렉토리를 의미한다.
	- `publicPath: "/dist"` 지정 여부에 따른 결과
		```html
		<script src="/bundle.js"></script></body>
		<script src="/dist/bundle.js"></script></body>
		```
		- https://example.com/bundle.js
		- https://example.com/dist/bundle.js
	- 이런것도 가능하다.
		- aws S3, cloudefront, route53를 사용한다고  가정함
		- `package.json` 내부에 version 변수를 js로 불러온 후 `publicPath`로 지정한다. 그 후 aws s3에 빌드파일을 S3에 업로드할 때 public
	- 참고
	  - [Public Path](https://webpack.js.org/guides/public-path/)
		- [What does “publicPath” in Webpack do?](https://stackoverflow.com/questions/28846814/what-does-publicpath-in-webpack-do)
2. `resolve.alias`
	- 프로젝트 내부에서 사용할 별칭에 대해 실제 엔티티를 매칭해준다.
		- `'vue$': 'vue/dist/vue.esm.js'`
			```js
			import Something from  './component/some' // some.js some.json 위에서 등록한 확장자는 생략가능 (여기서 * 설정값은 모든 파일 확장자를 뜻함)
			```
3. `resolve.extensions`
	- 프로젝트 내부에서 사용할 모듈에 대해 import시 확장자를 붙이지 안항도 된다.
		- `['*', '.js', '.vue', '.json']`