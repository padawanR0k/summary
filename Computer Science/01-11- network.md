<!-- $theme: default -->

# 프론트엔드개발을 위한 기초지식

## Network
컴퓨터간 리소스를 공유 가능하게 만드는 통신망

---
### 네트워크의 특징
- 리소스를 공유
- 네트워크로 연결된 다른 컴퓨터에 접근하여 파일을 생성,수정,삭제 가능
- 프린터와 스캐너,팩스 등의 출력장치도 동시에 접근가능

### 필요한 장비
- Router : ex) 공유기
- Network Card : ex) 메인보드의 랜카드
- Network Cable
- Distributor (Swich hub) : 대량의 네트워크 신호를 받아서 다시 뿌려주는것 ex) 피시방, 지하철

---
### 범위에 따른 네트워크 구분

#### LAN
- 근거리 통신망
#### WAN
- 광역 통신망

#### MAN
- 도시권 통신망
- LAN < MAN < WAN
#### WLAN
- 무선 근거리 통신망
- IEEE 802.11 표준을 기반

#### 802.11 != wifi ★
- 802.11: IEEE에서 개발된 무선통신기술
- wif: 와이파이 얼라이언스의 상표. 802.11 가술을 사용하는 무선 근거리 통신망 제품
- 2.4Ghz와 5Ghz의 차이점?
주파수 대역대가 높으면 신호 세기 강하다.
그러나 조금만 범위를 벗어나면 세기가 기하급수적으로 떨어진다.

#### 다른방식의 네트워크
- Lifi : 빛을 이용한 네트워크
- Power Line Networking : 전력선을 이용한 네트워크

### 네트워크 망구조방식
![topoloy](https://camo.githubusercontent.com/328a33cce33ce8e95eb6c7cc01a19941fae1817b/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f392f39362f4e6574776f726b546f706f6c6f676965732e706e67)
---

### Ethernet
- 전세계의 사무실이나 가정에서 일반적으로 사용되는 유선랜의 기술규격

### Network OSI 7 layer
![ OSI7 layer ](https://camo.githubusercontent.com/6c2b56b3471651c8b3f282daa8dbfae236068b41/687474703a2f2f70647331332e65676c6f6f732e636f6d2f7064732f3230303930372f32322f36372f64303037383036375f346136363661613733633663372e676966)

	각 레이어 들을 거치며 Data 앞에 헤더가 붙는다.
    
	1: ,2 : 데이터의 물리적인 이동	
---    
	3 : 데이터의 경로를 계산해서 보냄 ex) router
---    
	4 : 패킷의 크기로(1500byte)으로 분할,제어,재조립하여 데이터를 전송.
     오류관리도 함 
---
	5 :모든 통신 장비를 연결하고 관리하며 종료, 사용자의 연결상태를 서버가 갖고잇는 로그 (그와 반대로 Cookie는 브라우저가 갖고잇는 로그)
---
	6 : 데이터를 웹브라우저가 os에게 전달함, 암호화 복호화가 일어남
---	
	7 : 사용자에게 네트워크 활동에 대한 인터페이스를 제공 ex) 웹브라우저 ,사용자에게 보이는 유일한 계층 

폴링: Session Layer에서 요청과 응답사이에 요청을 또 넣는 기술 ex) facebook.com 의타임라인

#### 그 외
- 핫스팟 : 스마트폰이 라우터가 되는것
- 블루투스 테더링 : 1대1로 연결되는것
- 노트북에 유선랜선을 연결하면 노트북도 라우터가 된다.
  
## Network OSI Layer
![](https://camo.githubusercontent.com/6367e6a2a50124fb1b771d9b179c66ee3ab4a5ef/687474703a2f2f6366696c6532352e75662e746973746f72792e636f6d2f696d6167652f31333433304634363444413930344534313537374131)

### TCP/IP Protocol
Application : 어떤자료를
transport : 어떻게 보낼것인가
internet : 주소체계를 어떻게 정할건지

---
### 애플리케이션 레이어
#### HTTP
- http method : GET, POST, PUT, DELETES
- 헤더의 국가코드를 읽어서 국가별로 다른 주소를 보여줄수 있다. ex) .co.kr , co.uk
- http:// 와 https:// 차이는 보안능력의차이
```python
> from urllib.request import urlopen
> response = urlopen('https://www.google.com/')
> response
> response.readline()
> response.url
> response.status
```
- response.status = 200 정상
- response.status = 300 주소바뀜
- response.status = 404 오류 페이지없음
- response.status = 403 Forbidden 권한이없는 주소를 접속했을때 
- response.status = 500 서버오류

#### FTP
- 서버와 클라이언트 사이에 파일전송을 위한 프로토콜
- 보안에 매우 취약하다
- 현재는 FTPS, SFTP, SSH 를 사용한다. ( wordpress 가 사용중 /login)

#### SMTP
- 메일을 보내기 위한 프로토콜
---
### 트랜스포트 레이어

#### TCP
- 전송제어 프로토콜
- 옥탯(==Byte)을 안정적으로, 순서대로, 에러없이 교환가능하게함
#### TCP 패킷
- STREAM : 연속성이있는 데이터 ( 10개의 패킷이 모두 있어야 볼수있음)
- DATAGRAM : 데이터 하나에 정보가 있는 데이터 ( 패킷 1개만있어도 1개의 패킷을 볼수있음)

#### TCP 소켓
- STREAM sockect: 두개의 시스템이 연결된 후 확인되면 데이터를 교환 
- DATAGRAM socket : 서로 연결된지 확인안하고 데이터를 주고받음
---

### IP

#### IPv4
- 32bit로 구성
#### 127.0.0.1 vs 192.168.0.x
- 127.0.0.1 : 컴퓨터가 가지고 있는 무조건 반대신호를 반환하는 대역
- 192.168.0x : LAN에서 라우터가 할당한 내컴퓨터의 IP address
####  IPv6
- 128bit로 구성
- 16진수 3개로 구성됨
#### DNS
- 매핑하기 어려운 ip를 사람이 보기쉽게 url을 매핑하는 시스템

#### MAC 
- 12개의 16진수로 구성

#### ARP
- IP주소를 MAC 주소로 변경해줌

#### Defualt Subnetmask
- Class C : 255개
- Class B : 255^2개 ex) 큰 곳
- Class A : 255^3개

### UDP
- 데이터그램을 전송하기위한 프로토콜
- 빠른속도, 적은오버헤드
- 빠른전송이 필요한 게임에서 사용 ex) FPS, 대전

#### 안정적인 TCP vs 빨라야하는 UDP
![표](https://camo.githubusercontent.com/0f7372f8f42abcef7a0189b0dacd019b00045ffd/687474703a2f2f332e62702e626c6f6773706f742e636f6d2f2d4e736d376e53614d5a6f4d2f566d5664594764665276492f414141414141414142694d2f705934335a7477743775632f73313630302f5443505f5544505f686561646572732e4a5047)


### Socket
- 가상의 두 지점  ex) 전화교환수



# 정리
- 인터넷의 물리적 이동
-  TCP 와 UDP 차이는 뭔가
-  소켓?
-  Network OSI Layer 
-  TCP/IP Protocol