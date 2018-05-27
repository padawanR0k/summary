# 계산된 속성으로 리스트 정렬하기

v-for구문 내부에서 필터를 사용한 정렬은 Vue가 버전업 되면서 사라졌다. 계산된 속성으로 목록을 정렬하는 것이 훨씬 더 많은 유연성이 제공되며 정렬을 위해  모든 사용자 지정논리를 구현 할 수 있다. 엑셀처럼 목록을 정렬할 수 있는 기능을 만들어보자.

[Demo](https://codesandbox.io/s/z6k0939vy4)

## 설명

```Vue
<template>
  <div id="app">
    정렬기준: 
    <button :class="nameOrder === 1 ? 'descending' : 'ascending'" @click="change('name')">이름 정렬</button>
    <button :class="countryOrder === 1 ? 'descending' : 'ascending'" @click="change('country')">국가명 정렬</button>
    <button :class="yearOrder === 1 ? 'descending' : 'ascending'" @click="change('year')">나이순서 정렬</button>
    <!-- :class="yearOrder === 1 ? 'descending' : 'ascending' -->
    <!-- 현재 목록정렬이 오름차순인지 내림차순인지 css로 표현 -->
    <!-- @click="change()" 정렬기준이 무엇인지 알려줌 -->   
      
    <ol class="list">
      <!-- sortBy라는 계산된 속성를 사용하여 리스트를 렌더링한다. -->
      <li v-for="person in sortBy" :key="person.name">
        <span>이름: {{person.name}} </span>
        <span>나라: {{person.country}} </span>
        <span>나이: {{person.year}}</span>
      </li>
    </ol>
  </div>
</template>
```

```Vue
<script>
export default {
  data() {
    return {
      // 현재의 정렬기준을 나타낸다.
      standard: "unSorted", 
        
      // 정렬할 데이터들  
      people: [
        { name: "lee", country: "korea", year: 12 },
        { name: "sakura", country: "japan", year: 20 },
        { name: "tom", country: "usa", year: 25 },
        { name: "jessica", country: "brazil", year: 14 },
        { name: "wang", country: "china", year: 30 }
      ],
        
      // 정렬기준의 오름차순, 내림차순을 구별함
      nameOrder: 1,
      countryOrder: 1,
      yearOrder: 1
    };
  },
  computed: {
    // 정렬된 배열 
    sortBy() {
      if (this.standard === "unSorted") {
        return this.people
      } else {
        return this.people.sort((a, b) => {
          if (this.standard !== "year") {
            return a[this.standard] > b[this.standard] ? 
              this[this.standard + 'Order'] 
              : this[this.standard + 'Order'] * -1;
          } else {
            return (a[this.standard] - b[this.standard]) * this.yearOrder;
          }
        });
      }
    }
  },
  methods: {
    change(str) {
      // 정렬기준과 함께 차순 변경
      this.standard = str;
      this[this.standard + 'Order'] *= -1; 
    }
  }
};
</script>
```



sortBy는 정렬된 people배열을 반환한다. 계산된 속성을 사용하면 결과가 캐싱된다. 결과가 필요할 때마다 원래 목록이 변경되지 않았다면 함수가 호출되지 않고 캐시된 결과가 반환됨 