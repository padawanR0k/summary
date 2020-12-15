# 1. Promise란?

 Promise는 전통적인 콜백 패턴이 가진 단점을 일부 보완하며 비동기 처리 시점을 명확하게 표현한다.

# 2. 콜백 패턴의 단점



## 2.1 콜백 헬(Callback Hell)

JavaScript에서 빈번히 사용되는 비동기 처리 모델은 요청을 병렬로 처리하여 다른 요청이 중단되는 않는 장점이 있지만 단점도 가지고 있는데 그것은 여러개의 콜백함수가 순서를 보장하기 위해 nesting되어 복잡도가 높아지는 **Callback Hell**이다.

![callback hell](http://poiemaweb.com/img/callback-hell.png)

유저의 닉네임으로 다른 게시글을 확일할때 ajax안에 ajax를 호출하게되서 헬이 형성된다.

## 2.2 에러 처리의 한계

이벤트 큐에 있던 콜백함수가 Call Stack이 비어있을 때 까지 기다리다가 이벤트루프에 의해 Call Stack으로 욺겨지고 실행된다.



 ```js
try {
  setTimeout(() => { throw 'Error!'; }, 1000);
} catch (e) {
  console.log('에러를 캐치하지 못한다..');
  console.log(e);
}
 ```



# 3. Promise의 상태(State)

Promise는 비동기 처리가 성공하였는지 또는 실패하였는지 등의 상태(state) 정보를 갖는다.

| 상태          | 의미                                       | 구현                                               |
| ------------- | ------------------------------------------ | -------------------------------------------------- |
| pending       | 비동기 처리가 아직 수행되지 않은 상태      | resolve 또는 reject 함수가 아직 호출되지 않은 상태 |
| **fulfilled** | 비동기 처리가 수행된 상태 (성공)           | resolve 함수가 호출된 상태                         |
| **rejected**  | 비동기 처리가 수행된 상태 (실패)           | reject 함수가 호출된 상태                          |
| settled       | 비동기 처리가 수행된 상태 (성공 또는 실패) | resolve 또는 reject 함수가 호출된 상태             |



# 4. Promise의 생성

```js
var promise = new Promise((resolve, reject) => { 
  // new Promise인자로 콜백함수로받앗다.
  // 그 내부의 arrow function의 인자로 resolve, reject를 받았다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('resolved!');
  }
  else { /* 비동기 작업 수행 실패 */
    reject(Error('rejected!'));
  }
});
```





# 5. Promise 후속 처리 함수 then, catch

```js
// 비동기 함수
function asyncFunc(param) {
  // Promise 객체 선언과 반환
  return new Promise((resolve, reject) => {
    // 비동기 함수
    setTimeout(() => (param ? resolve('resolved!') : reject('rejected!')), 1000);
  });
}
```

```js
// asyncFunc 함수를 호출하면 Promise 객체를 생성하고 반환한다.
// 인자에 true를 전달 : resolve 메소드 호출
asyncFunc(true)
  .then(
    // resolve가 실행된 경우(성공), resolve 함수에 전달된 값이 result에 저장된다
    result => console.log(result), // resolved!
    // reject가 실행된 경우(실패), reject 함수에 전달된 값이 reason에 저장된다
    reason => {
      console.log(reason); // rejected!
      throw 'Error:' + reason;
    }
  )
  // 예외 발생 시 호출된다.
  .catch(err => console.log(err));
```

