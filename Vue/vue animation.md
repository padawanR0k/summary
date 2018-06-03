# 전환과 애니메이션

모든 전환은 네가지 종류에 따라 적용된다.

| 이름           | 적용시점               | 제거시점     |
| -------------- | ---------------------- | ------------ |
| v-enter        | 엘리먼트가 삽입되기 전 | 한 프레임 후 |
| v-enter-active | 엘리먼트가 삽입되기 전 | 전환 종료 시 |
| v-enter-to     | 한 프레임 후           | 전환 종료 시 |
| v-leave        | 전환 시작 시           | 한 프레임 후 |
| v-leave-active | 전환 시작 시           | 전환 종료 시 |
| v-leave-to     | 한 프레임 후           | 전환 종료 시 |

v는 전환의 이름이다. 따로 지정하지 않으면 v가 사용된다.



## 간단한 예시

여기에 사용된 애니메이션은 네가지 포인트로 구성된 FLIP기술을 따름

- 시작 First
  - 애니메이션의 첫 번쨰 프레임에서 속성을 가져온다. 여기서는 `transform: translateX(200px)`이다. 즉 시작점이 우측으로 200px 떨어져 있다.
- 끝 Last
  - 마지막 프레임의 속성을 가져온다.
- 반전 Inver
  - 첫 번째 프레임과 마지막 프레임 사이에 등록한 속성 변경을 뒤집는다.
- 재생 Play
  - 지금까지 수정한  모든 속성에 대한 전화을 생성한다.

```vue
<template>
  <div id="app">
    <button @click="taxiCalled = true">
      call a cap
    </button>
    <transition enter-class="slideInRight" enter-active-class="go">
      <p v-if="taxiCalled">🚕</p>
    </transition>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      taxiCalled: false
    };
  }
};
</script>
<style scoped>
  .slideInRight {
    transform: translateX(200px);
  }
  .go {
    transition: all 2s ease-out;
  }
</style>
```



## 자바스크립트로 애니메이션 적용하기

자바스크립트를 이용한 애니메이션은 css로 적용한 애니메이션의 성능과 유사하거나 더 빠르다. velocityJS 라이브러리를 사용하여 애니메이션을 적용시켜보자.

https://velocity.org/ 에서 cdn주소가져와 추가하자. 그후 위 코드에서 일부분만 수정한다.

```vue
<transition @enter="enter" :css="false">
<!-- :css="false" css적용을 중지시킨다.-->
<!-- @enter후크에 enter메서드를 바인딩한다. -->
    <p v-if="taxiCalled">🚕</p>
</transition>

<script>
...
methods: {
    enter (el) {
      Velocity(el,
        { opacity: [1, 0], translateX: ["0px", "200px"]},
        { duration: 2000, easing: "ease-out" }
      )
    }
  }
</script>
```

> Velocity(효과를줄 엘리먼트, 애니메이션 프로퍼티, 효과옵션)

1. 효과를 줄 엘리먼트 el 은 `<p>`
2. 줄 효과는 배열로 작성한다. 투명도를 0 -> 1로 x좌표를 200px -> 0px로
3. 애니메이션이 얼마나 어떻게 작용하는지에 대한 옵션 : 2초 ease-out

