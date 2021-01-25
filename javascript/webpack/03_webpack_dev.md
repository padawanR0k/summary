## [웹팩 데브 서버](https://webpack.js.org/configuration/dev-server/)
프론트엔드 개발시, 대부분의 개발이 react, angular, vue 등을 사용하여 진행된다. 각 도구들은 각자의 문법을 가지고 있고 react와 vue같은 경우는 파일확장자명도 다르다. (.jsx, .tsx, .vue) 이를 브라우저에서 실행시키기 위해서는 브라우저가 이해할 수 있는 html,css,js로 변경해야 한다. 하지만 개발할 때 코드 변경후 매번 저장하고 다시 빌드명령어를 치는건 비효율적이다. 웹팩 데브서버는 이런 부분을 해결해주고 추가적으로 로컬개발시 편의성을 제공해준다. (타입스크립트를 사용할 때 파일이 변경된걸 감지하고 매번 자동으로 트랜스파일을 해주는 tsc-watch와 비슷한 맥락)

- 설치
	```shell
	npm i webpack webpack-cli webpack-dev-server  -D
	```
- package.json에 명령어 등록
	```json
	"scripts": {
			"test": "echo \"Error: no test specified\" && exit 1",
			"dev": "webpack serve"
		},
	```
- 실행
	```shell
	npm run dev
	```

### 특징
- 데브서버로 빌드된 내용은 메모리상으로만 존재하고 파일시스템 상에는 존재하지 않는다. (메모리상에 적재하는 것이 파일시스템상에서 파일 입출력을 하는것 보다 빠르기 때문이다.)
- 파일을 저장할 때 마다 새로 빌드해서 최신코드를 반영해줌