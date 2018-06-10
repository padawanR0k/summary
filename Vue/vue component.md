# Vue 컴포넌트

Vue는 순수 HTML 및 CSS와 매우 밀접하게 연관되어있다. Vue 아키텍쳐의 주된 특징은 모든것이 컴포넌트로 구성될 수 있다는 것이다. 컴포넌트로 프로그램을 구성하면 그 크기에 상관없이 독립적인 작업을 할 수 있다. (angular랑 같다)



## 컴포넌트의 생성

> Vue.component('컴포넌트이름', {
>
> ​	...options...
>
> })

컴포넌트의 이름을 camelCase로 지정하면 Vue가 알아서 kebab-case로 변환한다는걸 알아두자.



컴포넌트의 템플릿옵션은 컴포넌트의 모양을 지정하며 Vue인스턴스의 el 옵션은 컴포넌트의 위치를 알려준다.

```vue
Vue.component('light-bulb', {
	template: `
		<div class="light-bulb">
         	<p>
                ereka!
            </p>   
		</div>
	`
})

new Vue({
	el: '#app'
})
```



## 컴포넌트의 지역적 등록

컴포넌트가 Vue 루트 인스턴스와 동일한 범위에 있다면 컴포넌트는 인스턴스에 의해 자동으로 등록된다. 규모 가 큰 애플리케이션이나 다른 컴포넌트를 임포트할 때는 이방법은 안좋다.

수동으로 컴포넌트를 등록해보자

```vue
var lightbulb = {
	template: `
	<div class="light-bulb">
    	<p>
             eureka!
        </p>    
	</div>	
`
}

new Vue({
	el: '#app',
	components: {
		'light-bulb': lightBulb
	}
	
})
```

이 방법을 지역적 등록(local resitration) 이라고 부른다.



## props를 사용한 데이터 전달

동일한 페이지에서 동일한 컴포넌트를 재활용하여 여러 벌 사용할 수 있지만 각 컴포넌트가 무엇을 해야하는지 알려줄 수 있는 수단이 존재해야한다.

Vue에서는 모든것이 반응형으로 동작하기 때문에 props를 사용하면 컴포넌트와 직접적으로 통신 할 수 있다. 컴포넌트에서 props를 선언할 때 두가지를 유의하여야한다.

- props는 단방향통신이다
- 고정된 값이거나 동적으로 변경될 수 있다.



```vue
Vue.component('sound-icon', {
	template: `
		<span>{{sound[level]}}</span>
	`,
    props:  { // 넘길 props의 타입을 지정해준다. TypeScript에서는 변수뒤에 콜론으로 타입을 붙이듯
        level: {
            type: Number
        }
    }
	data() {
		return{
			sound: ['🔈', '🔉','🔊']
		}	
	}
})
new Vue({
	el: '#app',
	data: {
		soundLevel: 0
	}
})
<div id="app">
    <label>SOund Level</label>
	<input type="number" v-model.number="soundLevel">
    <sound-icom :level="soundLevel"></sound-icom>
</div>
```

