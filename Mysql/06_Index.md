## Stored
- DBMS에서 제공하는 여러 기능들이 있는데, 그중 상태를 저장해서 필요할 때 마다 호출할 수 있는 기능들이 있다.
### Stored Procedure
- 함수처럼 SQL문을 저장하고 호출하는 기능
	- SQL문을 호출만할 수도 있고, 호출의 결과를 반환받을 수도 있다.
- 선언과 호출
	```sql
	DELIMITER $$
	CREATE PROCEDURE Name ()
	BEGIN
		...
	END
	$$ DELIMITER;

	CALL Name();
	```
- 파라미터 전달
	```sql
	USE sqlDB;
	DROP PROCEDURE IF EXISTS userProc1;
	DELIMITER $$
	-- IN 예약어가 붙은 파라미터는 외부에서 프로시져로 전달하는 파라미터를 뜻한다.
	CREATE PROCEDURE userProc1(IN userName VARCHAR(10))
	BEGIN
		SELECT * FROM userTbl WHERE name = userName;
	END $$
	DELIMITER ;

	CALL userProc1('홍길동');
	```

	```sql
	DROP PROCEDURE IF EXISTS userProc3;
	DELIMITER $$
	-- OUT 예약어가 붙은 파라미터는 프로시져 내부에서 외부로 전달시킬 변수이다.
	CREATE PROCEDURE userProc3(
			IN txtValue CHAR(10),
			OUT outValue INT
	)
	BEGIN
		INSERT INTO testTBL VALUES(NULL,txtValue);
		-- OUT될 outValue에 값을 할당할 땐, SELECT ~ INTO를 사용한다.
		SELECT MAX(id) INTO outValue FROM testTBL;
	END $$
	DELIMITER ;


	CALL userProc3 ('테스트값', @myValue);
	SELECT CONCAT('현재 입력된 ID 값 ==>', @myValue);
	```
- 특징
	- 모든 쿼리문자열을 전달하지않고, 프로시져를 실행시키는 문자열만 전달하기때문에 성능 향상
	- 클라이언트 응용 프로그램에서 직접 SQL을 작성하지 않아도 되기때문에 유지보수에 이점 (무조건은 아니다. 각 프로젝트 구조마다 다를 수도 있다.)
	- 디버깅하기 어렵다.
	- 각 프로시져에 대한 외부문서 관리가 필요하다.

### Stored Function
- 선언과 호출
	```sql
	USE sqlDB;
	DROP FUNCTION IF EXISTS userFunc;
	DELIMITER $$
	CREATE FUNCTION userFunc(value1 INT, value2 INT)
			RETURNS INT
	BEGIN
			RETURN value1 + value2;
	END $$
	DELIMITER ;

	SELECT userFunc(100, 200);
	```
- 사용자가 직접 생성하여 사용할 수 있는 함수
- 프로시져와 다른점
	- 모든 파라미터는 입력파라미터이다 -> IN, OUT사용할 수 없다.
	- 어떤 데이터형식을 반환할건지 작성해야하고, RETURN문으로 값을 반환한다.
	- SELECT문 안에서 호출된다.
	- SELECT문을 사용할 수 없다.
	- 주로 계산이나, 하나의 값을 반환하는데 주로 사용된다.
	- `SET GLOBAL log_bin_trust_function_creators = 1;`로 권한 허용필요


### Cursor
- 무엇을 선택하는 손 으로 이해 하면 된다. 여기서 말하는 무엇은 SELECT의 결과들이다. 이 커서는 SELECT 결과를 차례대로 선택하여 볼 수 있게 해줄 수 있을 뿐 아니라, 원하는 행을 선택 할 수도 있다.
- 언제 사용하는가?
	- 연계된 작업으로 첫번째와 마지막 등 특정 위치를 제어 하고자 할 때(당연하지 않는가?)
	- 유저가 커서에만 접속하게 만들어, 제약을 걸어둘 필요가 있을 때
	- 커서로 열고 있는 동안 기존 데이터가 변화되지 않도록 막을 때
	- 한번에 모든 량을 가져오면, 메모리가 부족해 지는 것을 방지하기 위해
- 주의 사항
	- 웹기반 응용프로그램에서는 커서를 사용하는 것은 무리가 따른다. 왜냐하면, 커서를 사용 하면, 사용 시간 중 DB 변경을 막게 되므로, 많은 커서 사용은 결국 DB 성능 저하를 불러 일이키기 때문이다.
