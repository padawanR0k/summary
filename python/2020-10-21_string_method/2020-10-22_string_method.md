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

- `str.find(text, start?, end?)`

    문자열에서 특정 문자를 가장 첫번째로 찾은 위치를 반환한다.  포함하지않는 경우 `-1`를 반환한다.

    `start, end` 인자를 전달하는 경우 검색하려는 범위를 제한할 수 있다.   `end` 인자는  생략가능하며 생략하면 `start~끝` 을 의미한다.

    ```python
    'python'.find('t')
    >> 2

    'python'.find('th')
    >> 2
    ```

- `str.starstwith(prefix, start?, end?)`

    문자열의 시작이 `prefix`  값과 일치하는 값으로 시작하는지 여부를 `Bool` 타입으로 반환한다

    ```python
    'python'.startswith('p') 
    >> True
    ```

- `str.endswith(suffix, start, end)`

    문자열의 끝이 `suffix`  값과 일치하는 값으로 끝나는지 여부를 `Bool` 타입으로 반환한다

    ```python
    'python'.endswith('n') 
    >> True
    ```

- `str.replace(old, new, count?)`

    문자열에서 `old` 값과 일치하는 문자열을 찾아서 `new` 값으로 바꾼다. `count` 인자를 전달하는 경우 바꾸는 횟수를 제한한다.

    ```python
    'python is python'.replace('python', 'js')
    >> 'js is js'
    'python is python'.replace('python', 'js', 1)
    >> 'js is python'
    ```

- `str.isalpha()`
    - 문자열이 숫자, 특수문자, 공백이 아닌 문자인지 여부를 `Bool` 타입으로 반환함

- `str.isdigit()`
    - 문자열이 모두 숫자인지 여부를 `Bool` 타입으로 반환함

- `str.isalnum()`
    - 문자열이 특수문자나 공백이 아닌 문자인지 여부를 `Bool` 타입으로 반환함

- `str.isspace()`
    - 문자열이 모두 공백인지 여부를 `Bool` 타입으로 반환함

- `str.isupper()`
    - 문자열이 모두 로마자 대문자인지 여부를 `Bool` 타입으로 반환함

- `str.islower()`
    - 문자열이 모두 로마자 소문자인지 여부를 `Bool` 타입으로 반환함

- `str.lower()`
    - 문자열을 모두 소문자로 변경함
- `ste.upper()`
    - 문자열을 모두 대문자로 변경함

## 텍스트 파일의 데이터를 읽고 처리하기

텍스트파일을 .csv로 저장해보자

- 목표
    - 공공데이터의 데이터를 가져와서  필요한 부분만 저장하여 파일 만들어보기
        - 가장 최신 분기 기준 각 구의 나이별 인구는 몇명인가?

- 데이터 출처
    - [서울시 주민등록인구 통계](https://data.seoul.go.kr/dataList/421/S/2/datasetView.do#none)

- 순서
    1. .txt파일을 다운로드 혹은 gitgist에 올린다.
        - [링크](https://gist.github.com/padawanR0k/ccc285fb7aaa94d814f5f859a8e9eac7)
    2. 파이썬에서 해당 파일을 불러온다.

        ```python
        import requests
        ...

        gistIUrl = 'https://gist.githubusercontent.com/padawanR0k/ccc285fb7aaa94d814f5f859a8e9eac7/raw/4e7a9e8b6a3d546d215fe23dcdd445dcf55f0f58/seoul_local_population_2.txt'

        def getGistcode():
            res = requests.get(url=gistIUrl)
            return res.text
        ```

    3. 원하는 데이터만 가져온다. (각 구의 총합만 가져오면됨)
        1. 필요없는 문자열을 없에거나 치환한다.
        2. 3번째 열이 `소계` 인 행만 가져온다.

        ```python
        def text_to_array(text):
            array = []
            rows = text.split('\n')    
            for i, r in enumerate(rows):
                row = []
                for j, col in enumerate(r.split('\t')):
                    col = col.replace(',', '')
                    col = col.replace('-', '0')
                    if col.isnumeric():
                        row.append(int(col))
                    else:
                        row.append(col)
                array.append(row)
            return array

        # 동 -> 구별로 데이터를 합침
        def zip_data(data):
            arr = []
            for i, row in enumerate(data):
                if i > 2:
                    # gu_arr = [] #년월, 구, 동
                    if i == 0 or row[2] == '소계':
                        row.pop(2)
                        arr.append(row) 
                
                else:
                    row.pop(2)
                    arr.append(row)
            
            return arr
        ```

    4. .csv 파일을 저장한다.

        ```python
        def arr_to_csv(array):
            path = pathlib.Path(__file__).parent
            print(path)
            f = open(str(path) + '/data.csv', 'wt', newline='')
            wr = csv.writer(f)
            wr.writerows(array)
            f.close()
                

        plain_text = getGistcode()
        arr = text_to_array(plain_text)
        simple_arr = zip_data(arr)

        simple_arr.pop(1)

        arr_to_csv(simple_arr)
        ```