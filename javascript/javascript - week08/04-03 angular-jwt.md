로그인을 하는 방법
```sequence
유저->서버: 아이디, 비밀번호입력

Note right of 서버: 정보가 맞으면 토큰생성 

서버--> 유저: 토큰건내줌!

```

1. 쿠키

2. 로컬 스토리지

  ​

로그인을 한 후 애플리케이션이 꺼지고 난뒤 접근하였을때, sign 페이지 접근하면 로컬 스토리지에 Token을 확인한다. 이 방법은 현업에서는 쓰지않는다. 지금은 공부하는것이기 때문에 사용함.

Token 내부에는 사용자의 정보를 넣어 놓을수 있다.  



페이스북에 로그인에 상태라면 토큰을 가지고 페북에가서 자신의 아이디,비밀번호를 건내주고, 맞다면 페이스북에서 토큰을 줌. 

로그인 기능은 공유모듈

기능 모듈들이 로그인 기능을 씀.

쉐어드모듈은 기능이있고 각자 짝을 이루는 라우팅모듈이있음



버튼이 클릭하면 signgin(  ) 발동

signin()은 this.form.value인자로



post 로 emial하고 비밀번호를 날려준다. <Token> 

subscribe은 호출한 요소가 한다.

서버가 보내는 토큰을 로컬에 저장한다.



서버에 이메일과 비밀번호를 post함

서버가 token주고 그거 저장함

/Todos로 내비게이트함

로컬스토리지에 저장할때 

로그아웃하면 토큰을 삭제한다. 



특정 회원만 로그인가능하능게하는것.

CanActivae를 AuthGaurd에 import

DI로 주입받은후 필터를 구현함.



# 1. JWT(JSON Web Token)이란?







# 2. Angular JWT 인증
## 2.1 Backend
## 2.2 Frontend
## 2.2.1 로그인 컴포넌트 (signin.component.ts)
## 2.2.2 대시보드 컴포넌트 (dashboard.component.ts)
## 2.2.3 인증 서비스 (auth.service.ts)
## 2.2.4 인증 가드 (auth.guard.ts)
## 2.2.5 라우팅 모듈 (app-routing.module.ts)
## 2.2.5 사용자 서비스 (users.service.ts)