- 선언과 호출
	```sql
	DROP PROCEDURE IF EXISTS gradeProc;
	DELIMITER $$
	CREATE PROCEDURE gradeProc()
	BEGIN
			DECLARE id VARCHAR(10); -- 사용자 아이디를 저장할 변수
			DECLARE hap BIGINT; -- 총 구매액을 저장할 변수
			DECLARE userGrade CHAR(5); -- 고객 등급 변수

			DECLARE endOfRow BOOLEAN DEFAULT FALSE;

			DECLARE userCuror CURSOR FOR-- 커서 선언
					SELECT U.userid, sum(price*amount)
							FROM buyTbl B
									RIGHT OUTER JOIN userTbl U
									ON B.userid = U.userid
							GROUP BY U.userid, U.name ;

			DECLARE CONTINUE HANDLER
					FOR NOT FOUND SET endOfRow = TRUE;

			OPEN userCuror;  -- 커서 열기
			grade_loop: LOOP
					FETCH  userCuror INTO id, hap; -- 커서를 실행시켜서 해당값을 id, hap 변수에 할당함
					IF endOfRow THEN
							LEAVE grade_loop;
					END IF;

					CASE
							WHEN (hap >= 1500) THEN SET userGrade = '최우수고객';
							WHEN (hap  >= 1000) THEN SET userGrade ='우수고객';
							WHEN (hap >= 1) THEN SET userGrade ='일반고객';
							ELSE SET userGrade ='유령고객';
					END CASE;

					UPDATE userTbl SET grade = userGrade WHERE userID = id;
			END LOOP grade_loop;

			CLOSE userCuror;  -- 커서 닫기
	END $$
	DELIMITER ;

	CALL gradeProc();
	```
- 배열(table)을 돌면서 각 데이터마다(row) 특정행위를 하게끔 하는것이 마치 js의 map, filter, reduce 같은 느낌이다.

### Trigger
- 테이블에 삽입,수정,삭제 등의 작업을 감지하여 자동으로 실행되는 개체이다.
- 프로시져와 작동은 비슷하나, 직접 실행시킬 수 없다.
- TRIGGER 구분
	- BEFORE
		- 작업 시작전에 실행
	- AFTER
		- 작업이 완료된 후 실행
- 선언
	```sql
	DROP TRIGGER IF EXISTS testTrg;
	DELIMITER //
	CREATE TRIGGER testTrg  -- 트리거 이름
			AFTER  DELETE -- 삭제후에 작동하도록 지정
			ON testTbl -- 트리거를 부착할 테이블
			FOR EACH ROW -- 각 행마다 적용시킴
	BEGIN
		SET @msg = '가수 그룹이 삭제됨' ; -- 트리거 실행시 작동되는 코드들
	END //
	DELIMITER ;

	SET @msg = '';
	INSERT INTO testTbl VALUES(4, '마마무');
	SELECT @msg; -- ''
	UPDATE testTbl SET txt = '블핑' WHERE id = 3;
	SELECT @msg; -- ''
	-- DELETE에 대한 트리거가 실행됬다
	DELETE FROM testTbl WHERE id = 4;
	SELECT @msg; -- '가수 그룹이 삭제됨'
	```
##### 참고
- [[MySQL] Stored Procedure 알아보기 (in MySQL)](https://medium.com/@dahaejeon/mysql-stored-procedure-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-in-mysql-1fdd342b661a)

- [21장, 커서 사용 : DECLARE CURSOR](https://www.ikpil.com/1111)


##### 개인적인 생각
DBMS자체에서 제공하는 기능들을 사용하게되면 클라이언트 응용프로그램단(java, js, python)에서는 해당 기능의 이름만알고 호출하면 된다는 장점이있다. 그런데 단점이 더 많은거같다.
1. 기능이 정확히 어떻게 작동하는지 클라이언트단 개발자는 알 수 없다.
2. git같이 유지보수하면서 버전관리, 용어의 통일, 환경변수 공유등을 할 수 없다.
3. 1번의 문제로 인해 커뮤니케이션에 비용이 더 많아질거같다.
문서화가 잘되어있고, 서로 간의 이해가 잘되어있다면 좋을듯하다.