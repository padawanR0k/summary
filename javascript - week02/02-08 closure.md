# 1. 클로저(closure)의 개념

내부함수와 외부함수가 있는데, **내부함수**에서 **외부함수의 변수**를 참조할때 내부함수가 외부함수보다 life-cycle이 더 긴 경우에

참조한 변수를 여전히 참조할 수 있는 현상



내부함수가 참조하는 외부함수의 지역변수 = 자유변수 = `x`

```js
function outerFunc() {
  var x = 10;
  var innerFunc = function () { console.log(x); };
  innerFunc();
}

outerFunc(); // 10
```

innerFunc()가 outerFunc()의 `x` 를 참조하고있다





```js
function outerFunc() {
  var x = 10;
  var innerFunc = function () { console.log(x); };
  return innerFunc; // return 추가됨.
}
// 함수 outerFunc를 호출하면 내부 함수 innerFunc가 반환된다. 그리고 함수 outerFunc의 실행 컨텍스트는 소멸한다
var inner = outerFunc();
inner(); // 10		inner는 function() {console.log(x)}와 같다.
```



![closure](http://poiemaweb.com/img/closure.png)

outerFunc실행컨텍스트의  AO-1이 소멸되었지만 

innerFunc실행컨텍스트의 스코프체인이 AO-1의 변수x를 참조하기 때문에 

outerFunc의 변수 x가 살아있는(참조가능한) 상태가 되었다.





# 2. 클로저의 활용

무분별한 클로저 사용은 메모리낭비이다. 적절한 상황에만 써야한다.



## 2.1 전역 변수의 사용 억제

클로저의 필요성을 이해하기 위해서 버튼이 클릭될 때마다 클릭한 횟수가 누적되어 화면에 표시되는 코드이다.

```js
<!DOCTYPE html>
<html>
<body>
  <p>전역 변수를 사용한 Counting</p>

  <button type="button" onclick="myFunction()">Count!</button>

  <p id="demo">0</p>

  <script>
    var counter = 0;

    function add() {
      return ++counter;
    }

    function myFunction() {
      document.getElementById('demo').innerHTML = add();
    }
  </script>
</body>
</html>
```

전역변수의 선언은 꼭 필요하지않는 이상 지양해야한다.  

클로져를 이용해서 문제를 해결해보자

```js
<!DOCTYPE html>
<html>
  <body>
  <p>클로저를 사용한 Counting</p>

  <button type="button" onclick="myFunction()">Count!</button>

  <p id="demo">0</p>

  <script>
    var add = (function () {
      var counter = 0;
      return function () {
        return ++counter;
      };
    }());

    function myFunction() {
      document.getElementById('demo').innerHTML = add();
    }
  </script>
  </body>
</html>
```

변수 add에는 즉시실행함수가 호출되어 그 결과 무명함수 `function () {return ++counter;}`를 반환한다. 따라서 `add()`를 실행하면 **변수 add에 담긴 함수가 호출**된다.





## 2.2 setTimeout의 콜백 함수

setTimeout 함수는 **첫번째 인자에 콜백 함수를 전달**하고, 두번째 인자에 시간 간격(ms: 1000분의 1초)을 지정한다. 즉 지정된 시간 간격으로 **콜백 함수를 반복 호출**한다.

```js
<!DOCTYPE html>
<html>
<body>
  <p>새로고침으로 다시 실행해 보세요</p>
  <script>
    var fade = function (node) {
      // 자유변수
      var level = 1; // ②
      var step = function () {
        var hex = level.toString(16); // ④

        // hex: '1' ~ 'f'
        node.style.backgroundColor = '#ff' + hex; // ⑤

        if (level < 15) { // ⑥
          level += 1;
          setTimeout(step, 100); // ⑦
        }
      };
      // setTimeout 호출 이후 fade 함수는 종료한다. 하지만 100ms 후 함수 step이 호출된다.
      // 즉 외부 함수 fade보다 내부 함수 step이 더 오래 유지된다.
      setTimeout(step, 100); // ③
    };

    fade(document.body); // ①
  </script>
</body>
</html>
```



이때 fade 함수는 이미 반환되었지만 외부함수 fade 내의 변수는 이를 필요로 하는 내부함수가 하나 이상 존재하는 경우 계속 유지된다. 이때 내부함수가 외부함수에 있는 변수의 복사본이 아니라 실제 변수에 접근한다는 것에 주의하여야 한다.



## 2.3 자주 발생하는 실수

아래예제는 클로저를 사용할때 발생할 수 있는 실수이다.

```js
var arr = [];

for (var i = 0; i < 5; i++) {
  arr[i] = function () 
    return i;
  };
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]()); // 01234가 반환될줄알았는데 55555가 반환된다.
}
```





바르게 동작하는 코드

```js
var arr = [];

for (var i = 0; i < 5; i++){
  arr[i] = (function (id) { // ② 
      					// 여기서 id는 자유변수
    return function () {
      return id; // ③
    };
  }(i)); // ①
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}
```

1.  배열 arr에는 즉시실행함수에 의해 함수가 반환된다.
2. 이때 즉시실행함수는 i를 인자로 전달받고 매개변수 id에 할당한 후 내부 함수를 반환하고 life-cycle이 종료된다. 매개변수 id는 자유변수가 된다.
3. 배열 arr에 할당된 함수는 id를 반환한다. 이때 id는 상위 스코프의 자유변수이므로 그 값이 유지된다.

위 예제는 자바스크립트의 함수 레벨 스코프 특성으로 인해 for 루프의 초기문에서 사용된 변수의 스코프가 전역이 되기 때문에 발생하는 현상이다. ES6의 let 키워드를 사용하면 이와 같은 문제는 말끔히 해결된다.