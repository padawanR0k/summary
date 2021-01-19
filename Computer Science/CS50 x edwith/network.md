# Network



## IP 주소

IP주소는 인터넷에 연결된 장치들을 식별할 수 있도록 해주며, 인터넷상의 다른 장치들이 특정 장치를 찾을 수 있도록 해준다. 

IPv4라고 불리는 32bit IP 주소 방식은 111.222.333.444 와 같이 이루어져있다. 32bit IP 주소방식은 약 40억개의 공인된 주소를 사용할 수 있다. 하지만 인터넷에 연결되는 장치들이 많아지면서 새로운 IP 주소방식이 필요하게 됐다.

IPv6라고 불리는 128bit IP 주소 방식은 숫자 8개를 가지며 각각 16bit 값을 나타낸다. af21:10a8:1253:abd3:3019:0c2e:0b80:12a0 이와 같이 이루어져 있다.

[![img](http://mooc.phinf.nhnnext.org/20170809_95/1502265126188NY3x2_PNG/6.2_-01.png?type=w760)](http://www.edwith.org/cs50/lecture/22871/#)  



### 인터넷에 연결

스마트폰을 인터넷에 연결시키는데는 생각보다 여러 단계가 필요하다. 먼저 AP(Access Point)에 무선으로 연결해야한다. 대부분의 AP의 예로는 공유기가 있다. 이 AP는 스위치에 연결된다. 스위치는 라우터에, 라우터는 인터넷의 나머지부분과 연결된다.

인터넷을 사용하는 과정에서는 DHCP(Dynamic Host Configuration Protocol)와 DNS(Domain Name System)는 중요한 역할을 한다.

DHCP는 컴퓨터에 IP주소를 할당하는 일을 하며 DNS는 URL을 받아서 IP주소를 변환해주는 일을 한다. 이게 무슨말이냐면

우리가 주소창에치는 naver.com은 URL이다. 이 URL을 주소창에 입력 후 엔터를 치면 DNS에서 이 URL과 일치하는 IP주소를 찾아서 우리에게 연결시켜준다. DNS는 우리가 사용하는 사설인터넷이 제공하는걸 써도 되지만 아시아에서 제일 빠른 [1.1.1.1](https://1.1.1.1/)을 사용해도된다.



### 사설 IP 주소

 사설IP 주소라고 알려진 어떤 주소들은 특정 로컬 네트워크 내에서 사용되도록 떼어 놓는다. 이 네트워크 밖에 있는 컴퓨터들은 이 네트워크 안에 있는 컴퓨터들에 접근할 수 없다. 보통, 사설 IP 주소를 갖는 장치들은 공인 IP 주소를 공유한다. 이 방식으로 IPv4표준에서 필요한 공용 IP 주소의  개수를 줄인다. 

> 10.#.#.#, 172.16.#.# - 172.31.#.#, 192.168.#.# 



## 라우터

한 장치에서 다른 장치로 데이터를 보낼 때 더 쉽게 데이터를 전송하는걸 돕기위해 라우터가 사용된다. 라우터는 데이터를 다양한 네트워크로 보내준다. 

### 라우팅 모델

인터넷에 연결된 모든 장치는 다른 모든 컴퓨터들과 물리적으로 연결되어있어야 한다. 아래의 그림을 보자[![img](http://mooc.phinf.nhnnext.org/20170809_293/1502270575484gYieF_PNG/6.4_-01.png?type=w760)](http://www.edwith.org/cs50/lecture/22875/#)  

이 방식은 모든 장치가 다른 장치를 거치는 과정이 필요가 없어서 좋긴 하지만 실제 인터넷에 연결된 장치는 수십억개가 넘으므로 이런 방식의 연결은 불가능에 가깝다. 라우터는 장치 연결간에 중재자 역할을 해준다. 

[![img](http://mooc.phinf.nhnnext.org/20170809_179/1502270620979hBb0a_PNG/6.4_-02.png?type=w760)](http://www.edwith.org/cs50/lecture/22875/#)  

아무리 많은 장치가 있다고 하더라도 라우터를 사용함으로써  효율적인 연결이 가능하다.

전송되는 데이터들은 패킷단위로 라우터를 통해 보내진다. 



### 라우팅 테이블

라우터는 각 데이터패킷의 목적지를 알 수 있도록 만들어져 있다. 이 정보들은 라우팅 테이블에 저장되어 있다. 라우터는 ip주소의 앞 숫자들을 보고, 패킷의 방향을 판단한다. 일반적으로 데이터가 인터넷의 한 지점에서 다른 지점으로 가기 위한 경로는 하나가 아니며, 같은 목적지의 데이터 패킷들을 서로 다른 경로로 전송한다. 



## TCP / IP

인터넷상의 통신은 프로토콜이 없이는 수신 장치가 정보를 받게끔 보장하거나 받은 정보를 무엇을 해야 할 지 보장해줄 수 없다. TCP 전송제어 프로토콜 (Transmission  Control Protocol) 과 IP 인터넷 프로토콜 (Internet Protocol)은 그 중에 하나이며, 둘을 함께 써서 TCP/IP로 알려져 있다.

IP가 패킷의 목적지를 알려주면 라우터를 통해서 데이터가 목적지까지 전송된다.

[![img](http://mooc.phinf.nhnnext.org/20170809_136/15022714048668tBno_PNG/6.5_-01.png?type=w760)](http://www.edwith.org/cs50/lecture/22877/#)  

### TCP

데이터를 다른 컴퓨터로 전송할 때 여러개의 작은 패킷으로 나누어 보내게 된다. TCP는 데이터를 순서있는 패킷들로 분해하는 일을 한다. 이 때, 같은 시간에 같은 순서로 목적 컴퓨터에 도착한다는 보장이 없으므로 각 패킷에 순서를 매긴다. 데이터가 전달된 컴퓨터에서는 매겨놓은 순서로 패킷을 재조립한다.

TCP는 또한 데이터에 포트번호를 할당한다. SMTP는 25, HTTP는 80



## HTTP

HTTP(Hyper Transfer Protocol)은 웹브라우저가 웹 서버와 통신하기 위한 프로토콜이다. 유저가 특정 웹페이지를 방문하려하면 HTTP프로토콜은 요청된 페이지를 유저에게 전달하는 과정을 용이하게해주며 표준적인 방법을 정해준다.



### 상태코드

웹 서버가 클라이언트로부터 HTTP요청(request)을 받으면, 서버는 클라이언트에게 응답(response)을 보내줘야한다. 이때 요청의 결과를 간단하게 표현한 상태코드를 함께 보내준다. 상태코드는 많지만 아래의 표는 자주보이는 상태코드들의 목록이다.

| 상태코드 |             의미             |
| :------: | :--------------------------: |
|   200    |             성공             |
|   301    |         영구 이동됨          |
|   302    |             찾음             |
|   403    |      금지됨 (forbidden)      |
|   404    | 찾을수 없음 (page not found) |
|   500    |        내부 서버 오류        |
