## 데이터의 형식의 종류
> MySQL에서 데이터형식의 종류는 30개 가까이된다. 자주 쓰이는 것부터 외우자.

데이터에 형식에 따라 메모리공간을 효율적으로 사용할지, 비효율적으로 사용할지 결정된다. 현실세계의 개념을 데이터로 옮기려고할 때, 목적에 맞는 데이터형식을 잘사용하여야한다. 예를 들어서 절대 1~100범위를 넘어나지 않을 데이터에 INT 타입으로 컬럼을 할당하는건 낭비이다.

### 숫자 데이터 형식

| 데이터 형식 | 바이트수 | 숫자범위 | 설명 |
|--|--|--|--|
|SMALLINT|2|-33,726~32,767| 정수|
|INT|4|약 -21억~+21억|정수|
|BIGINT|8|약 -900경~+900경|정수|
|FLOAT|4|-3.40E+38  ~ 1.17E-38|소수점 아래 7자리까지|
|DECIMAL(m,[d])|5~17|-10^38+1 ~ +10^38-1|전체 자릿수(m)와 소수점 이하 자릿수(d)를 가진 숫자형

### 문자 데이터 형식

| 데이터 형식 | 바이트수 | 설명 |
|--|--|--|
|CHAR(n)| 1~255 | 고정 길이문자형|
|VARCHAR(n)| 1~65535 | 가변길이 문자형|
|LONGTEXT(n)| 1~4294967295 | 최대 4GB킉의 TEXT 데이터값|
|LONGBLOB(n)| 1~4294967295 | 최대 4GB킉의 BLOB 데이터값|

### 시간 형식

| 데이터 형식 | 바이트수 | 설명 |
|--|--|--|
|DATE)| 3 | YYYY-MM-DD 형식만 사용|
|DATETIME| 8 | YYYY-MM-DD HH:MM:SS 형식으로 사용|


### 변수의 사용
```sql
SET @변수명 = 값
SELECT @변수명; # 값 출력
```
> 프로시져나 함수안에서는 DECLARE문으로 선언한 후 사용할 수 있다. 또한 @ 키워드 없이 사용한다. @변수명은 전역변수처럼 사용하는것이다.

```sql
SET @VAL = 3;
PREPARE myQuery
	FROM 'SELECT COL1 from TableName  ORDERR BY COL2 LIMIT ?'
EXECUTE myQuery USING @VAL;
```
이런식으로 `?`영역에 변수를 할당할수 도 있다.

#### 암시적 형변환
- 명시적 형변환
	- `CAST()`, `CONVERT()`와 같이 함수를 사용하여 변환
- 암시적 형변환
	- 위 함수등을 사용하지 않고 형변환하는것
```sql
SELECT '100' + '200'; # 300 암시적으로 정수로 형변환됨
SELECT CONCAT('100','200')  # '100200'
SELECT CONCAT(100,'200')  # 300
SELECT 1 > '2mega'; # FALSE 문자열이 정수 2로 변환되어 비교됨
```
> DMBS에 따라 암시적 형변환의 룰이 다르므로 조심해야한다.

#### MySQL 내장 함수

##### 제어문

- `IF(수식, 참일 경우, 겨짓일경우)`
```sql
SELECT IF (100>200, '맞음', '틀림)
```

<br />

- `IFNULL(수식, NULL일 경우)`
```sql
SELECT IF (NULL, '맞음', '틀림)
```

<br />

- `CASE ~ WEHN ~ ELSE ~ END`
- 타 언어들의 스위치문 개념
```sql
SELECT CASE COL1
	WHEN 1 THEN 'won'
	WHEN 2 THEN 'two'
	WHEN 3 THEN 'three'
	ELSE '?'
END AS 'CASE'
```


##### 문자열 함수

- `ASCII(), CHAR()`
	- 문자를 아스키코드로, 아스키코드를 문자로
```sql
SELECT ASCII('A'), CHAR(65);
```

<br />

- `IFNULL(수식, NULL일 경우)`
```sql
SELECT IF (NULL, '맞음', '틀림)
```

<br />

- `BIT_LENGTH(), CHAR_LENGTH(), LENGTH()`
	- 할당된 Bit 크기 또는 문자크기를 반환한다.
	- CHAR_LENGTH(); 문자의 개수를 반환한다.
	- LENGTH(); 할당된 Byte수를 반환한다.
```sql
SELECT BIT_LENGTH('abc'), CHAR_LENGTH('abc'), LENGTH('abc');
```

