## URI
URI는 locator, name 또는 둘다 추가로 분류될 수 있다.
- URI
	- 하위 개념
		- URL (Resource Locator): 리소스의 위치
		- URN (Resource Name): 리소스의 이름
	- 뜻
		- Uniform: 리소스 식별 방식
		- Resource: 자원, URI로 식별할수있는 모든 것
		- Identifier: 다른 항목과 구분시 필요한 정보
- 구조
	- scheme://[userinfo@]host[:port][/path][?query][#fragment]
	- port는 생략가능, http는 기본포트가 80, https는 443

## 웹 요청 흐름

- 전달정보
	- 메소드 정보
	- URL

- 전달순서
	1. 웹 브라우저가 HTTP 메세지 생성
	2. 3-way handshake 성공시 메세지 전달준비
	3. SOCKET 라이브러리를 통해 전달
		- TCP/IP 패킷내부에 IP와 PORT 정보를 가지고
	4. 전달받은 서버에서는 HTTP 메세지를 분석하여 알맞은 결과 메세지를 클라이언트에겍 HTTP 응답 메세지를 전달함.
		- 이 때, 응답 헤더에는 컨텐츠의 타입, 크기 등이 존재함
	5. 응답메세지를 받은 클라이언트의 브라우저는 메세지를 분석하여 유저에게 렌더링해줌
