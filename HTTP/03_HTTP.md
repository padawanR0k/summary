## HTTP

### 사용되는곳
- HTML, TEXT 존달
- IMAGE, 음성, 영상, 파일
- JSON, XML
- 서버간의 데이터 전달
- 거의 모든곳에서 사용되고 있음

### 기반 프로토콜
- TCP: HTTP/1.1, HTTP/2
- UDP: HTTP/3
- 현재는 HTTP/1.1 주로 사용함

### HTTP 특징
- 클라이언트-서버 구조
- 무상태(stateless) 프로토콜, 비연결성
- HTTP 메세지
- 단순하면서 확장가능한 메세지

### 클라이언트 - 서버 구조
- Request Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기한다.
- 비즈니스 로직은 서버에만 존재하고 UI를 그리기위한 로직은 클라이언트상에만 존재한다.
	- 각각 아키텍쳐가 독립적으로 성장할 수 있다.
	- 사용자가 급증하게되면? 수정할 부분은 서버쪽만 변경하면됨
	-> 관심사의 분리의 이점

### 무상태 프로토콜
- 장점
	- 서버 확장성 높음
		- 클라이언트 요청에 따라 서버를 대거 투입할 수 있음. (서버에서는 관리할 상태가 없기때문에)
- 단점
	-	클라이언트가 추가로 데이터 전송해야함
		- 프로세스를 진행하는데 여러 단계가 있을 때, 클라이언트단에서 각 단계에 대한 정보를 가지고 있다가 한번에 전송해야한다. (서버에는 상태를 저장하고 있지 않기 때문에)
		- 이 때문에, 클라이언트(웹페이지에서는 가면 갈수록 많은 상태데이터를 관리하다보니까 상태관리 프레임워크나 라이브러리들이 발전된듯하다.)
- 한계
	- 상태 유지 못함
		- 로그인 세션에 대한 정보를 유저의 브라우저(쿠키)나 DB에 저장해야함

### 비연결성
- N개의 클라이언트 - 서버가 자원을 주고받을 때
	- 연결을 유지
		- N개의 연결이 지속적으로 되어있어야함 -> 자원을 많이 씀
	- 비연결
		- 1개에 요청을 주고받는 그 당시에만 유지함 -> 자원을 덜 씀
- HTTP는 기본이 비연결 모델
- 서버자원을 매우 효율적으로 사용가능
- 한계
	- TCP/IP 새로운 연결 필요 -> 3 way handshake 시간이 추가됨
	- 대부분의 사이트는 많은 에셋(css, media 등) 로드를 필요로함 -> 많은 연결과 종료가 수반됨
		- 극복하기위해 HTTP 지속연결 유지 (Persistent Connections)


### HTTP 메세지

#### 요청 메세지 (Request)
- 구조
	1. HTTP-METHOD URL HTTP-VERSION CRLF(엔터)
		- HTTP-METHOD
			- 요청시 어떤 요청을 할것인지에 대한 정의 (GET, POST, ELETE, PUT 등)
		- URL
			- 요청의 목적지
		- HTTP-VERSION
			- HTTP 프로토콜중 어떤 프로토콜을 쓸것인지에 대한 정의
	2. body
		- 요청시 같이 보낼 정보 (일부 HTTP-METHOD는 사용 불가)
