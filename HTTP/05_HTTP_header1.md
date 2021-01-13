## HTTP 헤더
- HTTP 헤더는 키와 값으로 이루어져있다.
- HTTP전송에 필요한 모든 부가정보를 가지고 있다.

### HTTP 헤더 (1999년 RFC2616)
- RFC2616
- 헤더 분류
	- General 헤더
		- 메세지 전체에 적용 ex) 연결여부
	- Request 헤더
		- 요청정보 ex) user-agent
	- Response 헤더
		- 응답 정보 ex)server: apache
	- Entitiy 헤더
		- 엔티티 정보 ex) content-type: text/html

### HTTP BODY (1999년 RFC2616)
- 엔티티 본문을 전달하는데 사용함
	- 요청이나 응답에서 전달할 실제 데이터
- 엔티티 헤더는 엔티티 본문을 해석할 수 있는 정보를 제공한다
	- ex) content-type이 json인지 html인지에 대한 정보 등
		> 실무를 하면서 S3에 업로드된 이미지주소를 클릭하면 열리는것이아닌 다운로드되는 현상이 있었다. 구글링해보니 content-type이 octet-stream인 부분때문에 발생한 문제였다. 해당 헤더의 값으로 인해 브라우저가 응답을 받아들일 때 이미지를 보여주지않고 다운로드 했었던 것이다.

### 2014년 RFC7230~7235 변화
- 엔티티 (Entity) -> 표현 (Representaion)
- 표현 = 표현 + 표현 메타데이터

### HTTP BODY (2014년 RFC7230)
- 메시지 본문에 표현데이터 전달
- 메세지 본문 = payload
- 표현 = 요청이나 응답에서 전달되는 실제 데이터

### 표현 (representation)
- HTTP BODY의 데이터를 어떤 형식으로 전달할 것인가
- Content-Type
	- 데이터 형식
		- text/html; cahrset=utf-8
		- application/json
		- octet-stream/image
		- image/png
- Content-Encoding
	- 압축방식
		- gzip
		- deflate
		- identity
- Content-Language
	- 자연 언어중 어떤건지
		- ko
		- en
		- en-US
- Content-Length
	- 데이터  길이
- res,req 둘다 사용

### 협상 (contents negotiation)
- 클라이언트가 선호하는 표현 요청
- Accept
- Accept-Charset
- Accept-Encoding
- Accept-Language
	- 한국어 브라우저를 사용하는 유저가 외국 쇼핑몰 이벤트 사이트를 접속했을 때, 유저의 헤더에 `Accept-Language: ko`라는 값이 있는 경우, 서버는 한국어로 번역된 페이지를 전달할 수 있다. (아마존)
	- 이 때, 문제! 만약 기본값이 독일어인 웹사이트가 있다면? -> *우선순위로 해결가능*

#### 협상과 우선순위1
- Quality Values(q)값 사용
- q: 0~1사이의 숫자
- `Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7`
	- 한국어: 0.9점
	- 영어: 0.8점
	- 독일어: 0.7점
	-> 지원하는 언어는 영어, 독일어이고 기본값이 독일어여도 선호점수에 의해  해당 유저에게 영어를 보여주게된다.
	-> i18n도 이와 같은 헤더들을 사용하여 언어들을 구별하는게 아닐까 생각된다.

#### 협상과 우선순위2
- 더 구체적인것이 우선하여 적용된다. (문자열 일치)
- `Accept: text/*, text/plain, text/plain;format=flowed, */*`
	1. text/plain;format=flowed
	2. text/plain
	3. text/*
	4. */*
- `Accept: text/*;q=0.3, text/plain;q=0.7, text/plain;level=1, text/html;level=2;q=0.4, */*;q=0.5`
	- 이런식으로 문자열 일치에 대한 부분에 점수를 부여하는것도 가능하다.

#### 전송 방식
- 단축 전송
	- Content의 길이를 알 때, 길이를 그대로 응답헤더에 보낸다.
- 압축 전송
	- Content를 압축알고리즘으로 압축하여, 압축 방식을 Content-Encoding 값을 같이 보낸다.
- 분할 전송
	- Transfer-Encoding: chunked
	- 분할 전송의 경우 content-length를 보내면안됨!
	- 서버가 특정단위로 잘라서 보낸다. 모든 데이터를 다 보낸경우 0값을 보낸다
		- youtube같은 스트리밍??
