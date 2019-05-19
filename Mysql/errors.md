## 해결못한 에러들

---



## 해결한 에러들

---

- data import 오류

  - 상황

    - data import 시 오류 문구발생 후 0 rows import
      - Incorrect string value: '\\xEB\\xA7\\x8C\\xEC\\x9B\\x90...' for

  - 해결방법

    - 테이블 캐릭셋 변경

    - ALTER TABLE (테이블명) convert to charset utf8;

      