- `CONCAT_WS(구분자, 문자열1, 문자열2..)`
	- 문자열들을 구분자로 연결한다.
```sql
SELECT CONCAT_WS('-', '2020', '01', '01');
```


- `ELT(위치, 문자열1, 문자열2..)`
	- 받은 문자열들중 위치번째의 문자열을 반환함
```sql
SELECT ELT(1, '하나', '둘'); # '하나'
```

- `FIELD(위치, 문자열1, 문자열2..)`
	- 받은 문자열들중 일치하는 문자열의 위치를 반환
```sql
SELECT ELT('1', '1', '둘'); # 1'
```

- `FIND_IN_SET(위치, 문자열1)`
	- 받은 문자열에서 일치하는 문자열의 위치를 반환
	- 문자열은 1개만 받을 수 있으며, `,`로 분리되어있어야함
```sql
SELECT ELT('1', '1,둘'); # 1'
```

- `INSERT(기준 문자열, 위치, 길이, 삽입할 문자열)`
	- 기준 문자열의 위치부터 길이만큼 지우고 삽입할 문자열을 대체함
```sql
SELECT INSERT('1111', 2,3,'*'); # 1**1
```

- `LEFT|RIGHT(문자열, 길이)`
	- 문자열중 길이만큼만 반환한다
```sql
SELECT LEFT('1234', 2); # '12'
SELECT RIGHT('1234', 2); # '34'
```

- `SUBSTRING_INDEX(문자열, 구분자, 횟수)`
	- 구분자로 문자열을 나눈뒤 횟수만큼의 덩어리만 가져오고 나머지는 버린다.
```sql
SELECT SUBSTRING_INDEX('map.naver.com', '.', 2); # map.naver
SELECT SUBSTRING_INDEX('map.naver.com', '.', -1); # com
```

##### 시간 함수
- `ADDDATE(날짜, 차이), SUBDATE(날짜, 차이)`
```sql
SELECT ADDDATE('2020-01-01', INTERVAL 31 DAY); # '2020-02-01'
SELECT SUBDATE('2020-01-01', INTERVAL 31 DAY); # '2019-12-01'
```

- `CURDATE(), CURTIME(), NOW()`
순서대로 '연-월-일', '시:분:초', '연-월0일 시:분:초'를 구한다.

- `DATE()`
DATETIME 형식에서 '연-월-일' 형식만 추출함

- `TIME()`
DATETIME 형식에서 '시:분:초' 형식만 추출함


- `DATEDIFF(날짜1, 날짜2)`
날짜1, 날짜2의 일자차이를 반환함


- `TIMEDIFF(시간1, 시간2)`
날짜1, 날짜2의 시간차이를 반환함('hh:mm:ss')


## JOIN
실존하는 개념을 테이블로 모델링할 때 정규화가 진행되면서, 개념들이 작게 여러 테이블로 나누어지게된다. 나누어진 각각의 테이블을 서로의 관계를 이어주는 컬럼을 FK로 표기한다. 이걸 다시 유저에게 보여줄 때는 나누어져있던 테이블들을 하나로 합쳐야하는데 이때 사용하는것이 JOIN이다.

