# 1. 정규표현식(Regular Expression)

문자열에서 특정 내용을 찾거나 대체 또는 발췌하는데 사용한다.

![regular expression](http://poiemaweb.com/img/regular_expression.png)

```js
var tel = '0101234567팔';

var myRegExp = /^[0-9]+$/;

console.log(myRegExp.test(tel)); // false

var targetStr = 'This is a pen.';
var regexr = /is/ig;

// RegExp 객체의 메소드
console.log(regexr.exec(targetStr)); // [ 'is', index: 2, input: 'This is a pen.' ]
console.log(regexr.test(targetStr)); // true

// String 객체의 메소드
console.log(targetStr.match(regexr)); // [ 'is', 'is' ]
console.log(targetStr.replace(regexr, 'IS')); // ThIS IS a pen.
// replace( , ) 사용시 첫번째 인자에 문자열을 주면 제일 처음에 일치하는 1개만 반환하지만 정규표현식은 모두 찾아낸다
console.log(targetStr.search(regexr)); // 2
console.log(targetStr.split(regexr));  // [ 'Th', ' ', ' a pen.' ]
```





## 1- 2. 플래그

| Flag | Meaning     | Description                              |
| ---- | ----------- | ---------------------------------------- |
| i    | Ignore Case | 대소문자를 구별하지 않고 검색한다.                      |
| g    | Global      | 문자열 내의 모든 패턴을 검색한다.                      |
| m    | Multi Line  | 문자열의 행이 바뀌더라도 검색을 계속한다. 문서를 통째로 검사할때 사용함 |

플래그는 옵션이므로 선택적으로 사용한다. 플래그를 사용하지 않은 경우 문자열 내 검색 매칭 대상이 1개 이상이더라도 첫번째 매칭한 대상만을 검색하고 종료한다.

```js
var targetStr = 'Is this all there is?';
var regexr = /is/;

console.log(targetStr.match(regexr)); // [ 'is', index: 5, input: 'Is this all there is?' ]

regexr = /is/ig;

console.log(targetStr.match(regexr)); // [ 'Is', 'is', 'is' ]
```





## 1- 3. 패턴

패턴에는 찾고자 하는 대상을 문자열로 지정한다. 또한 패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있다. 몇가지 패턴 표현 방법을 소개한다.

>  `.`

임의의 문자 한개를 의미한다

```js
var targetStr = 'AA BB Aa Bb';
// 임의의 문자 3개
var regexr = /.../;
console.log(targetStr.match(regexr)); // [ 'AA ', index: 0, input: 'AA BB Aa Bb' ]
```

> `g`

이때 추출을 반복하지 않는다. 반복하기 위해서는 `g`를 써야한다.

```js
var targetStr = 'AA BB Aa Bb';
// 임의의 문자 3개를 반복하여 검색
var regexr = /.../g;
console.log(targetStr.match(regexr)); // [ 'AA ', 'BB ', 'Aa ' ]
```

> `i`

대소문자를 구별하지 않게 하려면 사용한다. 

```js
var targetStr = 'AA BB Aa Bb';
// 'A'를 대소문자 구분없이 반복 검색
var regexr = /A/ig;
console.log(targetStr.match(regexr)); // [ 'A', 'A', 'A', 'a' ]
```

앞선 패턴을 최소 한번 반복하려면 앞선 패턴 뒤에 `+`를 붙인다. 아래의 경우 앞선 패턴는 A이므로 A+는 AA 또는 A를 의미한다.

```js
var targetStr = 'AA AAA BB Aa Bb';
// 'A'가 한번이상 반복되는 문자열을 반복 검색
var regexr = /A+/g;
console.log(targetStr.match(regexr)); // [ 'AA', 'AAA', 'A' ]
```

> `|`

or의 의미와 같다.



> `+`

분해되지 않은 단어 레벨로 추출하기 위해서 사용함

```js
var targetStr = 'AA AAA BB Aa Bb';
// 'A' 또는 'B'가 한번이상 반복되는 문자열을 반복 검색
var regexr = /A+|B+/g;
console.log(targetStr.match(regexr)); // [ 'AA', 'AAA', 'BB', 'A', 'B' ]
```



> `[ - ]`

 범위를 지정한다.

```js
var targetStr = 'AA BB ZZ Aa Bb';
// 'A' ~ 'Z'가 한번이상 반복되는 문자열을 반복 검색
var regexr = /[A-Z]+/g;
console.log(targetStr.match(regexr)); // [ 'AA', 'BB', 'ZZ', 'A', 'B' ]

// 아래는 숫자를 추출한다.
var targetStr = 'AA BB Aa Bb 24,000';
// '0' ~ '9'가 한번이상 반복되는 문자열을 반복 검색
var regexr = /[0-9]+/g;
console.log(targetStr.match(regexr)); // [ '24', '000' ]
```



> `\d` 

숫자를 의미한다. 

```js
var targetStr = 'AA BB Aa Bb 24,000';
// '0' ~ '9' 또는 ','가 한번이상 반복되는 문자열을 반복 검색
var regexr = /[\d,]+/g;
console.log(targetStr.match(regexr)); // [ '24,000' ]
```



> `\D`

`\d` 와 반대로 동작한다.

```js
// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한번이상 반복되는 문자열을 반복 검색
var regexr = /[\D,]+/g;
console.log(targetStr.match(regexr)); // [ 'AA BB Aa Bb ', ',' ]
```



> `\w`

알파뱃과 숫자를 의미한다.

```js
var targetStr = 'AA BB Aa Bb 24,000';
// 알파벳과 숫자 또는 ','가 한번이상 반복되는 문자열을 반복 검색
var regexr = /[\w,]+/g;
console.log(targetStr.match(regexr)); // [ 'AA', 'BB', 'Aa', 'Bb', '24,000' ]

```



> `\W`

`\w` 와 반대로 동작한다.

```js
// 알파벳과 숫자가 아닌 문자 또는 ','가 한번이상 반복되는 문자열을 반복 검색
var regexr = /[\W,]+/g;
console.log(targetStr.match(regexr)); // [ ' ', ' ', ' ', ' ', ',' ]
```





## 1- 4. 자주 사용하는 정규표현식



# 2. Javascript Regular Expression
- 2.1 RegExp Constructor
- 2.2 RegExp Method
- 2.2.1 RegExp.prototype.exec()
- 2.2.2 RegExp.prototype.test()