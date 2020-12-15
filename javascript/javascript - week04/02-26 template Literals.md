ES6는 **템플릿 리터럴(**template Literals)이라고 불리는 새로운 **문자열 표기법**을 도입하였다. 템플릿 리터럴은 일반 문자열과 비슷해 보이지만, *‘ 또는 “ 같은 통상적인 따옴표 문자 대신 **백틱(backtick) 문자 `` ` **를 사용한다.*

```js
const template = `템플릿 리터럴은 '작은따옴표(single quotes)'과 "큰따옴표(double quotes)"를 혼용할 수 있다.`;

console.log(template);
```



ES6의 문자열에서는 white-space가 있는 그대로 적용된다. 이를 **String Interpolation(문자열 삽입)**이라 한다.

```js
const template = `<ul class="nav-items">
  <li><a href="#home">Home</a></li>
  <li><a href="#news">News</a></li>
  <li><a href="#contact">Contact</a></li>
  <li><a href="#about">About</a></li>
</ul>`;

console.log(template);
```



템플릿 리터럴은 문자열변수를 삽입할 때 `${}` 을 사용하여 삽입할수 있다.

이를 **템플릿 대입문(template substitution)**이라고 한다.

```js
const first = 'Ung-mo';
const last = 'Lee';

// 기존의 문자열 연결
console.log('My name is ' + first + ' ' + last + '.');
// ES6 String Interpolation
console.log(`My name is ${first} ${last}.`); // My name is Ung-mo Lee.
```

템플릿 대입문에는 문자열뿐만 아니라 **JavaScript 표현식**을 사용할 수 있다. 여기서 표현식이란 하나의 값으로 수렴되는 구문을 뜻한다.



```js
// 템플릿 대입문에는 문자열뿐만 아니라 표현식도 사용할 수 있다.
console.log(`1 + 1 = ${1 + 1}`); // 1 + 1 = 2

const name = 'ungmo';

console.log(`Hello ${name.toUpperCase()}`); // Hello UNGMO
```



