## Table
- RDBMS의 table은 행과 열로 구성되어있다.
- table에 대한 모델링설계도 중요하지만 운영시, 튜닝을 통한 속도 개선도 매우 중요하다.
### 컬럼 제약 조건들
- PK (Primary Key)
	- 테이블의 존재하는 row들의 고유한 키
	- 중복과 NULL이 될 수 없다.
	- N개의 컬럼을 합쳐서 기본키로 설정할 수도 있다.
		- 그런데 PK를 여러개로 묶어서 해버리면 JOIN시에 성능에 문제가 생기지 않을까?
			- https://okky.kr/article/207776 -> 상황에 맞는 설정을 해라
- NN (Not NULL)
	- 해당 컬럼이 NULL값을 허용하지 않아야하는 경우 사용한다. (PK는 NN설정이 강제된다.)
- UQ (UNIQUE)
	- 컬럼이 중복되지 않는 유일한 값이여야하는 조건이다. PK와의 다른점은 NULL을 허용한다는 점이다.
	- UQ 컬럼은 여러개여도 상관없다.
		- 이메일, 주민번호
- BIN (Binary)
	- 이진 문자열형식으로 데이터를 저장한다.
- UN (Unsigned)
	- 숫자형 데이터가 음수값으로 들어갈 경우가 없는 경우 사용한다.
- ZF
	- 입력한 자리수에 빈공간이 있으면 모두 0으로 채운다.
- AI (Auto Increasement)
	- row가 INSERT 될 때, 자동으로 1씩 증가하는 숫자값 (증가값을 설정으로 바꿀 수도있다.)
- G
	- 다른 열을 기반으로하는 수식에 의해 생성 된 값
- CHECK
	- 입력되는 데이터에 대해 점검하는 기능을 한다.
	- 이런 제약조건은 SQL단에서 하는것보다 상위 레이어?에서 하는게 예외처리, 코드관리 측면에서 더 좋을듯하다.
- DEFAULT
	- row가 INSERT될 때, 컬럼의 기본값을 설정할 수 있다.
	- 본인이 설정한 컬럼의 타입에 맞는 기본값을 설정해주어야한다.
	- 기본값으로 데이터를 넣고싶은경우 데이터 값대신 `default`예약어를 사용하면된다.

- FK (Foreign key)
	- 두 테이블간의 관계를 프로그래밍적으로 선언함으로써 무결성을 보장해준다.
		- 예를 들어서 FK를 설정하게되면 기준이 되는 테이블에 값이 존재해야만 외래 키 테이블에 데이터가 INSERT된다.
	- FK를 지정하면 무결성도 보장해주며, workbench자체에서 ERD를 그려줄 때 표기도 자동으로 해준다. 하지만 실무에서는 FK를 사용하지 않는 경우가 많다. 설계상으로만 존재하고 시스템에는 반영하지 않는다. 실서비스에서는 새로운 요구사항이 자주 생기고 서비스가 자주 변화하기 때문에 FK같은 강한제약은 오히려 독이 되기도한다. 또한 특정 컬럼을 업데이트할 때 연결된 FK를 체크하게된다. 데이터의 양이 많은 경우 체크에 드는 리소스가 성능에 문제가 되기도한다.
		- [외래키 안쓰는 이유?](https://okky.kr/article/497991)
		- [외래키 제약조건을 사용하지 않는 이유?](https://www.a-ha.io/questions/4061a1b5ddbe2245a168ca045afc2f65)

### 테이블압축
- mysql 5.0버전 이상부터는 테이블의 용량을 줄일 수 있다.
	- ```sql
			CREATE TABLE compressTBL( emp_no int , first_name varchar(14))
			ROW_FORMAT=COMPRESSED ;
		```
- 장단점
	- 압축을 하게 되면 압축에 대한 공간에 대한 이점이 있다. 압축을 하게 되면.. SELECT시 적은 block 만 읽으면 된다 I/O 성능 향상
	- 압축을 하게 되면 INSERT시 압축에 대한 CPU 부하가 발생 된다. 압축을 하게 되면 SELECT시 압축을 풀어주는 CPU 부하가 발생 된다.

### 테이블 삭제
```sql
DROP TABLE TableName;
```
- 만약 테이블에 FK가 존재할 경우 원본이 되는 FK테이블을 먼저 삭제한후 실행해야한다.

### 테이블 수정
- 이 부분은 주로 GUI툴(workbench)로 거의다 처리가능하다. 그래도 일단 내부적으로 실행되는 코드가 어떤것들인지 알아만 두자
```sql
-- 컬럼 추가
ALTER TABLE usertbl
	ADD homepage VARCHAR(30)  -- 열추가
		DEFAULT 'http://www.hanbit.co.kr' -- 디폴트값
		NULL; -- Null 허용함

-- 컬럼 삭제
ALTER TABLE usertbl
	DROP COLUMN mobile1;

-- 컬럼 수정
ALTER TABLE usertbl
	CHANGE COLUMN name uName VARCHAR(20) NULL ; -- 이름과 타입을 변경

-- 기본키 플래그 삭제
ALTER TABLE usertbl
	DROP PRIMARY KEY;

-- 기본키 삭제는 외래키 삭제가 먼저 이루어져야 가능
ALTER TABLE buytbl
	DROP FOREIGN KEY buytbl_ibfk_1;
```
## View
- table을 사용해서 만든 가상의 table이 view이다. 실체테이블은 존재하지 않는다. (사실상 SELECT문이다.)
	- 실제로 VIEW 생성 코드는 아래와 같다
		```sql
		CREATE VIEW v_usertbl
			AS
				SELECT userid, name, addr FROM usertbl;
		```
### 그럼 왜 사용하는걸까?
- 보안
	- 특정 개발자가 외주 혹은 신입이여서 민감한 정보를 보면 안되는 경우, view를 만들어 일부분만 공유할 수 있다.
- 복잡한 쿼리 단순화
	- 원본 테이블이 아닌 컬럼명칭이나 일부 계산이 들어간 컬럼이 새로 추가된 쿼리를 매번 실행시키는 대신 view에 저장하여	실행시키면 재활용이 가능하다.

## 테이블 스페이스
- 대용량 데이터를 다루는 경우, 성능향상을 위해 테이블 스페이스에 대해 설정하는것이 좋다.
- 실제로 저장되는 물리 공간을 말하며, 지정하지 않으면 기본값으로 설정되어있다.
	```sql
	SHOW VARIABLES LIKE 'innodb_file_per_table'; -- ibdata1:12M:autoextend (파일명:파일크기:최대파일크기)

	```
### 성능향상을 위한 테이블 스페이스 추가
- 대용량 테이블을 동시에 사용하는 경우, 테이블 마다 별도의 테이블 스페이스에 저장하는것이 성능에 좋다.
- 테이블 스페이스는 데이터를 저장하는 물리적인 공간을 직접 분리 할수 있게해준다
	- ```sql
		CREATE TABLESPACE ts_a ADD DATAFILE 'ts_a.ibd'; -- mysql 폴더에 새로운 tablespace파일을 만든다.
		CREATE TABLE table_c (SELECT * FROM employees.salaries); -- 기존에 있던 데이터를 새로운 테이블에 복사한다.

		ALTER TABLE table_c TABLESPACE ts_c; -- 새로만든 테이블의 tablespace를 변경항ㄴ다.
		```