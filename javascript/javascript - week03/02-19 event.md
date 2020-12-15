# intro

이벤트(event)는 어떤 사건을 의미한다. 브라우저에서의 이벤트란 예를 들어 사용자가 버튼을 클릭했을 때, 웹페이지가 로드되었을 때와 같은 것인데 이것은 DOM 요소와 관련이 있다.

이벤트는 브라우저가 감지를 한다. 개발자는 특정 이벤트가 발생했을때 어떤 처리 or 반응 할것인지 처리한다.

```js
var elem = document.getElementById('myButton');
elem.addEventListener('click', function() {
  console.log('Clicked!');
});

```

이벤트가 발생하면 그에 맞는 반응을 하여야 한다. 이를 위해 이벤트는 일반적으로 함수에 연결되며 그 함수는 이벤트가 발생하기 전에는 실행되지 않다가 이벤트가 발생되면 실행된다. 이 함수를 **이벤트 핸들러**라 하며 이벤트에 대응하는 처리를 기술한다.

---



## 1.이벤트 루프와 동시성

브라우저는 **단일쓰레드**에서 **이벤트 드리븐 방식**으로 동작한다.

**단일 쓰레드**는 쓰레드가 하나뿐이라는 의미이며 이말은 곧 하나의 작업(task)만을 처리할 수 있다는 것을 의미한다.  하지만  웹브라우저는 애니메이션 효과를 보여주면서 마우스 입력을 받아서 처리하고, Node.js기반의 웹서버에서는 동시에 여러개의 HTTP 요청을 처리하기도 한다. 이렇게 웹 애플리케이션에서 task가 동시에 처리되는것 처럼 느껴지게 하는것이 **이벤트 루프**이다.

![event-loop](http://poiemaweb.com/img/event-loop.png)



### Call Stack

```js
function multiply(a, b) { // 3
    return a * b;
}
function square(n) { // 2
    return multiply(n, n);
}
function printSquare(n) { // 1
    var squared = sqaure(n);
    console.log(squared);
}
printSquare(4); // 16
```



Call Stack이 쌓이는 구조

| multiply(n, n)     |
| ------------------ |
| square(n)          |
| printSquares(4)    |
| main() (이건 예시) |

### Heap

힙은 동적으로 생성된 객체 인스턴스가 할당되는 영역이다.

이와 같이 자바스크립트 엔진은 단순히 작업이 요청되면 요청된 작업을 Call Stack을 사용하여 순차적으로 실행할 뿐이다. 앞에서 언급한 동시성(Concurrency)을 지원하기 위해 필요한 *비동기 요청(이벤트를 포함) 처리는 자바스크립트 엔진을 구동하는 환경, 즉 브라우저(또는 Node.js)가 담당한다.*



### Event Queue(Task Queue)

비동기 처리 함수의 콜백 함수, 비동기식 이벤트 핸들러, Timer 함수(setTimeout(), setInterval())가 보관되는 영역이다. **이벤트 루프(Event Loop)에 의해 특정 시점(Call Stack이 비어졌을 때)에 순차적으로 Call Stack으로 이동되어 실행된다.**



### Event Loop

Call Stack내에서 1) 현재 실행중인 task가 있는지, 그리고 2) Event Queue에 task가 있는지 반복하여 확인한다.

---



## 2. 이벤트의 종류

대표적인 이벤트들



---



### 2-1. UI

| Event    | Description                                                  |
| -------- | ------------------------------------------------------------ |
| **load** | 웹페이지의 로드가 완료되었을 때                              |
| unload   | 웹페이지가 언로드될 때(주로 새로운 페이지를 요청한 경우)     |
| error    | 브라우저가 자바스크립트 오류를 만났거나 요청한 자원이 존재하지 않는 경우 |
| resize   | 브라우저 창의 크기를 조절했을 때                             |
| scroll   | 사용자가 페이지를 위아래로 스크롤할 때                       |
| select   | 텍스트를 선택했을 때                                         |



---

### 2-2. Keyboard

| Event     | Description            |
| --------- | ---------------------- |
| keydown   | 키를 누르고 있을 때    |
| **keyup** | 누르고 있던 키를 뗄 때 |
| keypress  | 키를 누르고 뗏을 때    |



---



### 2-3. Mouse

