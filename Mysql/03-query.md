## 기본적인 SQL 문법들
---

<br />

### SELECT
- CRUD에서 Read에 해당하는 개념이다. 테이블에서 원하는 컬럼을 조회한다. 부가적으로 사용할 수 있는 문법들이 많기 때문에 천천히 배워가자.
```sql
SELECT * FROM TableName;
SELECT COL1 FROM TableName;
```
- `SELECT` 바로 뒷부분에는 컬럼명이 올 수 있다. table에 존재하는 컬럼명이 될 수 도 있고, count()처럼 가상컬럼을 만들수 도 있다.
- `*`은 정규식표현에서의 개념과 비슷하게 `모든`컬럼을 가져온다.

#### WHERE 절
- 조회하는 결과에 조건을 걸기위해 사용한다.
- 걸수 있는 조건은 다양하다. 그중 일부만 알아보자

##### IN
- COL1 컬럼의 값이 `()` 내부에 있다면 해당 ROW 출력
```sql
SELECT * FROM TableName WHERE COL1 IN ("a", "b");
SELECT * FROM TableName WHERE COL1 IN (
	SELECT name FROM OtherTable
);
SELECT * FROM TableName WHERE (COL1, COL2) IN (
	SELECT name, age FROM OtherTable
);
-- 이처럼 IN 조건으로 지정할 부분에 서브쿼리를 작성할 수 있다. 서브쿼리가 WHERE문의 조건으로 오는 경우 서브쿼리의 결과 컬럼갯수가 IN 앞에 오는 컬럼 갯수와 동일해야 문법오류가 나지 않는다..
```

##### AND, OR
- COL1 컬럼의 값이 조건에 만족하면 해당 ROW 출력 (프로그래밍 언어에서의 AND, OR 연산과 동일 )
```sql
SELECT * FROM TableName WHERE COL1 = "a" AND COL2 = "ABC";
SELECT * FROM TableName WHERE COL1 = "a" OR COL1 =  "b";
```

##### BETWEEN
- COL1 컬럼의 값이 두 값 사이에 존재하면 출력
```sql
SELECT * FROM TableName WHERE COL3 BETWEEN 30 AND 40;
SELECT * FROM TableName WHERE COL3 >= 30 AND COL3 <=40; -- 결과가 동일함
```

##### LIKE
- COL1 컬럼의 값이 문자열이면서 입력한 조건에 만족하면 출력
- LIKE 기능을 `%` 또는 `_`와 같이 사용하면 컬럼의 인덱스가 존재해도 성능이 떨어질 수 있기 때문에 대용량 테이블에 사용시에는 비효율적임
```sql
SELECT * FROM TableName WHERE COL1 LIKE "a"; -- a 일떄만
SELECT * FROM TableName WHERE COL1 LIKE "a%"; -- 문자열이 a로 시작하는 값 모두
SELECT * FROM TableName WHERE COL1 LIKE "%DONG%"; -- DONG이라는 문자열을 포함한 값 모두
SELECT * FROM TableName WHERE COL1 LIKE "_a%"; -- 앞에 한글자가 존재하고 두번째 문자열이 a인  값 모두
```

#### 서브쿼리와 ANY, ALL, SOME
- WHERE 절의 조건으로 서브쿼리를 전달할 수 있다. 서브쿼리는 쿼리내부의 독립된 쿼리를 뜻한다.
- 서브쿼리를 남발하게 되면 쿼리 성능에 악영향을 끼치지 때문에 잘 사용하여야한다.
- 서브쿼리를 사용할 수 있는곳
	- SELECT
	- FROM
	- WHERE
	- HAVING
	- ORDER BY
	- INSERT문의 VALUES
	- UPDATE문의 SET
```sql
SELECT name FROM userTbl WHERE height = (SELECT height FROM userTbl WHERE name = "홍길동" )
-- 홍길동이라는 사람의 키와 같은 사람들의 이름을 가져오는 쿼리이다. 여기서 [홍길동이라는 사람의 키]를 가져오기 위해 서브쿼리를 WHERE절 뒤에 작성했다.
```
##### ANY, SOME
- 서브쿼리로 넘어온 값이 1개가 아니라 여러개일 떄, 그에 대해 1개만 조건이 만족해도 해당 ROW를 출력한다.
```sql
SELECT name FROM userTbl WHERE height >= ANY (SELECT height FROM userTbl WHERE name LIKE "김%" )
-- 성이 김씨인 사람들이 여러명이라는 가정하에
-- ROW의 height값이  김씨중 가장 작은사람 보다 크면 출력한다.
```
##### ALL
- 서브쿼리로 넘어온 값이 1개가 아니라 여러개일 떄, 그에 대해 모든 조건이 만족해야 해당 ROW를 출력한다.
```sql
SELECT name FROM userTbl WHERE height >= ALL (SELECT height FROM userTbl WHERE name LIKE "김%" )
-- 성이 김씨인 사람들이 여러명이라는 가정하에
-- ROW의 height값이  김씨중 가장 큰사람 보다 크면 출력한다.
```

