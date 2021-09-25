## 소프트웨어 개발

### 자료구조

- 선형구조
    - 배열
        - 메모리 낭비 발생
        - 반복적 데이터 처리에 용이
    - 스택
        - LIFO
    - 큐
        - FIFO
        - 시작과 끝을 표시하는 2개의 포인터가 있다
    - 데크
        - 양쪽이 다 큐처럼 동작
    - 선형리스트
        - 연속 리스트
            - 배열과 같이 연속되는 기억장소에 저장
            - 삽입, 삭제시 자료의 이동이 필요함
        - 연결 리스트
            - 노드의 포인트 부분을 이용해 서로 연결
            - 연결을 위한 포인터를 찾기때문에 접근속도가 느림
- 비선형구조
    - 트리
        - 노드와 브랜치를 이용해 사이클을 이루지않게 구성
        - 트리의 차수란?
            - 가장 많은 자식 노드를 가진 노드의 자식 노드 수
    - 그래프
        - 방향 그래프
            - 정점을 연결하는 선에 방향 존재
            - 그래프 최대 간선 수 = `n(n-1)`
        - 무방향 그래프
            - 정점을 연결하는 선에 방향이 없음
            - 그래프 최대 간선 수 = `n(n-1)/2`

### DBMS

### SQL

- DDL (Data Define Language)
    - 데이터 정의어
    - 저장될 데이터의 타입과 구조, 제약조건 명시
    - ex) CREATE, ALTER, DROP, TRUNCATE
- DML (Data Manipulation Language) (가장 자주 쓰는 것)
    - 사용자와 데이터 베이스 사이의 인터페이스 수단 제공
    - ex) SELECT, UPDATE, INSERT, DELETE
- DCL  (Data Control Language)
    - 무결성, 보안,  권한, 병행 제어
    - ex) GRANT, REVOKE, COMMIT, ROLLBACK

### 데이터 접속

- 매핑
    - 코드내에서 SQL을 직접 입력
    - ex) JDBC, MyBatis, ODBC
- ORM
    - 객체와 DB의 데이터를 연결, 쿼리 생성을 ORM에게 떠넘김
    - ex) JPA, Hibernate, Django, Sequalize,  typeORM

### 트랜잭션

DB의 상태를 변경시키는 작업의 단위, 한번에 수행되어야하는 연산들

- 특징
    - 원자성
        - 연산을 모두 반영 or 모두 반영 안함
    - 일관성
        - 일관성있는 상태를 유지
    - 독립성
        - 한 번에 한  개의 트랙잭션만 접근가능
    - 영속성
        - 성공저인 연산은 영구적으로 반영

- COMMIT
    - 변경후, 내용을 DB에 반영
- ROLLBACK
    - 트랜잭션이 행항 모든 변경작업을 취소
- SAVEPOINT
    - ROLLBACK할 저장점을 지정, 여러개 가능

### 절차형 SQL

- 프로시져
    - 미리 쿼리를 만들어 저장해놓고, 함수처럼 가져와 사용하느것
- 트리거
    - 입력, 갱신, 삭제 등의 이벤트 발생시 자동 수행하는 쿼리
- 사용자 정의 함수
    - 프로시져와 유사하나, 종료시  예약어 `RETURN` 을 사용해 결과를 반환

위에 있는 부분들은 큰 규모가 아닌이상 거의 사용안하는듯하다.

### 개발 지원 도구

다양한 툴을 하나의 인터페이스로  **통합**해 제공

- ex) 이클립스, VS, Xcode

- 빌드 자동화 도구
    - Ant
    - Maven
    - Gradle
    - Jenkins

### 버전 관리 도구

- 공유 폴더 방식 (완전 구식)
    - 공유 폴더에 저장하는 방식
    - ex) SCCS, RCS, PVCS
- 클라이언트/서버 방식 (구식)
    - 중앙 시스템에 저장되어 관리
    - 모든 버전관리는 서버에서 수행
    - ex) CVS, SVN
- 분산 저장소 방식 (현업)
    - 원격 저장소와 개발자들의 로컬 저장소에 함께 저장하는 방식
    - ex) git

### 애플리케이션 테스트

- 결험을 찾아내는 행위, 절차
- 고객의 요구사항이 구현돼는지 확인
- 기능에 대한 검증

- 기본원리
    - 완벽한 테스팅은 불가능
    - 결함은 20%모듈에 80%가 집중되어 발견됨 - 파레토 법칙
    - 살충제 패러독스 - 동일한 테스트 케이스에 의한 반복적 테스트는 새로운 버그를 찾을 수 없다.

### 애플리케이션 테스트 분류

- 실행 여부에 따름
    - 정적
        - 실행하지 않고 코드 분석
    - 동적
        - 실행하여 오류 분석
- 테스트 기반에 따름
    - 명세기반
        - 명세를 빠짐없이 테스트 케이스로 만듬
    - 구조기반
        - 내부의 논리 흐름에 따라 테스트 케이스 작성
    - 경험기반
        - 테스터의 경험을 기반
- 시각에 따른 테스트
    - 검증 테스트
        - 개발자의 시각에서 제품 생산과정 테스트
    - 확인 테스트
        - 사용자의 시각에서 제품 생산과정 테스트
- 목적에 따른 테스트
    - 회복
        - 에러 발생후 올바르게 복귀되는지 확인
    - 안전
        - 해킹으로부터 시스템을 보호할 수 있는지를 확인
    - 강도
        - 과부화테스트
    - 성능
        - 효율성 진단
    - 구조
        - 내부적인 논리적인 경로, 코드의 복잡도 진단
    - 회귀
        - 수정된 코드에서 결험이 없는지 확인
    - 병행
        - 기존, 변경된 두 소프트웨어에 동일한 데이터를 입력하여 결과 비교

