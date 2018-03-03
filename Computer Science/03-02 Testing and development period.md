# 개발, 배포주기 

**pre-alpha**

- 가장 유동적인 단계로서 요구사항 분석, 설계, 개발, 유닛 테스트( ex)로그인시 sql을 보내려는 시도 차단)를 포함
- 핵심기능이 시작한 상태



**Alpha**

- 핵심기능이 어느정도 완성되었을때 **내부인력으로** 테스트를 하는 단계

	​	

**Beta**

- 외부에 직,간접적으로 테스트진행
  - 오픈베타 : 일반유저에 모두 오픈
  - 클로즈드 베타 : 신청자 중 일부에게만 접근권한 제공
- 마케팅목적, 서버테스트




---




**RC (Realease Candidate)**

- 정식버전 
- Beta 테스트에서 많은 수정을 통해 가장 좋다고 생각되는 버전들. 이들 중 하나가 정식 버전이 됨.



**RTM(Release to Manufacturing)**

- 소프트웨어를 유저에게 제공될 준비가 완료된 상태
- 게임개발자가 개발결과물을 파는사람한테 주는 과정



**GA(General Availability)**

- 소프트웨어를 유저가 이용 가능한 상태
- 파는사람이 개발결과물을 팔기직전


---



# Agile Process



## Scrum

- 프로젝트 오너
- 개발팀
- 스크럼 마스터
  - 개발자와 오너사이의 커뮤니케이션관리 
  - 가장 핵심적인 존재



## Sprint

[![img](https://camo.githubusercontent.com/a5cee38675eed142312424595e2e2db6f9ec35a2/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f352f35382f536372756d5f70726f636573732e7376672f37303070782d536372756d5f70726f636573732e7376672e706e67)](https://camo.githubusercontent.com/a5cee38675eed142312424595e2e2db6f9ec35a2/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f352f35382f536372756d5f70726f636573732e7376672f37303070782d536372756d5f70726f636573732e7376672e706e67)

page와 기능 단계로 구분 (자세하게) => 하루, 30일 단위로 작업 분배 => 제품완성  



## Planning poker

- 작업시간 추정을 위해 사용함. 모든 팀원이 한가지 과제에 대해 충분히 토론하고 작업시간을 추정하기 위함
- deck 구성 0, 1/2, 1, 2, 3, 5, 8, 13, 20, 40, 100, ?, 무한대, 커피
- 점수는 단위작업시간(8시간)을 의미함
- 만장일치여야함. 가장적은시간과 가장많은시간을 낸 사람 끼리 토론.

#### poker일정 추정 과제

1. 은행 예금 계좌 및 체크카드 발급절차
2. Fizzbuzz
3. Profile Porfolio Page



## Pair Programming

- 시니어와 주니어가 한 팀을 이뤄 노하우를 전수하거나 같은 과제에 대해 충분한 논의를 함으로써 생산성 향상을 도모
- Navigator와 Driver가 한 팀을 이뤄 실시

## Code Review

검토사항

- 요구사항
- 설계요구 충족여부 ( 해봐야 느껴진다. )
- 과도한코딩
- 같은 기능
- 함수의 입출력
- 빌딩블록(API, 라이브러리, 자료구조, ..)
- 변수 사용전 초기화

---



# Websocket

socket 각각의 컴퓨터의 네트워크 end

웹은 Request & Response 이루어져 있음. 마치 페북의 타임라인 (특정 조건이있어야함. 스크롤을 맨 밑으로 내릴때)

**Websocket**은 스타크래프트처럼 턴이 없고 실시간. 마치 빗썹의 주가표 (조건없이 지 혼자서 계속 바뀜)

http://  

ws:// 웹소켓의 프로토콜?

### Communication channels

- simplex
  - 단방향 : 라디오
- half-duplex
  - 반이중 : 무전기 
- full-duplex
  - 양방향 : 전화, 현재의 거의모든 통신수단

## Websocket 이전엔...

- HTTP Request, Response
- Hidden Frame : 한번에 전부받아와 display: none으로 가려놨엇음
- Long Polling



Polling vs Websocket

[![img](https://camo.githubusercontent.com/c860ca32070be2c633b8f1cef44500ee0920ec51/687474703a2f2f64322e6e617665722e636f6d2f636f6e74656e742f696d616765732f323031352f30362f68656c6c6f776f726c642d313333362d312d312e706e67)](https://camo.githubusercontent.com/c860ca32070be2c633b8f1cef44500ee0920ec51/687474703a2f2f64322e6e617665722e636f6d2f636f6e74656e742f696d616765732f323031352f30362f68656c6c6f776f726c642d313333362d312d312e706e67)

## 다른점

Socket - HTTP run over TCP/IP 운영체제가제어

Websocket - 브라우저가 제어



## webSocket

HTML5의 표준 full-duplex 통신 방식

**broadcast ?**

> ex) 카톡에서 단톡방에서 문자를 보냈을때, 해당 문자가 서버를 통해 단톡방에있는 사람 모두에게 뿌려주는것