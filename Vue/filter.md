# 필터

첫번 째 버전의 뷰는 내장 필터가 여러 개 존재 했다. 하지만 특수한 상황에서도 잘 동작하는  라이브러리를 온라인상에서 찾는건 쉽기 때문에 최신 버전에선 거의 다 사라졌다. 이전 버전의 뷰에서는 필터의 용도가 필터링, 정렬에 집중하고 있던 반면에 지금은 **후처리**에 더 집중 하고 있다. 이전 버전에서 존재하던 capitalzie를 만들어 보자.

[Demo](https://codesandbox.io/s/w6zm94o0kl)

```vue
<template>
  <div id="app">
    // angular에서 파이프를 사용하는것과 비슷하다
    // {{ 대상 | 필터명 }}
    {{'hello world' | capitalize }} 
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  filters: {
    capitalize: function(string) {
      var [first, ...tail] = string; // 디스트럭쳐링
      return first.toUpperCase() + tail.join("");
    }
  }
};
</script>
```



## 팁

애플리케이션 디버깅을 위해 특정 객체를 JSON표현으로 나타내고 싶다면 `{{ somthing }}`  머스태치를 사용하면된다. 머스태치를 사용하는 것이 유용할 때는 동일한 값이 여러곳에서 변경되거나 변수의 실제 내용을 신속하게 확인하고자 할 때이다.