#### ORDER BY 절
- 출력결과를 특정 조건에 의해 정렬한다.
```sql
SELECT name FROM userTbl ORDER BY height -- 기본정렬은 DESC (오름차순)임
SELECT name FROM userTbl ORDER BY height DESC
SELECT name FROM userTbl ORDER BY height ASC --(내림차순)
SELECT name FROM userTbl ORDER BY height ASC, name ASC -- 다중 정렬도 가능
-- height값을 기준으로 오름차순 출력
```

#### DISTINCT
- 결과에서 컬럼의 중복을 제외해준다.
```sql
SELECT DISTINCT name FROM userTbl; -- 동명이인이 있다면 그 중 한명만 출력해준다.
```

#### LIMIT
- 결과의 갯수를 제한한다.
```sql
SELECT * FROM userTbl LIMIT 10; -- 10개만 노출한다.
SELECT * FROM userTbl ORDER BY age ASC LIMIT 10; -- 가장 나이많은 사람 10명을 출력한다.
SELECT * FROM userTbl ORDER BY age ASC LIMIT 5,10; -- 가장 나이많은 사람으로 정렬하여 10위부터 5개를 출력한다.
```

#### GROUP BY 절
- 출력 결과를 특정 기준으로 그룹화해준다.

```sql
SELECT * FROM userTbl GROUP BY age -- 출력 결과를 age기준으로 묶어준다.
SELECT sum(amount) FROM userTbl GROUP BY age -- 출력 결과를 age기준으로 그룹화 하면서  그룹화된 amount라는 컬럼들의 합을 구한다.
-- 만약 각 유저가 얼마나 쇼핑몰에 돈을 소비했는지 알고 싶은 경우, 유저의 PK값으로 구매내역 테이블을 GROUP BY 해서 출력하면 될것이다.
SELECT pk, name, sum(amount) FROM PaymentTbl GROUP BY pk
```

##### 집계함수 SUM, MAX, MIN, COUNT
```sql
SELECT SUM(amount) FROM userTbl  -- 모든 amount를 합한 값을 출력한다.
SELECT MIN(age) FROM userTbl  -- 가장 작은 age 값을 출력한다.
SELECT MAX(age) FROM userTbl  -- 가장 큰 age 값을 출력한다.
SELECT age, COUNT(age) FROM userTbl GROUP BY age -- 출력 결과를 age기준으로 묶고, 각 나이 마다 몇개의 ROW가 있는지 카운트하여 출력한다.
```

#### HAVING 절
- 출력결과를 특정 조건에 의해 정렬한다. 단, 집계함수들과 같이 쓰인다.
- `WHERE` 절과 비슷한 개념이지만 `GROUP BY` 다음 절에 사용된다.
```sql
SELECT name, sum(price) FROM userTbl WHERE SUM(price) > 10000  GROUP BY name -- 해당 쿼리는 문법 오류가 발생한다.

SELECT name, sum(price) FROM userTbl   GROUP BY name HAVING SUM(price) > 10000
```

#### ROLLUP
- 총합 또는 중간 합계가 필요한 경우 `WITH ROLLUP`문을 사용한다.
```sql
SELECT num, SUM(price * amout) AS 'cost'  FROM paymentTbl GROUP BY groupName, num WITH ROLLUP;
-- groupName로 그룹화하여 각 비용을 합산한 값을 출력한다.
```


### CREATE
- DB에 특정 구조를 생성한다.
```sql
CREATE TABLE TableName; -- 테이블을 생성한다.
CREATE database DbName; -- 데이터베이스를 생성한다.
```

### USE
- 입력한 쿼리를 작동시킬 스키마를 지정한다.
- mysql workbench를 사용하는 경우 [Navigator] - [Schemas]탭에서 우클릭후 [Set as Default Schema]로 기본 스키마 지정하면 매번 `USE SchemaName;`를 안써줘도된다.
```sql
USE SchemaName;
```


### SHOW, DESC
- SELECT와 비슷하지만, SHOW, DESC는 현재 DB의 정보와 상태를 보기 위해 사용한다.
```sql
SHOW DATABASES; -- 현재 서버의 스키마 목록을 조회한다.
SHOW TABLE STATUS; -- 현재 서버의 스키마 목록의 상태를 조회한다. (엔진, 행 갯수 등)

DESC TableName; -- 현재 테이블의 열에 대한 정보를 조회한다. (컬럼명, 타입, 조건, 기본값 등)
```