### INNER JOIN
![](https://www.w3schools.com/sql/img_innerjoin.gif)

PK와 FK로 관계가 존재하는 두 테이블에서 양쪽 모두에 존재하는 특정값을 찾을 때 사용한다.
```sql
select * from buytbl
	INNER JOIN usertbl
		ON buytbl.userID = usertbl.userID
	WHERE buytbl.userID = '000'
```
- 설명
	1. 구매내역테이블과 유저테이블을 INNER JOIN한다
	2. 유저의 PK값으로 ON 조건을 지정한다.
	3. JOIN된 테이블에서 고유값이 000인 row만 출력한다.
- 주의점
	- `select`할 컬럼들이 모든 컬럼이 아닌경우 `tableName.ColumnName` 이런식으로 테이블명과 컬럼명으로 지정하게된다. 이 때 중복적으로 쓰이는 테이블 때문에 가독성이 나빠지는데, 이를 위해 테이블명을 `AS 별칭`을 사용하여 줄일 수 있다.
	- 특정버전 (8.0.17)에서는 JOIN시 ORDER BY 없이도 자동으로 ORDER BY를 해주는 경우가
	있다.


### OUTER JOIN
![](https://www.w3schools.com/sql/img_fulljoin.gif)

- PK와 FK로 관계가 존재하는 두 테이블에서 양쪽 모두에 존재하는 특정값을 찾되, 한쪽 테이블 모두를 보여주기 위한 경우 사용한다. INNER JOIN보다는 활용빈도가 낮다.
- A 테이블에만 정보가 있는 경우, B테이블의 비어있는 row는 NULL이 된다.

```sql
select * from buytbl
	LEFT OUTER JOIN usertbl
		ON buytbl.userID = usertbl.userID
	WHERE buytbl.userID = '000';

select * from buytbl
	RIGHT OUTER JOIN usertbl
		ON buytbl.userID = usertbl.userID
	WHERE buytbl.userID = '000';

select * from buytbl
	FULL OUTER JOIN usertbl
		ON buytbl.userID = usertbl.userID
	WHERE buytbl.userID = '000';
```
- FULL OUTER JOIN같은 경우, N개의 테이블의 모든 컬럼을 가져와서 모든 ROW를 노출한다. (UNION은 세로로 길어지고, FULL OUTER JOIN은 가로,세로로 길어진다.)

### CROSS JOIN
![](https://www.w3resource.com/w3r_images/cross-join-example.png)
양쪽 테이블의 모든행을 JOIN시키는 기능; 결과 수 = (A 테이블의 row 갯수 * B테이블의 row 갯수)
```sql
select * from buytbl
	CROSS JOIN usertbl
	WHERE buytbl.userID = '000';
```
- 주의점
	- CROSS JOIN은 모든 row를 JOIN 하기 때문에 ON절을 쓸 수 없다.
	- CROSS JOIN의 결과는 두 테이블의 갯수를 곱한만큼이기 때문에 일반적으로는 COUNT를 하기위해서 사용한다.

### SELF JOIN
테이블 내부에 관계가 있어, 자기 자신을 JOIN하는것
```sql
select * from tableA as A
	INNER JOIN tableA as B
		ON A.COL1 = B.COL2
```

### UNION | UNION ALL | NOT IN | IN
- UNION
	- 앞뒤에 존재하는 테이블의 컬럼갯수와 타입이 서로 호환되는 데이터형식이여야한다.
	- 중복된 열은 제거된다.
```sql
-- UNION
SELECT * from talbeA
	UNION
SELECT * from talbeB
```

- UNION ALL
	- 앞뒤에 존재하는 테이블의 컬럼갯수와 타입이 서로 호환되는 데이터형식이여야한다.
```sql
-- UNION ALL
SELECT * from talbeA
	UNION ALL
SELECT * from talbeB
```

-> UNION은 비슷한 테이블을 합쳐 가로로 긴 데이터를 만들어낸다.


- IN, NOT IN
	- WHERE의 조건절로 사용된다. 특정 값이 데이터그룹에 존재하는지 확인한다.
	- IN 앞에 오는 컬럼의 갯수와 뒤에오는 값의 컬럼의 갯수가 일치하여야한다.
```sql
-- IN
SELECT * from talbeA
	WHERE COL1 IN (SELECT COL1 from talbeB)

-- NOT IN
SELECT * from talbeA
	WHERE COL1 NOT IN (SELECT COL1 from talbeB)
```

## SQL 프로그래밍
SQL도 다른 프로그래밍언어와 비슷한 분기, 흐름제어, 반복등의 기능이 있다.

```sql
DELIMITER $$
CREATE PROCEDURE 스토어드_프로시져이름()
BEGIN
END $$
DELIMITER;


CALL 스토어드_프로시져이름();
```
- 프로시져는 함수처럼 선언하고 함수처럼 가져다 쓸수있는 기능이다.

### IF ELSE
```sql
IF TRUE THEN
	... -- IF 의 조건으로 온 값이 참이면 실행되는 코드블럭
ELSE
	...
	-- IF 의 조건으로 온 값이 거짓이면 실행되는 코드블럭
END IF;
```

### CASE
- `IF ELSE`를 여러번 사용하게되면 가독성이 나빠진다. 이처럼 다중분기 처리시 사용한다. (switch문과 비슷)
- 주로 SELECT문에서 사용한다.
```sql
CASE
	WHEN 조건1 THEN
		...;
	WHEN 조건2 THEN
		...;
	WHEN 조건3 THEN
		...;
END CASE;


select
*,
SUM(price) as amount,
CASE
	WHEN SUM(price) > 500
		THEN 'DIA'
	WHEN SUM(price) > 100
		THEN 'GOLD'
	ELSE 'SILVER'
END
 AS 'rank'
 from sqldb.usertbl u
	JOIN sqldb.buytbl buy
		ON u.userID = buy.userID
	GROUP BY buy.userID;
```