#### 응답 메세지 (Response)
- 구조
	1. HTTP-VERSION Status-code reason-phrase
		- Status-code
			- 응답에 대한 결과를 코드로 나타낸것. [각 코드의 의미](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
		- reason-phrase
			- 사람이 이해하기 쉬운 영어단어로 되어있는 응답에 대한 설명
	2. HTTP Header
		- field-name: field-value 형식
		- HTTP 전송에 필요한 모든 부가정보가 들어간다.
			- 압축여부, body의 크기, 인증, 클라이언트 정보, 서버 정보, 캐시 정보 등
		- 임의로 개발자가 헤더를 추가 가능

	3. body
		- 실제 전송할 데이터
		- HTML, 미디어, JSON 모든 데이터를 보낼수있다.


### HTTP 실무
- 회원관리하는 API를 만드는 경우
	- 리소스를 식별하고, API URI 설계시 상황에 맞는 METHOD를 사용해서 최소한의 URL로 짜라
		- 요구사항에 대해 행위와 리소스를 분리하라; 리소스(명사) 행위(동사)
	- 예시
		- 회원목록 - GET /members
		- 회원조회 - GET /member/{id}
		- 회원등록 - POST /member/{id}
		- 회원수정 - PUT /member/{id}
		- 회원삭제 - DELETE /member/{id}
### HTTP 메서드 종류
- GET
	- 리소스 조회
	- 서버에 데이터 전달시 query를 통해서 전달
- POST
	- 요청 데이터 처리 (주로 새로운 데이터 생성에 사용)
	- body를 통해 데이터 전달
	- 주로 신규 리소스등록, 프로세스 처리에 사용
	- GET메서드를 사용해야하는 상황인데, JSON같은 큰 데이터를 전달해야하는 경우 대신 POST를 쓰기도함
- PUT
	- 리소스를 기존것에 덮어쓴다(대체), 리소스가 없으면 생성
	- ex. 당근마켓에서 [내 위치 인증]을 진행할 때 내 위치는 1개만 존재해야하니까 PUT을 사용할듯하다.
- PATCH
	- 리소스를 일부 수정시 사용
	- ex. 내 개인정보 수정시, 닉네임만 변경하는 경우
- DELETE
	- 리소스 삭제시 사용
- HEAD
	- GET메서드에서 상태줄과 헤더만 반환
- OPTIONS
	- 대상 리소스에 대한 통신가능 옵션을 설명(주로 CORS에 사용)
- CONNECT
	- 대상 자원으로 식별되는 서버에 대한 터널을 설정
- TRACE
	- 대상 리소스에 대한 경로를 따라 메세지 루프백 테스트 수행

### HTTP 메서드의 속성
- 안전
	- 호출을 여러번해도 리소스를 변경하지 않음 = 변경을 일으키는 메서드는 안전 X
- 멱등
	- f(f(x)) = f(x)
	- 1번 호출결과 == N번 호출결과
	- GET, PUT, DELETE
	- 멱등성이 있는 메서드는 여러번 호출해도 같은 결과를 유지하기 때문에, 자동복구 메커니즘에도 사용된다.
- 캐시가능
	- GET, HEAD, POST, PATCH 캐시가능
	- GET, HEAD 정도만 캐시로 사용
	- 실시간 위치를 보여주는 기능해는 캐시를 하면 안됨. 자주 바뀌지 않는 데이터를 불러올 때 사용하면 좋을듯하다. ex) 공지사항 게시판


### HTTP API 설계 예시
- 회원관리 시스템 개발시 (POST 기반 등록)
	- 목록 GET - /members
	- 등록 POST - /members
	- 등록 GET - /members/{id}
	- 등록 PATCH, PUT, POST - /members/{id}
	- 등록 DELETE - /members/{id}
	- *컬렉션*
		> 서버가 관리하는 리소스 디렉토리
		서버가 리소스의 URI를 생성, 관리
		- 회원 신규등록
			- POST
				- 신규 데이터가 어디에 생성되었는지는 서버가 알게된다.(생성된 row의 pk가 무엇인지)
				- 클라이언트는 등록될 리소스의 URI를 몰라도될 경우 사용

- 파일관리 시스템 개발시 (PUT 기반 등록)
	- 목록 GET - /files
	- 상세 GET - /files/{filename}
	- 등록 PUT - /files/{filename}
		- POST를 안쓴이유?
			- 파일의 이름이 URI -> 클라이언트가 등록될 URI를 알고 있음
	- 삭제 DELETE - /files/{filename}
	- 대량등록 POST - /files

	- *스토어*
		- 클라이언트가 관리하는 리소스 저장소
		- 클라이언트가 리소스의 URI를 알고 관리

#### URI 설계 개념정리
- 문서 (document)
	- 단일 개념
	- `/member/100`, `/member/star.jpg`
- 컬랙션 (collection)
	- 서버가 관리하는 리소스 디렉터리
- 스토어 (store)
	- 클라이언트가 관리하는 리소스 디렉터리
- 컨트롤러 (controller)
	- 문서, 컬렉션, 스토어로 해결하기 어려운 경우 추가 프로세스 실행
	- 단순히 리소스를 CRUD하는게 아닌 비즈니스 로직이 추가되는 경우에 controller를 많이 사용하게 될듯하다. 왜냐하면 비즈니스로직은 많은 데이터를 유저에게 받기 어렵디 때문에 적은 데이터로 보다 큰 데이터를 만들어내서 DB에 저장하기 때문이다.