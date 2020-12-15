

자바스크립트가 동작하는 핵심원리에 대해서 설명되어있다.

# 1. 실행 컨텍스트

 실행 가능한 코드가 실행되기 위해 필요한 환경

실행가능성이있는 코드를 실행하기 위해서는 환경이 필요하다. ex) 함수

- 전역 코드 : 전역 영역에 존재하는 코드

  - ```js
    var x = 1;
    console.log(x); // 다른언어에서는 함수호출없이 그냥 실행시키진않는 반면에 자바스크립트는 그냥 실행된다.
    ```

    ​

- Eval 코드 : Eval함수로 실행되는 코드

- 함수 코드 : 함수 내에 존재하는 코드



실행되는 본질은 다 똑같다

this는 함수가 어떻게 호출되었느냐에 따라 결정된다. 그 함수가 어떠한 환경에서 호출되었는가?

지역변수, 전역변수, 매개변수, arguments, 함수객체의 프로퍼티등과 같은 정보들을 어떻게 구조화해서 저장하고있는가?

이와 같이 실행에 필요한 정보를 형상화하고 구분하기 위해 자바스크립트 엔진은 실행 컨텍스트를 물리적 객체의 형태로 관리한다. 형상화한다는것이란 정보가 갖는 속성을 객체화한것이다.

아래의 코드를 살펴보자.

```js
var x = 'xxx';

function foo () { // 전역함수 foo()
  var y = 'yyy'; // 지역변수 y

  function bar () { // 내부함수 bar()
    var z = 'zzz'; // 지역변수 z
    console.log(x + y + z);
  }
  bar(); // foo() 함수 내부에서 bar()함수를 실행한다
}
foo(); // 전역에서 foo()를 실행한다.
```

 코드를 실행하면 아래와 같이 실행 컨텍스트 스택(Stack)이 생성하고 소멸한다.   큰 회색박스는 실행컨텍스트컨테이너를 시각화한것이다. 





![img](http://poiemaweb.com/img/ec_1.png)

global EC : 전역 실행컨텍스트

1. 전역실행컨텍스트에 코드가 한줄도 실행하기전에 전역실행컨텍스트객체가 쌓인다..


1. foo()를 만나면 foo()의 컨텍스트객체가 쌓인다.
2. bar()를 만나면 bar()의 컨텍스트객체가 쌓인다.
3. bar()함수가 끝나면 실행컨텍스트에서 bar()이 컨텍스트 객체가 사라진다.
4. foo()함수가 끝나면 실행컨텍스트에서 foo()이 컨텍스트 객체가 사라진다.
5. 결과적으로 전역실행컨텍스트객체만 남았다.

***

# 2. 실행 컨텍스트의 3가지 객체

실행 컨텍스트는 실행 가능한 코드를 형상화하고 구분하는 *추상적인 개념*이지만 물리적으로는 객체의 형태를 가지며 아래의 3가지 인터널프로퍼티를 소유한다.

> 인터널프로퍼티 : 내부적인 프로퍼티로 수정할 수 없는 프로퍼티이다. ex) `__proto__`