| Event     | Description                                                  |
| --------- | ------------------------------------------------------------ |
| **click** | 마우스 버튼을 클릭했을 때                                    |
| dbclick   | 마우스 버튼을 더블 클릭했을 때                               |
| mousedown | 마우스 버튼을 누르고 있을 때                                 |
| mouseup   | 누르고 있던 마우스 버튼을 뗄 때                              |
| mousemove | 마우스를 움직일 때 (터치스크린에서 동작하지 않는다)          |
| mouseover | 마우스를 요소 위로 움직였를 때 (터치스크린에서 동작하지 않는다) |
| mouseout  | 마우스를 요소 밖으로 움직였를 때 (터치스크린에서 동작하지 않는다) |



---

### 2-4. Focus

| Event              | Description               |
| ------------------ | ------------------------- |
| **focus**/focusin  | 요소가 포커스를 얻었을 때 |
| **blur**/foucusout | 요소가 포커스를 잃었을 때 |



---



### 2-5. Form

| Event      | Description                                                 |
| ---------- | ----------------------------------------------------------- |
| **input**  | input 또는 textarea 요소의 값이 변경되었을 때               |
|            | contenteditable 어트리뷰트를 가진 요소의 값이 변경되었을 때 |
| **change** | select box, checkbox, radio button의 상태가 변경되었을 때   |
| submit     | form을 submit할 때 (버튼 또는 키)                           |
| reset      | reset 버튼을 클릭할 때 (최근에는 사용 안함)                 |



---



### 2-6. Clipboard

| Event | Description            |
| ----- | ---------------------- |
| cut   | 콘텐츠를 잘라내기할 때 |
| copy  | 콘텐츠를 복사할 때     |
| paste | 콘텐츠를 붙여넣기할 때 |

---



## 3. Event Binding

어떠한 요소에서 이벤트를 발생시켯을때 바인딩한다.

"어떤"버튼이 클릭되었을때 특정 함수를 실행시키는 것



---

## 3-1.  HTML Event Handler

가장 옛날방식

```js
<!DOCTYPE html>
<html>
<body>
  <button onclick="myFunction()">Click me</button>
  <script>
    function myFunction() {
      alert('Button clicked!');
    }
  </script>
</body>
</html>
```

관심사가 다른 두 언어를 같이 쓰면 유지보수가 힘들어진다. 



---



## 3-2. 전통적(Traditional) DOM Event Handler

**하나의 함수**만을 바인딩할 수 있으며 **함수에 인수를 전달할 수 없는 단점**이 있다.

```js
<!DOCTYPE html>
<html>
<body>
  <button id="btn">Click me</button>
  <script>
    var btn = document.getElementById('btn');

    // 전통적 DOM Event Handler 방식은 이벤트 핸들러에 하나의 함수만을 바인딩할 수 있다
    // 첫번째 바인딩된 이벤트 핸들러 => 실행되지 않는다.
    btn.onclick = function () {
      alert('Button clicked 1');
    };

    // 두번째 바인딩된 이벤트 핸들러 => 첫번째 핸들러를 덮어씌었기 때문에~
    btn.onclick = function () {
      alert('Button clicked 2');
    };

    // DOM Level 2 Event Listener
    // 첫번째 바인딩된 이벤트 핸들러
    btn.addEventListener('click', function () {
      alert('Button clicked 1');
    });

    // 두번째 바인딩된 이벤트 핸들러
    btn.addEventListener('click', function () {
      alert('Button clicked 2');
    });
  </script>
</body>
</html>
```

---



## 3-3. DOM Level 2 Event Listener

`addEventListener()` 함수를 이용하여 대상 요소에 이벤트를 바인딩하고 해당 이벤트가 발생했을 때 실행될 콜백 함수를 지정한다.

![Event Listener](http://poiemaweb.com/img/event_listener.png)



- 하나의 이벤트에 대해 **하나 이상의 핸들러를 추가**할 수 있다.
- **캡처링**과 **버블링**를 지원한다.
- HTML 요소뿐만아니라 모든 DOM 요소(ex) SVG)에 대해 동작한다.

