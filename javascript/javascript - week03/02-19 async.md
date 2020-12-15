# 동기식 처리 모델 vs 비동기식 처리 모델

**브라우저는 단일 쓰레드(single-thread)에서 이벤트 드리븐(event-driven) 방식으로 동작한다.**

자바스크립트는 본래 단일스레드 기반의 언어이다. 하지만 실제 웹 애플리케이션들을 보면 여러 task가 동시에 처리되는 것처럼 보인다. 이는 자바스크립트가 이벤트 루프를 이용해서 비동기 방식으로 동시성을 지원하기 때문이다.

![img](http://poiemaweb.com/img/block_nonblock.png)

## 동기식  

- 하나의 작업이 끝날때까지 다른작업은 기다리고 있어야한다.	(브라우저가 멈춘거 처럼 보인다.)
- ex) `alert()` 함수
- ![synchronous](http://poiemaweb.com/img/synchronous.png)



```js
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  console.log('func2');
  func3();
}

function func3() {
  console.log('func3');
}

func1();
// func1
// func2
// func3
```





## 비동기식

- 하나의 작업이 끝날때까지 기다릴 필요가 없어진다.





![asynchronous](http://poiemaweb.com/img/asynchronous.png)

자바스크립트의 대부분의 DOM 이벤트와 Timer 함수(setTimeout, setInterval), Ajax 요청은 비동기적으로 동작한다.

아래는 비동기식으로 동작하는 코드이다. 순차적으로 실행되지 않는다.

```js
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  setTimeout(function() { 
    console.log('func2');
  }, 0); // 0초후에 실행하여라.

  func3();
}

function func3() {
  console.log('func3');
}

func1();
// func1
// func3
// func2  
```

![setTimeout](http://poiemaweb.com/img/settimeout.png)

setTimeout 메소드가 비동기 함수이기 때문.

![event-loop](http://poiemaweb.com/img/event-loop.gif)

>  이벤트루프



모든 함수는 Call Stack에 있어야 실행됨.



1. Web API에게 `setTimeout`메서드를 호출해달라고 부탁함.
2. 지정해놓은 시간이 지나면 Event Queue로 이동함
3. Call Stack에 있는 모든 것이 실행되면 
4. Event Queue에 있는게 Call Stack으로 이동하여 실헹된다.



> - 멀티쓰레드
>   - 하나의 프로세스가 여러개의 일을 하는것 멀티쓰레드, 대신 리소스를 많이 씀 
> - 싱글쓰레드
>   - 하나의 프로세스가 하나의 일만 하는것, 뚝뚝 끊기는 현상이 일어난다.





단일 쓰레드방식인 브라우저를 멀티쓰레드를 사용하는것 처럼 보여주려고 하다보니 일급객체가 생겻다.