![img](http://poiemaweb.com/img/excute_context_structure.png)

- VO: 변수, 함수표현식을 관리한다.

- SC: 스코프에대한 정보를 관리한다.

- thisValue: this에 대한 정보를 관리한다.

  ​

```js
var EC = { // 예를 들자면 이렇다.
    VO : {변수,함수의 정보},
    SC : ,
    thisValue : 
}
```







---



## 2- 1. Variable Object (VO / 변수객체)

VO : 실행 컨텍스트가 생성되면 자바스크립트 엔진은 ***실행에 필요한 여러 정보들을 담을 객체를 생성한다***. 변수객체로 변수에 대한 정보를 관리한다. 

VO는 아래와 같은 정보를 담고있다.

- 변수(함수 표현식 포함)
- 매개변수(parameter)와 인수 정보(arguments)
- 함수 선언(**함수 표현식은 제외**)



1.  전역컨텍스트의 경우
   - 자기가 알아서 무조건 호출됨
   - 매개변수가 없다
   - 매개변수가 없어서 arguments가 없다





```js
var x = 'xxx';

function foo () {
  var y = 'yyy';

  function bar () {
    var z = 'zzz';
    console.log(x + y + z);
  }
  bar();
}
foo();
```



> 전역 컨텍스트의 경우

![ec-vo-global](http://poiemaweb.com/img/ec-vo-global.png)

함수명을 프로퍼티로 함수자체를 값으로 한다.



```js
var x = 1;
window.x === x; 

var a = b = 1; // 방향 <--
// b = 1이 먼저 실행  
// a = b
```





> 함수 컨텍스트의 경우
>
> Variable Object는 **Activation Object(AO / 활성 객체)**를 가리키며 매개변수와 인수들의 정보를 배열의 형태로 담고 있는 객체인 arguments object가 추가된다.

![ec-vo-foo](http://poiemaweb.com/img/ec-vo-foo.png)





---



## 2- 2. Scope Chain (SC)

스코프의 체인을 관리한다. 

내 스코프에서 찾았을때 없으면 프로토타입체이닝에 의해서 찾아가는것. 항상 최상위 스코프는 GO이다.

만들어진 함수에서 객체에서 메서드에서 원



![ec-sc](http://poiemaweb.com/img/ec-sc.png)

VO는 전역변수와 전역함수를 관리하기 때문에 GO내부의 foo()와  x를 참조한다.



SC(스코프체인)은 리스트를 가진다. 순서가있는 배열(객체)를 가진다.  자신이 참조할 수 있는 스코프를 순서대로 배열해 놓는다. 자신의 스코프를 최우선으로한다. 자신의 스코프에 찾는 변수가 없으면 다음 배열의 스코프로 찾아간다.



global EC의 SC(스코프체인)은 전역(GO)뿐이다.

foo() EC의 SC(스코프체인)은 전역과 자기의 스코프(AO)를 가르킨다.



---

## 2- 3. this value

this 프로퍼티에는 this 값이 할당된다. this에 할당되는 값은 함수 호출 패턴에 의해 결정된다.

메서드일때

생성자함수로 만들어진 인스턴스객체일때



***

# 3. 실행 컨텍스트의 생성 과정

```js
// (1)전역 코드에의 진입
var x = 'xxx';

function foo () {
  var y = 'yyy';

  function bar () {
    var z = 'zzz';
    console.log(x + y + z);
  }
  bar();
}

foo();
```



---

## 3- 1. 전역 코드에의 진입

컨트롤이 진입하기 이전에 GO(window)가 만들어진다. 이 객체는 단일 사본으로 존재하며 어디서든 **접근이 가능**하다. 코드가 종료되면 전역 객체의 라이프 사이클은 종료한다. (코드가 종료되는 경우에는 페이지이동, 브라우저 종료(탭종료) 하는 경우)

![초기 상태의 실행 컨텍스트](http://poiemaweb.com/img/ec_3.png)



전역 객체를 만들고 실행컨텍스트 스택에 넣는다.

![전역 실행 컨텍스트의 생성](http://poiemaweb.com/img/ec_4.png)

이후 아래의 순서로 실행이된다.

1. 스코프 체인(SC)의 생성과 초기화 :  SC의 값을 지정해준다.
2. Variable Instantiation(변수 객체화) 실행 
3. this value 결정


---

### 3- 1.1 스코프 체인의 생성과 초기화

실행 컨텍스트가 생성된 이후 가장 먼저 스코프 체인의 생성과 초기화가 실행된다.

전역실행컨텍스트(global EC)의 스코프체인(SC)가 전역객체(Global object)를 참조한다.이때 ***스코프 체인(SC)**은 전역 객체의 레퍼런스를 포함하는 리스트가* 된다.



![스코프 체인의 생성과 초기화](http://poiemaweb.com/img/ec_5.png)

---

### 3- 1.2 Variable Instantiation(변수 객체화) 실행

스코프 체인의 생성과 초기화가 종료하면 변수 객체화(Variable Instantiation)가 실행된다.

변수객체화(Variable Instantiation)는  변수객체(Variable Object)에 프로퍼티와 값을 *추가하는 것을 의미한다.*

![Variable Instantiation](http://poiemaweb.com/img/ec_6.png)

1. (함수인 경우) **매개변수(parameter)**가 변수객체(Variable Object)의 프로퍼티로, 인수(argument)가 값으로 설정된다.
2. 대상 코드 내의 **함수** 선언(함수 표현식 제외)을 대상으로 함수명이 변수객체(Variable Object)의 프로퍼티로, 생성된 함수 객체가 값으로 설정된다.(**함수 호이스팅**)
3. 대상 코드 내의 **변수** 선언을 대상으로 변수명이 변수객체(Variable Object)의 프로퍼티로, undefined가 값으로 설정된다.(**변수 호이스팅**)

```js
// (1)전역 코드에의 진입
var x = 'xxx';

function foo () {
  var y = 'yyy';

  function bar () {
    var z = 'zzz';
    console.log(x + y + z);
  }
  bar();
}

foo();
```

위 예제 코드를 보면 전역 코드에 변수 x와 함수 foo(매개변수 없음)가 선언되었다. Variable Instantiation의 실행 순서 상, 우선 2. 함수 foo의 선언이 처리되고(함수 코드가 아닌 전역 코드이기 때문에 1. 매개변수 처리는 실행되지 않는다.) 그 후 3. 변수 x의 선언이 처리된다.



---

#### 3- 1.2.1 함수 foo의 선언 처리

함수를 찾으면 함수명을 프로퍼티로 함수자체를 값으로

![함수 foo의 선언 처리](http://poiemaweb.com/img/ec_7.png)

생성된 함수 객체는 `[[Scopes]]` 프로퍼티를 가지게 된다.

`[[Scopes]]` 프로퍼티는 ***함수 객체만이 소유하는 내부 프로퍼티***(Internal Property)로서 함수 객체가 *실행되는 환경을 가리킨다.*

![함수 foo의 Scopes](http://poiemaweb.com/img/foo-scopes.png)

지금까지 살펴본 실행 컨텍스트는 아직 코드가 실행되기 이전이다. 하지만 스코프 체인이 가리키는 **변수 객체(VO)에 이미 함수가 등록되어 있으므로** 이후 코드를 실행할 때 함수선언식 이전에 함수를 호출할 수 있게 되었다.

이때 알 수 있는 것은 함수선언식의 경우, 변수 객체(VO)에 함수표현식과 동일하게 함수명을 프로퍼티로 함수 객체를 할당한다는 것이다. 단, 함수선언식은 변수 객체(VO)에 함수명을 프로퍼티로 추가하고 즉시 함수 객체를 즉시 할당하지만 함수 표현식은 일반 변수의 방식을 따른다. 따라서 함수선언식의 경우, 선언문 이전에 함수를 호출할 수 있다.

---

#### 3- 1.2.2 변수 x의 선언 처리

![변수 x의 선언 처리](http://poiemaweb.com/img/ec_8.png)

**var 키워드**로 선언된 변수는 **선언 단계와 초기화 단계가 한번에** 이루어진다. 다시 말해 스코프 체인이 가리키는 **변수 객체에 변수가 등록되고 변수는 undefined로 초기화**된다.  이러한 현상을 **변수 호이스팅**이라한다.



---

### 3- 1. 3 this value 결정

![this value 결정](http://poiemaweb.com/img/ec_9.png)

 this value가 **결정되기 이전에 this는 전역 객체를 가리키고 있다가** 함수 호출 패턴에 의해 **this에 할당되는 값이 결정**된다. 전역 코드의 경우, this는 전역 객체를 가리킨다.

**전역 컨텍스트(전역 코드)의 경우, Variable Object, 스코프 체인, this 값은 언제나 전역 객체이다.**



---

## 3- 2.  전역 코드의 실행

지금까지는 코드 실행 환경을 갖추기 위한 사전 준비, 코드의 실행은 지금부터 시작된다.

```js
var x = 'xxx';

function foo () {
  var y = 'yyy';

  function bar () {
    var z = 'zzz';
    console.log(x + y + z);
  }
  bar();
}

foo();

```

함수가 실행되면 함수컨텍스트가 만들어지고 그에 1대1로 대응하는 VO객체가 만들어지는데 그것을 AO(활성객체)라고 부른다.



---

### 3- 2.1 변수 값의 할당

전역 변수 x에 문자열 ‘xxx’를 할당할 때, 현재 실행 컨텍스트의 스코프 체인이 참조하고 있는 Variable Object를 선두(0)부터 검색하여 변수명에 해당하는 프로퍼티가 발견되면 값(‘xxx’)을 할당한다.

![변수 값의 할당](http://poiemaweb.com/img/ec_10.png)



---

### 3- 2.2 함수 foo의 실행

![함수 foo의 실행 컨텍스트 생성](http://poiemaweb.com/img/ec_11.png)



1. 전역 코드의 함수 foo가 실행되기 시작하면 **새로운 함수 실행 컨텍스트가 생성**된다.
2.  전역 코드의 경우와 마찬가지로 **1. 스코프 체인의 생성과 초기화**, **2. Variable Instantiation 실행**, **3. this value 결정**이 순차적으로 실행된다.

단, 전역 코드와 다른 점은 이번 실행되는 **코드는 함수 코드라는 것**이다. 따라서 전역 코드의 룰이 아닌 **함수 코드의 룰이 적용**된다.

![함수 foo의 실행 컨텍스트 생성](http://poiemaweb.com/img/ec_11.png)

---

#### 3- 2.2.1 스코프 체인의 생성과 초기화

함수 코드의 **스코프 체인의 생성과 초기화**는 우선 Activation Object에 대한 참조를 스코프 체인의 선두에 설정하는 것으로 시작된다. (스코프체인(SC)의 첫배열요소(0)은 AO-1를 가르킴)



![스코프 체인의 생성과 초기화](http://poiemaweb.com/img/ec_12.png)

1. 우선 빈객체를 생성한다.( AO-1)
2. 그 후, 함수를 호출한 주체의 Scope Chain이 참조하고 있는 객체가 스코프 체인에 push된다. 

> 함수를 호출한 주체의 Scope Chain = **[0]**
>
> [0]이 참조하고 있는객체 = **foo** 
>
> 스코프체인에 push됨 = **foo() EC의 스코프체인(SC)에 추가됨**

3. 이제 함수 foo를 실행한 직후 실행컨텍스트는 [0]이 참조하고있는 객체를 **먼저 탐색하고 원하는 것이 없다면 순차적으로 다음 스코프체인[1]을 참조**하게된다.



![스코프 체인의 생성과 초기화](http://poiemaweb.com/img/ec_13.png)







---

#### 3- 2.2.2 Variable Instantiation 실행



**스코프 체인의 생성과 초기화**에서 생성된 Activation Object를 Variable Object로서 Variable Instantiation가 실행된다.

Activation Object(활성객체) = bar

Variable Object(변수객체)  = VO

Variable Instantiation이 실행됨 =  var가 변수객체로서 객체화됨



이것을 제외하면 전역 코드의 경우와 같은 처리가 실행된다. 즉, 함수 객체(VO-1)를 Variable Object에 바인딩한다.  (프로퍼티는 bar, 값은 새로 생성된 Function Object. bar function object의 [[Scope]] 프로퍼티 값은 AO-1과 Global Object를 참조하는 리스트 SC）

```js
var x = 'xxx';

function foo () {
  var y = 'yyy'; // (2)

  function bar () { // (1) 함수객체가 변수보다 먼저 할당된다.
  }
  bar();
}

foo();
```



![Variable Instantiation 실행](http://poiemaweb.com/img/ec_14.png)





변수 y를 Variable Object(AO-1)에 설정한다 이때 프로퍼티는 y, 값은 undefined이다.

```js
function foo () {
  var y = 'yyy'; // (2)  아직 코드를 실행한것이아닌 준비작업이기 때문이다.
```



![Variable Instantiation 실행](http://poiemaweb.com/img/ec_15.png)

---

#### 3- 2.2.3 this value 결정

생성자함수로 만들어진 인스턴스나 메서드를 제외하면 모든 this는 전역객체를 참조한다.



![this value 결정](http://poiemaweb.com/img/ec_16.png)



---

## 3- 3. foo 함수 코드의 실행

```js
var x = 'xxx';

function foo () {
  var y = 'yyy';

  function bar () {
    var z = 'zzz';
    console.log(x + y + z);
  }
  bar();
}

foo();
```



이제 함수 foo 코드블럭 내 구문이 실행된다 .

예제를 보면 변수 y에 문자열 ‘yyy’의 할당과 함수 bar가 실행된다.

---

### 3- 3.1 변수 값의 할당



```js
var x = 'xxx';

function foo () { // 전역이아닌것들은 현재 스택에 안쌓임
    var y = 'yyy';

    function bar () {
        var z = 'zzz';
        console.log(x + y + z);
    }
    bar();
}

foo();
```

지역 변수 y에 문자열 ‘yyy’를 할당할 때, 현재 실행 컨텍스트의 스코프 체인이 참조하고 있는 Variable Object를 선두(0)부터 검색하여 변수명에 해당하는 프로퍼티가 발견되면 값 ‘yyy’를 할당한다.

1. yyy 를 변수`y`에다가 할당하려면 y를찾아야한다.
2. 현재 실행컨텍스트(foo EC)의 스코프체인이 참조하고 있는 Variable Object = A0-1
3.  선두부터 검색한다. [0]은 A0-1, [1]은 GO
4. 변수명에 해당하는 프로퍼티 [0]의 AO-1안에 y 프로퍼티 발견 후  'yyy'할당  

![변수 값의 할당](http://poiemaweb.com/img/ec_17.png)





---

### 3- 3.2 함수 bar의 실행

![함수 bar의 실행](http://poiemaweb.com/img/ec_18.png)



함수bar가 실행되기 시작하면 새로운 함수 실행컨텍스트(bar EC)가 생성된다.

bar() EC로 컨트롤이 이동하면 이전 함수 foo와 마찬가지로 

1. 스코프체인의 생성초기화 
2. Variable Instantiation 실행
3. this value 결정

이 순차적로 실행된다.

![함수 bar의 실행 컨텍스트](http://poiemaweb.com/img/ec_19.png)

이 단계에서 `console.log(x + y + z);` 

- x : AO-2에서 x 검색 실패 → AO-1에서 x 검색 실패 → GO에서 x 검색 성공 (값은 ‘xxx’)
- y : AO-2에서 y 검색 실패 → AO-1에서 y 검색 성공 (값은 ‘yyy’)
- z : AO-2에서 z 검색 성공 (값은 ‘zzz')







---



# 단어개념정리



실행컨텍스트 : 실행 가능한 코드르 형상화하고 구분하는 추상적개념. 실행가능한 코드를 위한 환경 

- 실행컨텍스트의 프로퍼티 : VO, SC, this
  - VO: arguments, 전역함수, 변수
  - SC : 스코프체인 GO, AO 
    - GO : 전역객체
    - AO : 활성객체