- 범위 전송
	- Content-Range: bytes 1001-2000/2000
	- 분할과 다르게 범위를 지정해놓고 보내는 방법

#### 일반 정보
- Form
	- user-agent의 이메일정보
	- 검색엔진 같은곳에서 사용
- Referer
	- 아주 많이 쓰이는 헤더!
	- 현재 웹페이지에 들어오기 전에 이전페이지의 주소
	- GA처럼 유저 유입경로를 트래킹을 하기 위한 곳에서 많이 사용한다. (마케팅에 중요하다. 어떤 광고매체가 가장 좋은 효과를 내는지 알수가 있으니)
	- referer은 referrer의 오타이다.
- User-Agent
	- 유저가 사용하는 브라우저나 OS의 정보
	- 해당 부분으로 유저들이 브라우저를 많이 쓰는지 알수 있다 .
	- 에러 발생시, 로그가 있다면 브라우저를 특정할 수 있다.
- Date
	- 응당이 발생한 시각

#### 특별한 헤더 정보
- Host
	- 요청에서 사용
	- 필수값임!
	- 예시
		- 가상 호스트를 사용하면 1개의 서버에서 여러 domain을 가질 수 있다.
		- 이 때, 해당 서버에 요청한 경우 어느 domain으로 보내야할지 알수 없다. 이를 해결하기위해 사용한다. ex) `Host: aaa.com`
- Location
	- 리다이렉션
	- Status code가 3xx의 경우, Location헤더를 통해 자동으로 리다이렉트를 시킨다.
- Allow
	- 허용 가능한 HTTP 메서드
	- 405(Method Not Allowed) 코드를 응답에 보낸 경우 같이 보내야한다.
- Retry-After
	- 서비스가 불능인 경우 언제까지 불능인지 알려줄수있음
	- 초단위 또는 날짜를 표시한다.

#### 인증 헤더
- Authorization
	- 클라이언트 인증정보를 서버에 전달
	- `Authorization: Basic xxx`
	- 키를 발급받아 사용하는 기업들의 open API나, 세션이 필요한 API 사용시 자주 사용한다.
	- 클라이언트가 인증에 실패한 경우 서버는 클라이언트에게 유효한 인증방법을 안내해야한다. ex) `WWW-Authenticate: Newauth realm="apps", type=1, title="Login to \"apps\"", Basic realm="simple"`

#### 쿠키
- Stateless의 대안
- 시나리오
	- A유저가 호텔예약사이트에 접속한다
	- 사이트에서는 A유저의 브라우저에 Set-Cookie를 통해 쿠키를 저장한다.
	- 앞으로 A유저가 호텔예약사이트에 요청을 보낼 때 항상 쿠키가 같이 전달되므로 사이트에서는 이 사람이 처음들어오는 유저인지 아닌지 판단할 수 있다. 실제로 이런식으로 가격대를 조절함
- Cookie: 클라이언트가 쿠키 저장
- Set-Cookie: 서버 -> 클라이언트 전달
- 사용처
	- 로그인 세션 관리 (로그인 성공시 서버에서 토큰을 발급하여 그값을 쿠키로 유저에게 전달한다)
- 보안에 민감한 데이터는 저장하면 절대안된다.
- 쿠키 관련 옵션
	- 생명주기
		- expires
			- 만료일을 지정하여 쿠키를 만료시킴
		- max-age
			- 초단위로 지정하여 쿠키를 만료시킴
		- 세션 쿠키
			- 브라우저 종료시 만료
	- 도메인
		> google.com에서 접속했다고 가정하자
		- 옵션을 명시한 경우
			- map.google.com, google.com 두 곳에서 쿠키 사용가능
		- 생략한 경우
			- google.com에서만 쿠키 사용가능
	- 경로
		- ex) `path=/home`
		- 경로를 포함한 하위 경로페이지만 쿠키 접근 가능
		- 일반적으로는 `path=/`을 사용함
	- 보안
		- Secure
			- 적용시 https 프로토콜인 경우에만 전송
		- HttpOnly
			- XSS 공격방지
			- `document.cookie`로 접근이 불가능함
		- SameSite
			- XSRF공격 방지
			- 요청 도메인과 쿠키에 설정된 도메인이 같을 때만 쿠키 허용