[addEventListener()](https://developer.mozilla.org/ko/docs/Web/API/EventTarget/addEventListener) 함수는 IE9 이상에서 동작한다. IE 8 이하에서는 [attachEvent()](ttps://developer.mozilla.org/ko/docs/Web/API/EventTarget/attachEvent) 함수를 사용한다.

```js
if (elem.addEventListener) {    // IE 9 ~
  elem.addEventListener('click', func);
} else if (elem.attachEvent) {  // ~ IE 8
  elem.attachEvent('onclick', func);
}
```



input 요소를 blur 이벤트에 바인딩하였다. 사용자 이름이 **최소 2자 이상**이야한다는 규칙을 세우고 이에 부합하는지 확인하는 처리를 한다.

```js
<!DOCTYPE html>
<html>
<body>
  <label for="username">User name </label>
  <input type="text" id="username">
  <em id="message"></em>
  <script>
    var elem = document.getElementById('username');
    var msg  = document.getElementById('message');

    elem.addEventListener('blur', function () { // blur 가 발생하면 익명함수를 실행
      if (elem.value.length < 2) { // 2대신 상수화
        msg.innerHTML = '이름은 2자 이상 입력해 주세요';
      } else {
        msg.innerHTML = '';
      }
    });
  </script>
</body>
</html>
```



2자 이상이라는 규칙이 바뀌면 이 규칙을 확인하는 모든 코드를 수정하는것은 비효율적이다. 2자 이상이라는 **규칙을 상수화**하고 함수의 인수로 수정하면 규칙이 변경되더라도 **함수는 수정하지않아도 된다.**

 ```js
var MIN_USER_NAME_LENGTH = 2; // 이름 최소 길이

var elem = document.getElementById('username');
var msg  = document.getElementById('message');

function checkUserNameLength(n) {
  if(elem.value.length < n) {
    msg.innerHTML = '이름은 ' + n + '자 이상이어야 합니다';
  } else {
    msg.innerHTML = '';
  }
}

elem.addEventListener('blur', function() {
  checkUserNameLength(MIN_USER_NAME_LENGTH);
});
 ```





---



## 4. 핸들러 함수 내부의 this



### 4-1. HTML Event Handler

이벤트 핸들러 내부의 **this는 window**를 가리킨다.

```js
<!DOCTYPE html>
<html>
<body>
  <button onclick="foo()">Button</button>
  <script>
    function foo () {
      console.log(this); // window
      console.log(event.currentTarget); // <button onclick="foo()">Button</button>
    }
  </script>
</body>
</html>
```



### 4-2. 전통적(Traditional) DOM Event Handler

**this는 이벤트에 바인딩된 요소**를 가리킨다. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.

```js
<!DOCTYPE html>
<html>
<body>
  <button id="btn">Button</button>
  <script>
    var btn = document.getElementById('btn');
    btn.onclick = function() {
      console.log(this); // <button id="btn">Button</button>
      console.log(event.currentTarget); // <button id="btn">Button</button>
      console.log(this === event.currentTarget); // true
    };
  </script>
</body>
</html>
```



---



### 4-3. DOM Level 2 Event Listener

이벤트 핸들러 내부의 **this는** 콜백함수의 this는 이벤트가 바인딩된 객체이다.

```js
<!DOCTYPE html>
<html>
<body>
  <button id="btn">Button</button>
  <script>
    var elem = document.getElementById('btn');
    elem.addEventListener('click', function (event) {
      console.log(this); // <button id="btn">Button</button>
      console.log(event.currentTarget); // <button id="btn">Button</button>
      console.log(this === event.currentTarget); // true
    });
  </script>
</body>
</html>
```



---

## 5. Event Flow(이벤트 흐름)

![event flow](http://poiemaweb.com/img/eventflow.svg)



계층적 구조에 포함되어 있는 HTML 요소에 이벤트가 발생할 경우 연쇄적 반응이 일어난다. 이벤트 전파되는데 전파방향에 따라 **버블링과 캡쳐링으로** 구분이된다.

**자식 요소**에서 발생한 이벤트가 **부모 요소**로 전파되는 것을 **버블링**이라 하고, **자식 요소**에서 발생한 이벤트가 **부모 요소**부터 시작하여 이벤트를 발생시킨 자식 요소까지 도달하는 것을 **캡처링**이라 한다.

부모요소가 자식요소들의 이벤트를 잡으려면 버블링

자식요소가 부모요소의 이벤트를 잡으려면 캡쳐링



`addEventListener`의 세번째 인자에 false를 주면 캡쳐링을 잡아낸다.



```js
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
  </style>
<body>
  <p>버블링 이벤트 <button>버튼</button></p>
  <script>
    var body = document.querySelector('body');
    var para = document.querySelector('p');
    var button = document.querySelector('button');

    // 버블링
    body.addEventListener('click', function () {
      console.log('Handler for body.'); // 3 
    });

    // 버블링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.'); // 2
    });

    // 버블링
    button.addEventListener('click', function () {
      console.log('Handler for button.'); // 1
    });



    // 캡처링
    body.addEventListener('click', function () {
      console.log('Handler for body.'); // 1
    }, true);

    // 캡처링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.'); // 2 
    }, true);

    // 캡처링
    button.addEventListener('click', function () {
      console.log('Handler for button.'); // 3 
    }, true);


    // 버블링
    body.addEventListener('click', function () {
      console.log('Handler for body.'); // 3 
    });

    // 캡처링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.'); // 1
    }, true);

    // 버블링
    button.addEventListener('click', function () {
      console.log('Handler for button.'); // 2
    });
  </script>
</body>
</html>
```



---



## 6. Event 객체

모든 이벤트핸들러는 **첫번째 매개변수**에 이벤트 객체를 넣어준다.

예제에서 e라는 이름으로 매개변수를 지정하였으나 **다른 매개변수 이름을 사용하여도 상관없다**.

```js
<!DOCTYPE html>
<html>
<body>
  <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
  <em id="message"></em>
  <script>
  function showCoords(e) { // e: event object
    var msg = document.getElementById('message');
    msg.innerHTML =
      'clientX value: ' + e.clientX + '<br>' +
      'clientY value: ' + e.clientY;
  }
  addEventListener('click', showCoords);
  </script>
</body>
</html>
```



---



## 6-1. Event Property

- `Event.target`
  - 이벤트를 실제로 발생시킨 요소를 가리킨다.

```js
<!DOCTYPE html>
<html>
<body>
  <button id="btn1">Hide me 1</button>
  <button id="btn2">Hide me 2</button>
  <script>
  function hide(e) {
    e.target.style.visibility = 'hidden';
    // this.style.visibility = 'hidden'; 동일하기 동작한다.
  }

  document.getElementById('btn1').addEventListener('click', hide);
  document.getElementById('btn2').addEventListener('click', hide);
  </script>
</body>
</html>
```



- `Event.currentTarget`

  - addEventListener 함수를 호출한 요소를 가리킨다.
  - 이벤트 핸들러 함수 내의 this에는 addEventListener를 호출한 요소가 바인딩된다. 
  - this와 동치이다.

  ```js
  <!DOCTYPE html>
  <html>
  <head>
    <style>
      html, body { height: 100%; }
      div { height: 100%; }
    </style>
  </head>
  <body>
    <div>
      <button>배경색 변경</button>
    </div>
    <script>
      function bluify(e) {
        // this: addEventListener를 호출한 요소(div 요소)
        console.log('this: ', this);
        // target: 이벤트를 발생시킨 요소(button 요소 또는 div 요소)
        console.log('e.target:', e.target);
        // currentTarget: addEventListener를 호출한 요소(div 요소)
        console.log('e.currentTarget: ', e.currentTarget);

        // 언제나 true
        console.log(this === e.currentTarget);
        // currentTarget과 target이 같은 객체일 때 true
        console.log(this === e.target);

        // click 이벤트가 발생하면 이벤트를 발생시킨 요소(target)과는 상관없이 this(addEventListener를 호출한 div 요소)의 배경색이 변경된다.
        this.style.backgroundColor = '#A5D9F3';
      }

      // div 요소에 이벤트 핸들러가 바인딩되어 있다.
      // 자식 요소인 button이 발생시킨 이벤트가 버블링되어 div 요소에도 전파된다.
      // 따라서 div 요소에 이벤트 핸들러가 바인딩되어 있으면 자식 요소인 button이 발생시킨 이벤트를 div 요소에서도 핸들링할 수 있다.
      document.querySelector('div').addEventListener('click', bluify);
    </script>
  </body>
  </html>
  ```



- `Event.type`

  - 발생한 이벤트의 종류를 나타내는 문자열을 반환한다.

    ```js
    <!DOCTYPE html>
    <html>
    <body>
      <p>키를 입력하세요</p>
      <em id="message"></em>
      <script>
      function getEventType(e) {
        console.log(e);
        document.getElementById('message').innerHTML = e.type + ' : ' + e.keyCode;
      }

      var body = document.querySelector('body');

      body.addEventListener('keydown', getEventType);
      body.addEventListener('keyup', getEventType);
      </script>
    </body>
    </html>
    ```



- `Event.cancelalbe`

  - 요소의 기본 동작을 취소시킬 수 있는지 여부(true/false)를 나타낸다.

    ```js
    <!DOCTYPE html>
    <html>
    <body>
      <a href="poiemaweb.com">Go to poiemaweb.com</a>
      <script>
      var elem = document.querySelector('a');

      elem.addEventListener('click', function (e) {
        console.log(e.cancelable);  // true

        // 기본 동작을 중단시킨다.
        e.preventDefault();
      });
      </script>
    </body>
    </html>
    ```

  ​

- `Event.eventPhase`

  - 이벤트 흐름(event flow) 상에서 어느 단계(event phase)에 있는지를 반환한다.

  | 반환값 | 의미        |
  | ------ | ----------- |
  | 0      | 이벤트 없음 |
  | 1      | 캡쳐링 단계 |
  | 2      | 타깃        |
  | 3      | 버블링 단계 |

## 6-2. Event Method

- `Event.preventDefault()`

  - 이벤트의 **기본 동작을 취소**한다. 단 Event.cancelable가 true일 경우에 한한다.

  - ```js
    <!DOCTYPE html>
    <html>
    <body>
      <a href="http://www.google.com">go</a>
      <script>
      document.querySelector('a').addEventListener('click', function(e) {
        console.log(e.target, e.target.nodeName);

        // a 요소의 기본 동작을 중단한다. a태그 클릭시 이동하지않는다.
        e.preventDefault();
      });
      </script>
    </body>
    </html>
    ```

    ​

  ​

- `Event.stopPropagation()`

  - 이벤트의 전파(propagation: 버블링, 캡처링)를 중단한다. 부모 요소에 동일한 이벤트에 대한 다른 핸들러가 지정되어 있을 경우 사용된다.


---



## 7. Event Delegation (이벤트 위임)

```html
<ul id="parent-list">
  <li id="post-1">Item 1</li>
  <li id="post-2">Item 2</li>
  <li id="post-3">Item 3</li>
  <li id="post-4">Item 4</li>
  <li id="post-5">Item 5</li>
  <li id="post-6">Item 6</li>
</ul>
```

모든 li 요소가 클릭 이벤트에 반응하는 처리를 구현하고 싶은 경우, li 요소에 이벤트 핸들러를 바인딩하면 총 6개의 이벤트 핸들러를 바인딩하여야 한다. 그리고 **동적으로 li 요소가 추가되는 경우, 아직 추가되지 않은 요소는 DOM에 존재하지 않으므로 이벤트 핸들러를 바인딩할 수 없다.** 이러한 경우 **이벤트 위임**을 사용한다.

부모 요소(ul#parent-list)에 이벤트 핸들러를 바인딩하는 것이다. 이는 이벤트를 발생시킨 요소의 부모 요소에도 영향(버블링)을 미치기 때문에 가능한 것이다.

```js
 var msg = document.getElementById('msg');

    document.getElementById('parent-list').addEventListener('click', function (e) {
      console.log('[target]: ' + e.target);
      console.log('[target.nodeName]: ' + e.target.nodeName);

      // li요소 에서 발생한 이벤트인지 확인한다. 
      if (e.target && e.target.nodeName == 'LI') {
        msg.innerHTML = 'li#' + e.target.id + ' was clicked!';
      }
    });
```



---



## 8. 기본 동작의 변경

이벤트 객체는 **요소의 기본 동작**과 **요소의 부모 요소들**이 *이벤트에 대응하는 방법을 변경하기 위한 메소드를 가지고 있다.*

- `Event.preventDefault()` 
  - **a 태그** 처럼 클릭 이벤트 외에 별도의 브라우저 행동을 중지시킴
- `Event.stopPropagation()`
  - stopPropagation 은 부모태그로의 이벤트 전파를 중지시킴


---



## 참고

- [`Event.preventDefault()`, `Event.stopPropagation()` 둘의 차이점](http://ismydream.tistory.com/98)
- [자바스크립트와 이벤트 루프](https://github.com/nhnent/fe.javascript/wiki/June-13-June-17,-2016)
- [Philip Roberts: Help, I’m stuck in an event-loop.](https://vimeo.com/96425312)

