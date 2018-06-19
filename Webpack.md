# 웹팩이란?

웹팩은 웹을 제작할때 여러 파일로 나누어서 제작된 파일들을 합쳐준다. (HTML, CSS, JS, 웹폰트, 이미지, Json 데이터 등)

왜냐?  여러 파일을 여러번 http 요청을 보내는것보다는 한번의 큰용량의 파일을 요청하는것이 더 효율적이기 떼문이다. 이런 효율성때문에 간단한 이미지들은 스프라이트로 만들어 한번의 요청으로 받고, 파편화된 CSS, JS는 Gulp, Grunt 같은 번들러로 하나로 묶어내는 작업을 하는것이다. 

웹팩은 이런 번들역할을 하는데에 있어 게임체인저였다. 하나로 합쳐주면서 크로스 브라우징 대응도 해주고 압축도 해주는등 여러모로 좋은 점이 많았다.

![webpack](https://cdn.filepicker.io/api/file/QIuZVivBTFWIu8LN9i3E) 

 최근 JS는 모듈개념이 도입되면서 `import`나 `require`로 서로 의존할 수 있다. 이런 것들을 하나의 JS파일로 합쳐준다. 하나의 파일로 합치기엔 너무 클때는 역할에 따라 나누어서 JS파일을 생성할 수 도 있다.

1. 라이브러리 JS
2. 수정이 잦은 JS



## 사용법

```npm
npm i -g webpack webpack-cli && npm i -D webpack webpack-cli
```

글로벌과 개발환경에도 설치하자 웹팩4부터는 webpack-cli까지 같이 설치해야 webpack이란 명령어를 같이 쓸 수 있게 되었다.



`package.json`이 있는 경로에 `webpack.config.js` 파일을 생성하자. 만약 다른이름으로 설정파일을 만들고싶다면?

```npm
 webpack --config 파일명 // --config 플래그를 사용하면된다.
```



## `webpack.config.js` 내부

```javascript
const webpack = require('webpack');
module.exports = {
  mode: 'development',
  entry: {
    app: '',
  },
  output: {
    path: '',
    filename: '',
    publicPath: '',
  },
  module: {

  },
  plugins: [],
  optimization: {},
  resolve: {
    modules: ['node_modules'],
    extensions: ['.js', '.json', '.jsx', '.css'],
  },
};
```



### mode

웹팩4 버전부터 추가되었다. mode의 값이 development면 개발용, production이면 베포용이다. 배포용일 경우는  최적화되어 컴파일한다. 

웹팩3버전이라면 config파일에서 mode랑 optimization을 빼야한다.



### entry

```javascript
entry: {
    app: '파일 경로',
    zero: '파일 경로',
}	
```

웹팩이 파일을 읽기 시작하는 부분이다. 객체 내부의 key값`app`이 결과물이되고  value값`파일경로`이  하나로 뭉칠 파일들을 뜻한다.



```javascript
entry: {
  app: ['a.js', 'b.js'],
},
```

위의 경우는 `a.js`와 `b.js`를 하나로 묶어 `app.js`로 만들어준다. 웹팩은 entry의 js파일부터 import, require 관계로 의존되어있는 다른 js파일까지 알아서 파악하여 entry의 키의 개수만큼 묶어준다.

npm모듈도 기재할 수 있다. 

```javascript

  entry: {
    vendor: ['@babel/polyfill', 'eventsource-polyfill', 'react', 'react-dom'],
    app: ['@babel/polyfill', 'eventsource-polyfill', './client.js'],
  }
```

이렇게 하면 각각의 엔트리가 polyfill들이 적용된 상태로 결과물이 나온다. 최신 JS문법을 사용하면서 IE를 지원하려면 `@babel/polyfill', 'eventsource-polyfill` npm에서 다운받은 후 저렇게 엔트리에 넣어주어야한다.



### output

 ```javascript
output: {
    path: '/dist',
    filename: '[name].js',
    publicPath: '/',
  },
 ```

`path` : 결과물이 저장될 경로

`publicPath`: 파일들이 위치 할 서버상의 경로

`filename`: 파일들이 저장될 형식 ex) entry프로퍼티의  app, vendor가 `app.js`, `vendor.js` 로 나오게끔 해준다.

`hash`: 웹팩 컴파일 시 매번 랜덤한 문자열을 부여한다. 캐시삭제시에 유용함

`chunkhash`: 파일이 달라질때만 랜덤한 문자열을 부여한다. 변경되지 않은 파일들은 계속 캐싱하고 변경된 파일만 새로 불러온다.



### loader

ES2015이상 문법을 IE구형브라우저에 호환시키기위해 babel을 웹팩과 같이 사용한다. 또는  jsx문법을 컴파일 하려는 목적일 수 도있다.

```npm
npm i -D babel-loader @babel/core @babel/preset-env
```

loader와 core는 필수로 받아야하고 나머지는 프리셋이다.

env는 브라우저에 필요한 ecmascript 버전을 파악해서 polyfill을 넣어준다.

```javascript
{
  module: {
    rules: [{
      test: /\.jsx?$/,
      loader: 'babel-loader',
      options: {
        presets: [
          [
            '@babel/preset-env', {
              targets: { node: 'current' }, // 노드일 경우만
              modules: 'false'
            }
          ]
        ],
      },
      exclude: ['/node_modules'],
    }],
  },
}
```

`@babel/preset-env`의 `module` 값이 false로해야 트리 쉐이킹이 된다. (import되지않은 export를 정리해주는 기능)

`rules` 의 정규표현식에 부합하는 파일들을 loader에 지정한 로더(`babel-loader`)가 컴파일 해준다.

`options` : 적용할 프리셋들의 목록

`exclude`는 바벨로 컴파일을 제외할 것들을 지정주는 옵션

`include`: 꼭 이 로더를 사용해서 컴파일할 것들을 지정해주는 옵션



###plugin

부가적인 기능으로 다양한 플러그인을 적용시켜 효과적으로 번들링할 수 있게 도와준다. 압축을 한다거나  핫리로딩, 파일을 복사하는등 부수적인 작업을 할 수 있게해준다.

```javascript
{
  plugins: [
    new webpack.LoaderOptionsPlugin({
      minimize: true,
    }),
    new webpack.EnvironmentPlugin(['NODE_ENV']), 
  ],
}
```



### optimization

웹팩4부터 최적환관련 플러그인들이 이 속성으로 통합되었다.

```javascript
{
  optimization: {
    minimize: true/false, // == UglifyJsPlugin
    splitChunks: {},	// == CommonsChunkPlugin
    concatenateModules: true, // == ModuleConcatenationPlugin
  }
}
```

만약 `webpack.config.js`의 mode가 `production` 이라면 `minimize`와 `splitChunks`옵션은 자동으로 켜진다.



# 참고한 글들

[웹팩 설정하기](https://www.zerocho.com/category/Webpack/post/58aa916d745ca90018e5301d)