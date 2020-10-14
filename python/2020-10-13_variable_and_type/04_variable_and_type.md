# 4장 - 변수와 자료형

날짜: 2020년 10월 13일

### 변수

---

파이썬에서는 등호를 사용해 변수에 값을 할당한다.

- 주의점
    - 변수명은 숫자로 시작할 수 없다

        ex) `3star`

    - 공백을 포함할 수 없다.
    - 대소문자를 구분한다.
    - 밑줄(`_`)이외의 특수문자는 사용 불가능하다.
    - 다음과 같은 **예약어**는 변수로 사용할 수 없다.
        - None
        - False
        - True
        - and
        - as
        - assert
        - break
        - class
        - cotinue
        - def
        - del
        - elif
        - else
        - except
        - finnaly
        - for
        - from
        - global
        - if
        - import
        - lamda
        - nonlocal
        - not
        - or
        - pass
        - raise
        - return
        - try
        - while
        - with
        - yield
    - 동적언어이기 때문에 처음에 할당한 값의 타입과 다른 값을 할당해도 오류가 나지않음.
        - 최근 3.0 버전부터는 Type Annotaion을 지원한다. [링크](https://seorenn.tistory.com/77)

            ```python
            # variable_name: type
            some_value: int = 10
            ```

### 문자열

---

파이썬에서는 문자열을 표시하기위해 `"`, `'` 를 사용한다. (백팁은 없는게 좀 그렇다.) 문자열내에 특정 따옴표를 또 사용하고 싶은 경우, 문자열을 감싼 따옴표와 다른걸 사용하거나,  `\`  를 사용해야한다.

여러줄로 이루어진 문자열에는 `""" ... """` 를 사용한다.

파이썬의 문자열에서는 문자열에 곱하기 연산자를 사용할 수 있다. 문자열에 대해 곱하기 연산자는 반복하는 기능을 가진다.

```python
star = "*"
print(start * 3)
Out: ***
```

### 리스트

---

파이썬의 리스트는 대괄호를 사용해 만든다. 리스트에 들어가는 값들에 대해 데이터 타입은 자유롭다. (인터프리터 언어들은 다 그런가?...)

```python
score = [1,2,3]
```

- 특정 순서에 있는 값 조회

    ```python
    score[1] # 2
    score[-1] # 3
    ```

    조회하기위한 인수로 음수값을 넣게되면 뒤에서부터 순서를 세어 값을 가져온다.

    ⚠️ 배열의 길이를 초과하여 값을 조회하게되면 오류가 `indexError` 발생한다! (js에서는 undefined가 출력됬었는데..)

    ⚠️ 배열에서 특정값을 조회할 때, 인수를 `int` 타입으로 전달하지 않으면 오류가 발생한다. (js의 배열은 객체로 만들어진 유사배열여서 문자열로도 조회가 가능했나보다)

- 두 리스트 연결 (js의 Array.concat)

    ```python
    list01 = [1]
    list02 = [2]
    print(list01 + list02) # [1,2]
    ```

- 리스트 반복

    ```python
    list01 = [1]
    print(list01 * 3) # [1,1,1]
    ```

    → 문자열 연결과 반복과 비슷하게 작동한다.

- 리스트 범위 조회 (js의 Array.slice)

    `array[start_index:end_index]`

    - start_index~(end_index - 1)까지 조회

    `array[start_index:]`

    - start_index~last_index까지 조회

    `array[:end_index]`

    - start_index~end_index까지 조회

    `array[::step_index]`

    - step_index * n  인덱스들만 조회
        - ex) array[::3] → 0, 3, 6, 9 ... 번째 index 조회
- 리스트 특정 인덱스 삭제

    ```python
    list01 = [1,2,3]
    del list01[1] # [1,3]
    ```

- 리스트 특정값 존재 여부확인

    ```python
    list01 = [1,2,3]
    list01 in 1 # True
    ```

- 리스트 기본 내장 메소드
    - `append(value)`
        - 맨뒤에 인수를 삽입함 (js의 Array.push()와 동일함)
    - `insert(index, value)`
        - index위치에 value를 삽입함
    - `extend(values)`
        - 배열을 뒤에 삽입함
    - `remove(value)`
        - 가장 첫번째로 일치하는 값을 제거함
    - `pop()`
        - 가장 뒤에있는 value 제거후 반환 (js의 Array.pop()과 동일함)
    - `index(value)`
        - 가장 첫번째로 일치하는 값의 위치를 반환함 (js의 Array.indexOf()와 동일함)
    - `count(value)`
        - 리스트에서 일치하는 항목의 카운트를 반환함
    - `sort()`
        - 숫자나 문자열로 구성된 부분을 오름차순으로 정렬한 후 반환함
    - `reverse()`
        - 역순으로 정렬

⚠️ 리스트의 메소드는 원본데이터를 변형시킨다. 만약 원본데이터를 그대로 두고 싶다면 새로운 변수에 복사를 하여 사용하면 된다.

- [얕은복사와 깊은복사](https://wikidocs.net/16038)
    - 리스트의 아이템들이 immutable한 타입의 값인 경우 얕은복사만 하면 되지만, 아닌 경우에는 `list.deepcopy(array)` 메소드를 사용하면된다.
        - 얕은복사방법

            ```python
            >>> a = [1,2,3]
            >>> b = a[:]

            >>> id(a)
            4396179528
            >>> id(b)
            4393788808 # 변수의 메모리상 주소가 다르다는 것을 알 수 있다.
            >>> a == b
            True
            >>> a is b
            False
            ```

### 튜플

---

튜플은 리스트와 유사한 읽기 전용 데이터 타입이며 불변한 순서가 있는 객체의 집합이다. 튜플을 생성하기 위해서는 소괄호를 사용하거나 생략한다. 단 생략했을 경우에 튜플을 1개만 입력하는 경우 무조건 뒤에 콤마를 입력해야한다.

기존 튜플을 수정하는 코드나, 지우는 코드는 에러를 발생시킨다.

```python
tuple1 = (1,2,3,4)
tuple2 = 1,2,3,4 # 같은 코드
tuple3 = 1,

a = 1
b = 2
c = 3
tuple3 = a,b,c
print(tuple3) # (1, 2, 3)
```

만약, 튜플을 생성한 이후에 특정 값을 더 추가하고 싶은경우 연결연산자(`+`)를 사용하여 추가할 수 있다. 마찬가지로 반복연산자(`*` )도 사용가능함

```python
>>> t = t + (3 ,5)
>>> t
(1, 'korea', 3.5, 1, 3, 5)

>>> t * 2
(1, 'korea', 3.5, 1, 3, 5, 1, 'korea', 3.5, 1, 3, 5)
```

읽기전용이라고 해서 무조건  상수를 위한 값으로 사용할 필요는 없다.

```python
>>> def minmax(items):
...     return min(items), max(items)
...     # 괄호를 생략함
>>> minmax([7,5,2,1,11,15,55])
(1, 55)

# js의 destructuring과 비슷하게도 사용가능하다.
>>> lower, upper = minmax([7,5,2,1,11,15,55])
>>> lower
1
>>> upper
55

# 이러한 특징으로 파이썬은 이런것도 가능하다!
>>> a = '감자'
>>> b = '고구마'
>>> a, b  = b, a # 이런식으로 두변수사이의 값을 바꾸려면 대부분 언어에서는 임시 변수를 하나두어서 해결해야한다.
>>> a
'고구마'
>>> b
'감자'
```

iterable한 데이터들은 튜플로 변형할 수 있다.

```python
>>> tuple([1, 7, 5, 3, 9])
(1, 7, 5, 3, 9)
>>> tuple("abcde")
('a', 'b', 'c', 'd', 'e')
```