# 9장 - 문자열과 텍스트파일 데이터

날짜: Oct 21, 2020

파이썬은 문자열 처리를 위한 다양한 내장 메서드가 제공된다. 이를 활용해서  데이터를 다뤄보자

## 문자열을 다루기 위한 내장 메서드 정리

---

- `str.split([sep, ] maxsplit=N)`

    하나의 문자열을 인자로 받은 문자열을 기준으로 분리시켜 `list` 형태로 반환한다.  `maxsplit`을 입력한 경우 분리되었을 때 `maxsplit` 번째만큼만 분리된 값을 반환한다.

    ```python
    coffee = "에스프레소,아메리카노,카페라떼,카푸치노"
    print(coffee.split(","))
    >> ['에스프레소', '아메리카노', '카페라떼', '카푸치노']

    coffee = "에스프레소   아메리카노 카페라떼 \n 카푸치노"
    print(coffee.split()) # 공백, 줄바꿈으로 분리시킬때는 인수값을 생략가능하다 " " 생략함
    >> ['에스프레소', '아메리카노', '카페라떼', '카푸치노']
    ```

- `str.strip([chars])`

    문자열 앞뒤로 인자로 받은 `chars`  값과 일치하는 값이 있다면 지우고 문자열을 반환한다. 인자로 `ab`로 전달해도 일치하는 값을 찾을때는 한 문자씩 찾게된다. 

    ```python
    ' asd '.strip()
    >> 'asd'
    'apythona'.strip('a') 
    >> 'python'

    'apythonb'.strip('ab') # a와 b를 확인함
    >> 'python'
    'apythonb'.strip('ba') # a와 b를 확인함
    >> 'python'
    ```

- `str.lstrip([chars])`

    문자열 앞부터 인자로받은 `chars`값과 일치하는 값을 다른 문자열을 만날때 까지 지우고 문자열을 반환한다.

    ```python
    '  python'.lstrip()
    >> 'python'
    ```

- `str.rstrip([chars])`

    문자열 뒤부터 인자로받은 `chars`값과 일치하는 값을 다른 문자열을 만날때 까지 지우고 문자열을 반환한다.

    ```python
    'python   '.lstrip()
    >> 'python'
    ```

- `str.join(seq)`

    문자열을 항목으로 갖는 시퀀스를 구분자를  `seq` 값으로 지정하여 하나의 문자열로 만들어 반환한다.

    ```python
    address_list = ['서울시', '도봉구']
    address_list.join('') # 서울시도봉구
    address_list.join(' ') # 서울시 도봉구
    address_list.join('-') # 서울시-도봉구
    ```