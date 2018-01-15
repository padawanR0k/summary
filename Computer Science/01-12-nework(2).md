# Web Programming

## 웹 개발의 변화
>**1991~1999** : 정적인 컨텐츠를 중심으로 한 웹기술이 발달

	각각의 .html 주소에 html파일이 전부 필요함
    
> **1999~2009** : Linux, Apache, Mysql, Php 를 중심으로 동적인서버, 정적인 클라이언트모델이 지속

	하나의 템플릿만 있으면 됨.
    
 > **2009 ~ 현재** : javascript 동적인  웹


## Web Browser

### 브라우저의 연대기
- Netscape (1994) - Internet Explorer(1995) - Netscape가 망하면서 FireFox(2004) - Chrome(2008)

### PWA (Progressive Web Apps)
- 구글 크롬 엔지니어 알렉스 러셀이 2015년에 고안한 개념으로 웹을 앱처럼 사용 할 수 있게 한다.
- 웹보다 안정적이고 빠르다. 앱처럼 푸쉬도 가능하다
- 진화 : 점진적인 개선을 통해 작성되므로, 어떤 브라우저를 선택하든 상관없이 모든 사용자에게 적합하다
- 반응형 : 데스크톱, 태블릿, 모바일 등 모든 폼팩터에 맞다
- 연결 독립적 : 오프라인이나 느린 네트워크에서 작동(로컬 기기의 캐시에서 로드)
- 앱과 유사 : 앱 스타일의 상호작용 및 탐색 기능을 사용자에게 제공
- 안전 : HTTPS를 통해 제공됨
- 검색 가능
- 재참여 가능 : 푸시 알림 같은 기능 가능
- 설치 가능 : 앱 스토어 등록이 필요 없음
- 링크 연결 가능 : URL을 통해 손쉽게 공유할 수 있음

더 알아보기 http://www.bloter.net/archives/274549

### AMP ( Accelerated Mobile Pages )

> Accelerated Mobile Pages는 HTML 페이지로서 다양한 기술적 접근방식을 활용하여 속도를 최우선순위로 삼고 콘텐츠를 거의 즉시 로드함으로써 사용자에게 더욱 빠른 사용 환경을 제공한다.
- 구글 검색결과에 상위에 더 많이 노출되게 된다. 

### Web architecture
![아키텍쳐](https://camo.githubusercontent.com/5187497eca25548e4f964a7ac62af8d827e08866/687474703a2f2f7777772e7475746f7269616c73706f696e742e636f6d2f6e6f64656a732f696d616765732f7765625f6172636869746563747572652e6a7067)

### Client-side
- HTML/CSS, javaScript
- jQuery, AJAX
- Front-end Web Framework
	- AngularJS
	- React.js
	- Vue.js
- CSS Framework
	- Bootstrap
	- Foundation
>  Framework : 소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것. 기본적으로 구성하고있는 뼈대를 말한다고 보면됨.

### Server-sdie
- 다루는 언어마다  전부다르다
	- PHP: Laravel
	- javaScript: Node.js(Express.js)
	- Java: Spring
	- Python: Django, Flask
	- Golang: itself
	- Ruby: Ruby on Rails
## Database
- RDBMS : 엑셀과 비슷하다.
	- MySQL
	- PostgreSQL
	- MariaDB
- noSQL : 한장 한장 데이터의 구조가 다르다.
	- MongoDB
	- CouchDB
	- Redis


## URI, URL ★

### URI
	https://www.example.com/post/how-to-make-url
전송방식 + 파일경로 + 파일이름

### URL
	https://www.example.com/post/
전송방식 + 파일경로


## API

- 응용프로그램에서 사용할수 있도록 운영체제나 프로그래밍언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스
- 사용자와 커널을 이어주는 도구
### Web API
> 웹서버,웹브라우저를 위한 API
> 
### REST API 
- URI 와 HTTP method를 이용해서 정보를 다루는 인터페이스
- 리소스 중심 API 명세(URI를 읽는 것으로 이해 가능)
- 클라이언트의 상태를 신경쓰지 않음

### REST 장단점 ★
장점
- 스펙없이 기존의 HTTP를 이용해서 처리함

단점
- 표준이 없음
- 메소드가 4개뿐이다.

## GraphQL
- 페이스북이 만든 오픈소스
- URI 경로에 안들어남.
![](https://camo.githubusercontent.com/40a49871253d2c085676b34983c9716a4ea7fded/68747470733a2f2f646162316e6d736c76766e74702e636c6f756466726f6e742e6e65742f77702d636f6e74656e742f75706c6f6164732f323031372f30352f313439343235373438333030332d4772617068514c5365727665722e706e67)