### 화이트박스 테스트, 블랙박스 테스트

- 화이트박스 테스트
    - 내부의  논리적인 모든 경로 테스트 케이스 설계
    - 논리적 경로 점검
    - 종류
        - 기초 경로  검사
        - 제어구조검사
            - 조건 검사
                - 논리적 조건 테스트
            - 루프 검사
                - 반복 구조에 맞춰 테스트
            - 데이터 흐름 검사
                - 변수의 정의와 변수 사용위치에 맞춰 테스트
- 블랙박스 테스트
    - 어떤 일이 일어나는지 알 수 없음
    - 소프트웨어 인터페이스에서 실시
    - 종류
        - 동치 분할 검사
            - 참, 거짓 자료를 균등하게 케이스 구성
        - 경계값 분석
            - 경계값을 위주로 테스트 케이스로 선정
        - 원인-효과 그래프 검사
            - 입력 데이터가 출력데이터에 끼치는 영향분석→ 효용성이 높은(테스트 케이스작성이 효과적인)케이스를 선정
        - 비교 검사
            - 병행 목적 애플레케이션 테스트
        - 오류 예측 검사
            - 데이터 확인 검사

### 개발 단계에 따른 테스트

- 단위
    - 모듈이나 컴포넌트처럼 최소단위로 테스트
    - 주로 구조기반 테스트를 시행
    - ex) OOO를 위한 클래스, OOO를 도출하는 함수
- 통합
    - 단위 테스트가 완료된 모듈을 결합한 하나의 시스템을 완성시키는 과정의 테스트
    - 통합된 컴포넌트 간의 테스트
    - ex) 빅뱅 테스트, 상향식 테스트, 하향식 테스트
- 시스템
    - 컴퓨터 시스템에서 완벽하게 수행되는가 점검
    - ex) 블랙박스 테스트, 화이트박스 테스트
- 인수
    - 사용자의 요구사항을 충족하는지 테스트

### 통합 테스트

- 상향식
    - 하위 → 상위
        - 클래스, 함수 → 모듈 → 컨트롤러, 모델, 뷰 처럼 점점 코드의 크기가 더 커짐
- 하향식
    - 상위 → 하위
        - 위 순서와 반대로
    - 깊이 우선, 넓이 우선 통합법
- 혼합식
    - 하위수준에서는 상향식, 상위 수준에서는 하향식
    - 샌드위치 통합 테스트 방법

### 테스트 케이스, 시나리오, 오라클, 하네스

- 테스트 케이스
    - 요구사항을 구현했는지 확인하는 입력값, 실행 조건, 기대결과 등으로 된 명세서 코드
- 테스트 시나리오
    - 여러 개의  테스트 케이스를 묶은 집합, 여러 시나리오로 작성해 작성
    - 테스트 케이스를 적용하는 구체적인 절차를 명세
- 테스트 오라클
    - 테스트 결과가 올바른 값인지 판단하는 활동
- 테스트 하네스
    - 테스트 드라이버
        - 시스템제어나 호출하는 컴포넌트를 대체하는 툴
    - 테스트  스텁
        - 골격만 있는 특별한 목적의 컴포넌트를 구현한것. (복잡한 기능을 대체하고, 내부는 비어있는)
    - 테스트 슈트
        - 테스트 케이스의 집합
    - 테스트 스크립트
        - 테스트 실행 절차에 대한 명세서
    - 목 오브젝트
        - 사용자의 행위나 데이터를 조건부로 미리 입력해두는것

### 애플리케이션 성능 분석

- 용어 설명
    - 처리량
        - 일정한 시간동안 애플리케이션이 처리가능한 양
    - 응답시간
        - 요청-응답이 도착할 때 까지 걸린시간
    - 경과시간
        - 작업을 의뢰한 시간부터 처리가 완료될 때까지 걸린시간
    - 자원 사용률
        - 작업 수행하는 동안 사용된 자원 사용률
- 소스코드 최적화
    - 클린코드 작성원칙
        - 가독성
        - 단순성
        - 의존성 배제
        - 중복성 최소화
        - 추상화
- 소스코드 품질분석 도구
    - 정적
        - pnd, cppcheck, checkstyle, sonarQube, ccm, covbertuna
    - 동적
        - Avalanche, Valgrind

### 모듈 연계

- Enterprice Application Intergration
    - 정보 전달, 연계, 통합 등 상호 연동이 가능하게 해주는 솔루션

- 포인트 투 포인트
    - 점 대 점 (재활용 어려움)
- 허브엔 스포크
    - 단일 점접인 허브를 둠(중앙 집중형식)
    - 허브에서 에러 발생시 전체 영향감
- 메세지 버스
    - 애플리케이션 사이에 미들웨어를 둠
    - 확장성 좋음
- 하이브리드
    - 허브와 스포크와 메세지버스 혼합 방식
    - 데이터 병목 최소화

### 인터페이스 검증/구현

- 도구
    - xUnit
        - 다양한 언어를 지원
    - STAF
        - 서비스 호출, 컴포넌트 재사용 등 다양한 환경 지원
    - FitNesse
        - 웹기반 테스트 케이스 설계, 실행, 결과 지원
    - NTAF
        - STAF 장점, FitNesse의 장점을 통합
        - **NHN에서 사용한다고함!**
    - Selenium
        - 다양한 브라우저를 지원. 브라우저를 코드로서 제어할 수 있음
    - watir
        - Ruby 테스트 프레임워크
- 인터페이스 오류 발생 주기적 확인
    - 오류 로그
        - 자세한 오류 원인 및 내역확인 가능하나 양이 많음
    - 오류 테이블
        - 관리가 용이하나 별도의 분석이 필요
    - 감시도구 사용 (APM)
        - 스카우터
        - 제니퍼