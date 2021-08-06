# Libft

[](https://wiki.42seoul.work/ko/subjects/libft)

## 요약

- 기존에 있는 함수를 구현하거나 요구하는 함수를 작성하기
- 나만의 라이브러리 만들기

## todo list

- [x]  memset
- [x]  bzero
- [x]  memcpy
- [x]  memccpy
- [x]  memmove
- [x]  memchr
- [x]  memcmp
- [x]  strlen
- [x]  strlcpy
- [x]  strlcat

- [x]  strchr
- [x]  strrchr
- [x]  strnstr
- [x]  strncmp
- [x]  atoi
- [x]  isalpha
- [x]  isdigit
- [x]  isalnum
- [x]  isascii
- [x]  isprint
- [x]  toupper
- [x]  tolower

---

- [x]  calloc
- [x]  strdup

---

function part 2

- [x]  ft_substr
- [x]  ft_strjoin
- [x]  ft_strtrim
- [x]  ft_split
- [x]  ft_itoa
- [x]  ft_strmapi
- [x]  ft_putchar_fd
- [x]  ft_putstr_fd
- [x]  ft_putendl_fd
- [x]  ft_putnbr_fd

---

bonus part

- [x]  ft_lstnew
- [x]  ft_lstadd_front
- [x]  ft_lstsize
- [x]  ft_lstsize
- [x]  ft_lstadd_back
- [x]  ft_lstdelone
- [x]  ft_lstclear
- [x]  ft_lstiter
- [x]  ft_lstmap

## 정리

### memset

- 매개변수로 받은 참조변수에 정해진 수 만큼, 매개변수로 받은 값으로 초기화한다 (메모리 블록을 체운다)
- 여기서 메모리 블록은 1바이트 기준이다.
    - 1바이트 기준으로 메모리 블록을 체우기 때문에, 0이 아닌 int형을 두번째 매개변수로 전달하게 되면 4byte씩 채워져서 의도하지않은 결과가 생긴다.
        - 문자열은 1byte크기니까 사용해도됨
        - 4byte들은 memset으로 초기화하지 말것.

    ```c
    char arr[100] = "abcdefghijk";
    memset(arr, 'a', 2); // aacdefghijk

    char arr[100] = "abcdefghijk";
    memset(arr, 1, 2); // cdefghijk

    int arr4[100] = {1,2,3};
    ft_memset(arr4, 10, 1); // 10 2 3

    // 첫번째 인자와 두번째 인자의 타입을 맞춰야하는듯?
    ```

- 링크

    [[C, C++] memset 함수 사용하기](https://twpower.github.io/79-usage-of-memset-function)

    - memset 함수를 사용하는 이유

    [memset 사용시 주의할 점](https://minusi.tistory.com/entry/memset-%EC%82%AC%EC%9A%A9%EC%8B%9C-%EC%A3%BC%EC%9D%98%ED%95%A0-%EC%A0%90)

    - 메모리 블록을 채운다는 개념

    [[C언어] 메모리 관련 함수 설명 및 예제(memset, memcpy, memcmp, memchr)](https://reakwon.tistory.com/88)

### bzero

- memset인데 0만 가능함

### memcpy

- [[C언어/C++] memcpy 메모리 복사 함수 설명 및 예시](https://blockdmask.tistory.com/442)

- ***주의할점 1**
    - 길이를 계산할때 **char* 타입의 C언어 문자열 형태의 문자열**의 전체를 복사할때는 맨 뒤에 문자열의 끝을 알리는 **"\0"의 길이도 계산**해서 넣어야하기 때문에 **+1의 길이**만큼 해주어야합니다. 아래 예제에서 확인하시죠.
    - memcpy는 source 메모리 블록과 dest의 메모리 블록이 겹쳐져 있는 곳에서는 사용하지 못합니다. 즉 복사할 메모리랑, 복사 한 결과값을 붙여넣을 메모리가 겹쳐져 있다면 함수가 제대로 작동하지 않습니다. 만약 동일한 메모리 공간에 덮어씌워야 한다면 memmove 함수를 사용하면 됩니다.memmove 함수 **[[바로가기]](https://blockdmask.tistory.com/444)but, 요즘엔 memcpy, memmove 둘다 동일하게 작동하긴 합니다.**

### memccpy

- memccpy(3)는 src 데이터를 n바이트의 데이터를 dest에 복제할 때에 src 데이터에서 문자 c를 만나면 c까지 복제하고 복제를 중단합니다. 복제된 dest변수에서 복제가 끝난 다음 번지를 return합니다. 만약 문자 c를 만나지 않았다면, n바이트를 복제하고 NULL을 return합니다.

- [memccpy(3) - memory 영역간 데이터 복제(특정 문자까지)](https://www.it-note.kr/66)

- [[C언어] 메모리 관련 함수 설명 및 예제(memset, memcpy, memcmp, memchr)](https://reakwon.tistory.com/88)

### memmove

- 메모리를 이동시킨다.
- memcpy와 다른점
    - memcpy
        - A → B
    - memmove
        - A → BUFFER → B
        - 더 안전성이 좋음

- [[C언어/C++] memmove 메모리 이동 함수 설명 및 예시](https://blockdmask.tistory.com/444)

- 내가 잘못한 부분
    - dst === src + 1 === new_dst
        - src 한칸 밀려진 dst를 dst에 다시 넣고 있음

        ```c
        복사할 글자: l
        src: lorem ipsum dolor sit amet
        복사할 글자: l
        src: llrem ipsum dolor sit amet
        복사할 글자: l
        src: lllem ipsum dolor sit amet
        복사할 글자: l
        src: llllm ipsum dolor sit amet
        복사할 글자: l
        src: lllll ipsum dolor sit amet
        복사할 글자: l
        src: llllllipsum dolor sit amet
        복사할 글자: l
        src: lllllllpsum dolor sit amet
        복사할 글자: l
        src: llllllllsum dolor sit amet
        복사할 글자: l
        src: lllllllllum dolor sit amet
        llllllllum dolor sit a%
        ```

    - dest, src가 겹치는 경우 문제발생한 것
    - src, dst 모두 null일 경우 로직을 진행하면 안됨.

### memchr

- js의 indexOf와 거의 동일함

### memcmp

- strcmp와 비슷하나, strcmp는 문자열 비교시에 사용하고 memcmp는 문자열이 아닌것을 비교할 때 사용할 것

    [c 함수 strncmp 와 memcmp vs strcmp.](http://forum.falinux.com/zbxe/index.php?document_srl=533273&mid=lecture_tip)

    - memcmp는 널값이 나와도 n만큼 전부 검사한다.



### strlcpy

- null값을 보장하는 복사

    ### `strncpy` vs  `strlcpy`

    - `strncpy` 널 종단 문자를 보장하지 않는다.
    - `strlcpy` 널 종단 문자를 보장한다.
        - [http://blog.naver.com/PostView.nhn?blogId=i1004me2&logNo=140152740726](http://blog.naver.com/PostView.nhn?blogId=i1004me2&logNo=140152740726)

    [https://blog.dasomoli.org/478/](https://blog.dasomoli.org/478/)

### strlcat

- null값을 보장하는 이어붙이기

### strchr

- indexOf와 비슷하다.
- 찾으려는 문자가 `\0` 일 경우 반환되는 값은 첫번째 매개변수의 `\0` 이여야한다.

### strrchr

- strrchr와 비슷하나 거꾸로한 버전
- 찾으려는 문자가 `\0` 일 경우 반환되는 값은 첫번째 매개변수의 `\0` 이여야한다.

### strnstr

- strstr과 비슷 문자열내에서 문자열을 찾되 특정 인덱스까지만 찾음.
- 문자열을 찾는 로직을 기존에 작성한 ft_memcmp로 대체하여 짧게만듦
- [슬랙에서 추가적인 테스트케이스 공유받음](https://42born2code.slack.com/archives/CU6MU5TB7/p1620534218447100)

    ```c
    gcc -o test libft.a ft_strnstr.c && ./test
    ```

    ```c
    #include <string.h>
    #include <stdio.h>
    int main()
    {
    	char *in1;	// 첫 번째 인자로 넘길 문자열
    	char *in2;	// 두 번째 인자로 넘길 문자열
    	size_t num; // 세 번째 인자로 넘길 size_t
    	char *temp1;
    	char *temp2; // NULL 값 입력시 원래 자리 회복을 위한 변수
    	in1 = (char *)malloc(sizeof(char) * 100);
    	in2 = (char *)malloc(sizeof(char) * 100);
    	temp1 = in1;
    	temp2 = in2;
    	while (1)
    	{
    		printf("usage:\t<in1>\t<in2>\t<num>\n");
    		scanf("%s %s %ld", in1, in2, &num);
    		if (ft_strncmp(in1, "\\0", 5) == 0)
    			ft_strlcpy(in1, "", 1); // "\0" 가 오면 in1을 빈문자열로 치환
    		if (ft_strncmp(in2, "\\0", 5) == 0)
    			ft_strlcpy(in2, "", 1); // "\0" 가 오면 in2를 빈문자열로 치환
    		if (ft_strncmp(in1, "NULL", 10) == 0)
    			in1 = 0; // "NULL" 이 오면 in1을 NULL로 치환
    		if (ft_strncmp(in2, "NULL", 10) == 0)
    			in2 = 0; // "NULL" 이 오면 in2를 NULL로 치환
    		printf("-----input----\n");
    		printf("in1:\t#%s#\n", in1); // input 확인
    		printf("in2:\t#%s#\n", in2);
    		printf("num:\t#%ld#\n", num);
    		printf("----result----\n"); // result 비교
    		printf("strnstr:\t#%s#\n", strnstr(in1, in2, num));
    		printf("ft_strnstr:\t#%s#\n", ft_strnstr(in1, in2, num));
    		p
    rintf("--------------\n");
    		if (!in1 || !in2)
    		{
    			in1 = temp1;
    			in2 = temp2;
    		}
    	}
    }
    ```

### strncmp

- memcmp와 매우 비슷함
    - 대신 \0이 나타나면 검색을 중지함

        ```c
        strncmp() is designed for comparing strings rather than binary data,
             characters that appear after a `\0' character are not compared.
        ```

- [strncmp 함수에 대하여](https://m.blog.naver.com/PostView.nhn?blogId=tipsware&logNo=221415178947&proxyReferer=https:%2F%2Fwww.google.com%2F)

- [c 함수 strncmp 와 memcmp vs strcmp.](http://forum.falinux.com/zbxe/index.php?document_srl=533273&mid=lecture_tip)

### atoi

- 기존에 했던거랑 조금 다름
- 규칙
    - +-는 하나만 올 수있음
        - 중간에 오면 그전까지 숫자를 변환함
    - 0으로시작하는 2자리이상의 숫자열은 0으로 치환
    - 중간에 갑공백 또는 숫자가 아닌 문자오면 그 전까지 치환
    - 숫자앞부분에 공백이 있으면 해당 공백은 무시한다.
    - 확인해봐야할 테스트 케이스
        - 0
        - 01-1
        - 빈칸
        - a10
        - 10a

- 슬랙

### calloc

- malloc 써서 메모리 확보 + 0으로 초기화
- 가장 작은 단위인 char로 바꾸고 1바이트씩 0으로 초기화 해줌

### substr

- js String.substr 이랑 똑같음
- 세부로직은 다시 짜기 싫어서 ft_memcpy 재활용
- unit-test) protected와 not protected 차이?
    - libft_unit_test에서 함수가 protected 되지 않았다는 게 어떤 의미 일까요? 객체에서 protected와 비슷하다고 보면 될까요? 이 부분을 모두 protected로 고쳐야 하는지 혹시 알려주실 수 있나요??
- 주의
    - 마지막 파라미터가 0인 경우 0을 반환하는게 아님. 잘라낼 문자가 없긴해도 할당한 메모리를 넘겨주긴해야 통과함 (libft-war-machine)

### strjoin

- 피신때와 다르게 seperoatr가 없음
- 이것 또한 ft_memcpy 재활용
- 주의
    - 두개의 파라미터를 받는데 이 때 하나라도 널값인 경우 널값을 반환해야한다.
    - 문자열의 길이를 알아내는 함수를 쓰기전에 먼저 해당값이 널값인지 확인해야함 (정확히 왜그런지는 잘모르겟다.)
        - 이거랑 관련 있는듯함

            [strlen not checking for NULL](https://stackoverflow.com/questions/5796103/strlen-not-checking-for-null)

```
int main()
{
	printf("%s\n", ft_strtrim("abqbc", "abc"));
	printf("%s\n", ft_strtrim("xavocadoyz", "xyz"));
	return 0;
}
```

![https://media.vlpt.us/images/jungjaedev/post/ef5161a2-4944-41b9-899f-1aa3e9b3211d/strtrim.png](https://media.vlpt.us/images/jungjaedev/post/ef5161a2-4944-41b9-899f-1aa3e9b3211d/strtrim.png)

### ft_strtrim

- 문자열 앞뒤를 잘라낸다. js에서 trim은 공백을 없에지만 여기서는 두번째 인자로 받은 문자열내에 포함되면 다 잘라낸다.
- 만약 문자열이 모두 잘라졌을 경우 빈 문자열을 반환해야한다.
    - `ft_strtrim("           ", ""); // ""`
- 빈값 내뱉을때 오류남

```c
char			*ft_strtrim(char const *s1, char const *set)
{
	size_t	start_idx;
	size_t	end_idx;
	char	*str;
	int		len;

	start_idx = get_front_idx(s1, set); // 잘라낼 문자열의 시작 인덱스 가져옴
	if (s1 == 0) //. 자를 문자열이 널값이면 그냥 그대로 반환함
		return (char *)(s1);
	if (start_idx == (size_t)ft_strlen((char *)s1)) // 탐색시 문자열 끝까지 이동함 -> 모두 잘림
		return (ft_strdup(""));
	end_idx = get_back_idx(s1, set);
	len = end_idx - start_idx + 1;
	str = (char *)malloc(sizeof(char) * (len) + 1);
	if (!str) // 할당 실패
		return (0);
	if (!len)
		return (ft_strdup(""));
	ft_strlcpy(str, (char *)s1 + start_idx, (end_idx - start_idx + 2));
	return (str);
}

// test case
int main()
{
	char *result;
	result = ft_strtrim(" ", 0); // null
	result = ft_strtrim("", " "); // "
	printf("[%s] \n", result);
	return 0;
}
```

### ft_split

- 피신때와 다르게 seperator의 길이가 1임
- 자르려는 첫번째 파라미터가 NULL 값인지 확인해야함
- libft-war-machine
    - 2,3,4 버스에러남.. → 원인을 모르겠음 ㅅㅂ
        - 원인은 멀록해줄때 문자열크기를 제대로 생각하지않음
- 주의
    - 중간에 할당 실패시 모두 멀록 프리해줘야함...

        중간에 메모리할당 실패시 그 전까지 할당한 모든것들을 free 해주지않으면 메모리 누수발생

        - [슬랙링크](https://42born2code.slack.com/archives/CU6MTFBNH/p1582659462127500)
            - 내용
        - [슬랙링크](https://42born2code.slack.com/archives/CU6MTFBNH/p1587525154488600)
            - 내용

                저도 sohpark님께 배우고 free 죄다 해준 다음에 널처리까지 해줬습니다.ㅋㅋ 댕글링 포인터라고 해서 free해준다음에 str=NULL;을 안해주면 메모리 누수가 발생한다고 하는걸 평가 받다가 알게 되었었어요

```c

char			**ft_split(char const *s, char c)
{
	char		**arr;
	int			idx;
	char const	*prev_str;

	idx = 0;
	prev_str = s;
	if (s == 0 || !(arr = (char **)malloc(sizeof(char *) * get_arr_size(s, c))))
		return (0);
	while (*s)
	{
		if (*s == c)
			s++;
		else
		{
			prev_str = s; // c가 아닌 문자열의 시작부분의 포인터를 저장한다
			while (*s && *s != c) // c를 만날때 까지 포인터를 뒤로 이동시킨다.
				s++;

			// 현시점에서는 prev_str와 s의 사이가 잘려질 문자의 시작과 끝이다.
			// 끝 - 시작 + 1 == 문자열의 길이임 +1 해주는 이유는 널종단문자때문에
			if (!(arr[idx] = (char *)malloc(sizeof(char) * (s - prev_str + 2))))
				return (free_arr(arr, idx)); // 실패시 모두 할당취소함
			ft_strlcpy(arr[idx], (char *)prev_str, s - prev_str + 1);
			idx++;
		}
	}
	arr[idx] = 0;
	return (arr);
	}
```

### strmapi

- 문자열에 함수를 적용시켜라

```
#include <stdio.h>int add(int a, int b)
{
	return a + b;
}

int mul(int a, int b)

{
	return a * b;
}

int main()
{
	int(*fp)(int, int);

	fp = add;
	printf("%d\n", fp(10, 20)); //30

	fp = mul;
	printf("%d\n", fp(10, 20)); //200
}
```

[ft_strmapi](https://velog.io/@jungjaedev/ftstrmapi)

### ft_lstmap

- 새로운 객체를 위한 메모리 할당 실패시 전부 할당 해제해야함
    - [https://42born2code.slack.com/archives/CU6MU5TB7/p1602501707036900?thread_ts=1602500619.033100&cid=CU6MU5TB7](https://42born2code.slack.com/archives/CU6MU5TB7/p1602501707036900?thread_ts=1602500619.033100&cid=CU6MU5TB7)

        이는 `ft_split`, `ft_lstmap`에서 `free` 함수가 허용된 이유입죠

        3개의 반응make

- del값이 null 일 경우 어떻게 처리해야하는지???
- f함수는 기존의 content 값을 수정하기 위해 사용함
- 새로운 리스트의 첫번째노드를 변수에 저장하고
- 새로운 아이템을 저장하기 위한 변수를 새로 만듬.

### ft_lstclear

- 단방향 링크드 리스트여서  앞부터 free시키면 오류발생함
    - 역순으로 순회할 방법을 재귀함수로 해결함

### ft_itoa

- unsigned int여도 부호값이 살아있는듯함
    - int와 계산을 같이하는 경우 다시 int로 바뀌어서 그런건가?

---

## etc

- [왜 unsigned char를 쓰는가](https://kldp.org/node/75686)
    - 모든 비트가 value 값임
    - 의도하지 않은 결과를 피할수  있다. (ex. 127이상 값)

- 과제 진행 방법
    1. 1pdf에서 구현해야하는  함수들 체크
    2. 메뉴얼 + 구글링으로 구현
    3. Makefile SRCS 에 추가
    4. libft.h 헤더에 프로토타입 추가
    5. LibfTest로 단위 테스트

        ```c
        bash grademe.sh {함수명} -f
        bash grademe.sh ft_memccpy -f
        ```

### 라이브러리 만들기

- ar 을 사용해서 만들어야함
- bonus를 제출할려면 bonus 목표갸 필요함

- [Linux의 ar 명령을 사용하여 정적 라이브러리를 만드는 방법 - 최신](http://choesin.com/linux%EC%9D%98-ar-%EB%AA%85%EB%A0%B9%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EC%A0%95%EC%A0%81-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC%EB%A5%BC-%EB%A7%8C%EB%93%9C%EB%8A%94-%EB%B0%A9)

## `Makefile`

### 요약

- make 유틸리티는 일정한 규칙을 준수하여 목표파일을 만들어낸다.
- 일정한 규칙은 `Makefile` 파일내부에 작성하는 것

### 메이크파일 만들기

```bash
gcc -o foo foo.c bar.c # foo를 만들기위한 쉘스크립트
```

- 메이크파일로 대체해보자

    ```
     foo:   foo.o bar.o
            gcc -o foo foo.o bar.o

     foo.o: foo.c
            gcc -c foo.c

     bar.o: bar.c
            gcc -c bar.c

    ```

    ---

    입력하는데 주의하실 것이 있습니다. 자, 위 화일을 보십시요. 형식은 다음과 같습니다.

    ---

    ```
     목표:  목표를 만드는데 필요한 구성요소들...
            목표를 달성하기 위한 명령 1
            목표를 달성하기 위한 명령 2
            ...

    make foo
    -> foo를 만들기 위해 필요한 구성요소는 [foo.o] [bar.o]가 있다.
    -> foo.o 를 만들기 위한 구성요소는 foo.c가 있다.
    -> bar.o 를 만들기 위한 구성요소는 bar.c가 있다.

    약간 함수처럼 작동하게 된다. 목표는 함수같은 느낌으로 호출된다. 필요한 구성요소는 함수의 파라미터로 작동하는 느낌?
    ```

    - 중요! 중요! 명령줄에서 다음 줄로 넘어간 후,  <탭>키를 누릅니다. 꼭 한 번 이상은 눌러야 합니다. 절대 스페이스키나 다른 키는 사용해선 안됩니다.

- 메이크파일은 똑똑한 빌드를 진행해준다.
    - 특정 목표를 위해 1번파일과 2번파일이 필요하다고하자
    - make 명령어를 진행한 후 1번파일만 수정하고 다시 명령어를 실행시키면 1번파일에 대한 목표들만 다시 실행된다. (필요한 파일들만 다시 빌드하는 것들이 약간 웹팩대체제들이랑 비슷한 느낌이다.)
- make all 은 관념적인 네이밍이다.
    - 말 그대로 필요한 모든것을 만들라는 명령임

        ```bash

        $ make foo1
                $ make foo2
        #---------------- 대신에

        all: foo1 foo2 foo3
         foo1: <생략>
         foo2: <생략>
         foo3: <생략>
        ```

### 매크로

- 재활용할 목적으로 코드뭉치를 변수화하는 것
- `${}` `$()` `$..` 로 값을 불러와서 사용 가능하나 `$()` 권장
- `make -p` 는 이미 정의된 매크로들을 보여주며 해당 매크로들은 재정의 가능

### 꼬리말 규칙, 패턴 규칙

```
 .c.o:
        gcc -c ${CFLAGS} $<
```

- `.c.o:`
    - c 를 입력화일로 받고 .o 화일을. 만든다 (꼬리말 규칙 == Suffix rule)
    - .c.o 라는 전통적인 표현 말고 GNU 버전 (확장문법중 - 패턴 규칙)
        - `%.o: %.c`

### 내부 매크로

- `${CFLAGS}`
    - 메이크파일 내부에 작성된 변수 == 매크로 기능
    - 매크로 기능은 중복되는 코드를 줄일때 사용하면 좋다. 복잡한 메이크 파일의 경우 옵션을 주는 부분을 매크로로 관리하면 편해질수 있다. (변수화)
- `$<`
    - 입력을 받는다. 여기서는 `목표`란에 입력된 코드에 의해 `.c`들이 입력으로 들어오게 되는것임

```bash
%_dbg.o: %.c
        gcc -c -g -o $@ ${CFLAG} $<

 DEBUG_OBJECTS = main_dbg.o edit_dbg.o

 edimh_dbg: $(DEBUG_OBJECTS)
        gcc -o $@ $(DEBUG_OBJECTS)
```

- `$@`
    - 목표의 이름 출력할 파일명이된다.
    - 출력 화일을 의미합니다. 콜론의 왼쪽에 오는 패턴을 치환합니다.
    - 예시
        - %_dbg.o 라는 표현을 잘 보십시요. foobar.c 라는 입력화일(%.c)이 있다면 % 기호는 foobar 를 가리키므로 %_dbg.o 는 결국 foobar_dbg.o 가 됩니다.
- `$<`
    - 입력 화일을 의미합니다. 콜론의 오른쪽에 오는 패턴을 치환합니다.
    - $< 는 현재의 목표 파일보다 더 최근에 갱신된 파일 이름이라고 하였다. .o 파일보다 더 최근에 갱신된 .c 파일은 자동적으로 컴파일이 된다. 가령 main.o를 만들고 난 다음에 main.c를 갱신하게 되면 main.c는 $<의 작용에 의해 새롭게 컴파일이 된다.
- `$*`
    - 입력 화일에서 꼬리말(.c, .s 등)을 떼넨 화일명을 나타냅니다.
    - 확장자가 없는 현재의 목표 파일(Target)
- `$?`
    - 현재의 목표 파일(Target)보다 더 최근에 갱신된 파일이름

### 매크로 치환

`$(MACRO_NAME:OLD=NEW)`

- 매크로 사용시, 매크로 그대로를 사용하는 것이 아닌 일부를 수정해야할 필요가 있을때 사용한다.

```bash
OBJS = main.o read.o write.o
SRCS = $(OBJS:.o=.c) # 이것은 SRCS = main.c read.c write.c 와 같다.
```

### 주의사항

- 여러개의 명령을 실행시키려는 경우 가로로 길어지게 쓰는것보다는   `\`를 써서 개행을 하면서 쓰자
- 임시변수? 를 사용할땐 `@` 를 참조할땐 `$$` 를 사용

    ```
     all:
             @HELLO="안녕하세요?"; echo $$HELLO

    ```

    ---

    명령의 맨 처음에 @ 문자를 붙여봅시다.

    ```
     $ make
     안녕하세요?
    ```

- 구성

    ```
    .SUFFIXES : .c .o     --+
    CFLAGS = -g             |
                            |
    OBJS = main.o \         |
    read.o \                | 매크로 정의 부분
    write.o                 |
    SRCS = $(OBJS:.o=.c)    |
                            |
    TARGET = test         --+

    $(TARGET): $(OBJS)                    --+
                    $(CC) -o $@ $(OBJS)             |
    dep :                                   |
                    gccmakedpend $(SRCS)            |
    new :                                   | 명령어 정의 부분
                    touch $(SRCS) ; $(MAKE)         |
    clean :                                 |
                    $(RM) $(OBJS) $(TARGET) core  --+

    - 여기부터 의존관계 부분
    ```

### Rule

- 파일명을 일치 시킬때, 일치시키는 방식
- rules
    - Explicit
        - 와일드 카드나 내부 변수 없이 직접 지정하는것
    - Pattern
        - 명확하게 타겟이나 와일드카드를 사용한 특정한 패턴 형태
    - Implicit
        - 내부 변수나 makefile database를 이용한 것
    - Static parttern
        - Target 파일에 대한 리스트가 한정적으로 지정되어 있을 경우에 해당한다.

내가 작성한거 주석 추가

```makefile

CFLAGS=-Wall -Wextra -Werror # gcc 컴파일 옵션
SRCS=\
	ft_memset.c \
	ft_bzero.c \
	ft_memcpy.c \
	ft_memccpy.c \
	ft_memmove.c \
	ft_memchr.c \
	ft_memcmp.c \
	ft_strlen.c \
	ft_strlcpy.c \
	ft_strlcat.c \
	ft_strchr.c \
	ft_strrchr.c \
	ft_strnstr.c \
	ft_strncmp.c \
	ft_isalpha.c \
	ft_isdigit.c \
	ft_isalnum.c \
	ft_isascii.c \
	ft_isprint.c \
	ft_toupper.c \
	ft_tolower.c \
	ft_atoi.c \
	ft_calloc.c \
	ft_strdup.c \
	ft_substr.c \
	ft_strjoin.c \
	ft_strtrim.c \
	ft_split.c \
	ft_itoa.c \
	ft_strmapi.c \
	ft_putchar_fd.c \
	ft_putstr_fd.c \
	ft_putendl_fd.c \
	ft_putnbr_fd.c \
	ft_lstnew.c \
	ft_lstadd_front.c \
	ft_lstsize.c \
	ft_lstlast.c \
	ft_lstadd_back.c \
	ft_lstdelone.c \
	ft_lstclear.c \
	ft_lstiter.c \
	ft_lstmap.c \ # 라이브러리 생성에 필요한 .c 파일 이름들

BONUSSRCS=ft_strncpy_bonus.c \

NAME=libft.a
NFLAGS=-R CheckForbiddenSourceHeader
OBJS = $(SRCS:.c=.o) #매크로 치환 (.c를 .o 로 바꿈)
BONUSEOBJS = $(BONUSSRCS:.c=.o)

all : $(NAME) # all이라는 목표는. NAME이라는 의존성을 가짐

%.o : %.c # 패턴 매칭 목표. .o로 끝나는 목표는 .c로 끝나는 파일을 의존성으로 가짐
	gcc $(CFLAGS) -c $< -o $@ # $<은 콜론 오른쪽 즉, ft_*.c 들이 들어오고,
						                # $@는 콜론 왼쪽 즉 ft_*.o 가 들어옴

$(NAME): $(OBJS)
	ar -cr $@ $^ # $@는 콜론 왼쪽 즉,  libft.a  $ˆ는 현재 항목의 의존성 리스트 즉, $(OBJS) == ft_*.o


clean :
	rm -f $(OBJS)

fclean : clean
	rm -f $(NAME)

re : fclean all

bonus: $(OBJS) $(BONUSEOBJS)
	ar -cr $@ $^
```

---

### Makefile `.PHONY` target

- 다른 카뎃의 Makefile들을 참고하다가 보게됨
- Makefile의 목적(target)은 특정 파일을 만들기위해서 사용되기도 하지만 특정 파일이 안생기는 경우도 있음(clean, fclean)
- 특정 파일이 안생기는 == 목적파일이 없는 경우 make시 해당 내용을 매번 실행
- 또한 의존성이 없는 clean은 항상 최신일거라고 간주되며 확인하는 과정이 없다. 그래서 `clean` 이라는 파일을 만들게되면 `make clean`은 동작하지않게된다.
- `.PHONY` 지시자를 사용하여 특별한 타겟의 전제조건으로 만들어 준다.
    - `make clean` 명령은 `clean` 이라는 파일이 존재하는지 여부와 상관없이 명령을 수행할 수 있게된다

## Ref

- Makefile
    - [http://doc.kldp.org/KoreanDoc/html/gcc_and_make/gcc_and_make-3.html#ss3.1](http://doc.kldp.org/KoreanDoc/html/gcc_and_make/gcc_and_make-3.html#ss3.1)
    - [https://wiki.kldp.org/KoreanDoc/html/GNU-Make/GNU-Make.html#toc4](https://wiki.kldp.org/KoreanDoc/html/GNU-Make/GNU-Make.html#toc4)
    - [https://nanite.tistory.com/77](https://nanite.tistory.com/77)
    - [https://pinocc.tistory.com/131](https://pinocc.tistory.com/131)

## tester

- [ ]  [libftTester](https://github.com/Tripouille/libftTester)
- [ ]  [lift-war-machine](https://www.notion.so/libft-a9f03cf7bea44a7a94b0afa8d86815d1)
- [ ]  [libft-unit-test](https://www.notion.so/libft-a9f03cf7bea44a7a94b0afa8d86815d1)
- [ ]  [Libftest](https://www.notion.so/libft-a9f03cf7bea44a7a94b0afa8d86815d1)
