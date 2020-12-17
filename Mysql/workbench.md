> mysql workbench 사용시 이슈 정리


### 특정문구로 sql이 실행이 안되는 경우 강제로 실행시키기
- 오류
	- sql파일 내부에 `source ...` 문이 존재했는데 이 부분 때문에 문법오류가 발생하여 실행이 되지않는다.
- 해결방법
	-	[File]-[Run SQL Script]-[Run] ([링크](https://stackoverflow.com/questions/45227599/mysql-syntax-error-source-source-is-not-valid-input-at-this-position))


### 특정 table이 readonly상태인 이유
- 테이블에 PK가 존재하지 않는 테이블은 mysql workbench UI상 존재하는 수정가능한 테이블이 readonly상태이다.

