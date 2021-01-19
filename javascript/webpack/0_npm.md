[참고](https://joshua1988.github.io/webpack-guide/tutorials/code-splitting.html#%EC%8B%A4%EC%8A%B5-%EC%A0%88%EC%B0%A8)

## npm (Node Package Manager)
- Nodejs로 javascript 패키지들을 관리할 수 있게 도와줌. 또 많이 쓰이는 예로는 yarn이 있음.

### 패키지 매니저는 왜 쓰는가?
- 패키지 관리시 편리함 (사용하는 버전, 패키지 목록)
- 타 개발자와 공유시 편리함. `node_modules`폴더는 크기가 커서 git에 포함하지 않는다. 개발자들은 패키지들의 목록이 들어있는 package.json만 공유하고 패키지 매니저를 사용하면 된다.

### 설치, 삭제 명령어 (npm, yarn)

#### [npm](https://www.npmjs.com/)
- 패키지 설치
	- `npm install <이름>` or `npm i <이름>`
		- 설치완료시, package.json의 `dependencies`에 추가됨
		- `--save-dev` or `-D`
			- 프로젝트 폴더의 package.json `devDependencies`에 추가됨
		- `--global`
			- 전역에 설치하는 옵션 (sudo 권한을 요구할 수도 있다)
			- 프로젝트 폴더의 package.json `dependencies`에 추가되지 않으며, pc 전역에서 사용할 수 있게끔 설치된다
				```sh
				# window
				%USERPROFILE%\AppData\Roaming\npm\node_modules

				# mac
				/usr/local/lib/node_modules
				```
	- `dependencies` | `devDependencies` 차이
		- 결과물에 직접 들어가서 사용되는 라이브러리 -> `dependencies`
			- vue, react, momentjs 등
		- 개발시에 필요한 라이브러리 -> `devDependencies`
			- webpack, node-sass, babel 등
- 라이브러리 삭제
	- `npm uninstall <이름>`

#### [yarn](https://classic.yarnpkg.com/en/docs/cli/)
- 패키지 설치
	- `yarn add <이름>`
		- 설치완료시, package.json의 `dependencies`에 추가됨
		- `-D`
			- 프로젝트 폴더의 package.json `devDependencies`에 추가됨
	- `yarn global add`
		- 전역에 설치하는 명령어 (sudo 권한을 요구할 수도 있다)
		- 프로젝트 폴더의 package.json `dependencies`에 추가되지 않으며, pc 전역에서 사용할 수 있게끔 설치된다
- 라이브러리 삭제
	- `yarn remove <이름>`