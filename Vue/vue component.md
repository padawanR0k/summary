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
            type: Number,
		   default: 0, // 기본값을 정해줄 수도 있다.
            validator (value) { // 값을 검증할 수도 있다.
			return value >= 0 && value <= 3; 
            }
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



props의 기본값을 Object 또는 Array타입으로 지정하고 싶다면 함수를 사용하여야한다.

```javascript
default () {
	return {
		greeting: 'hello';
	}
}
```



## 컴포넌트에서 믹스인 사용하기 (상속)

믹스인은 컴포넌트에 높은 유연성을 부여하며 다른 **컴포넌트의 기능을 재사용** 할 수 있는 방법이다.

기본 메커니즘은 이렇다.

1. 컴포넌트의 옵션을 모방한 객체를 정의한다. (아래 코드에서 SuperGreet)
2. 실제 컴포넌트 내부에 mixins 옵션의 배열에 해당 객체를 위치시킨다.

 

### 믹스인 순서

뷰에서는 다양한옵션을 혼합하기위해 다양한 전략을 사용한다.

객체를 포함하는 옵션은 하나의 큰 객체로 병합된다. 예를 들어, 한가지 메서드를 포함한 컴포넌트가 있다고 치자. 믹스인옵션으로 그 컴포넌트를 추가한 두번째 컴포넌트(메서드가 2개인)는 최종적으로 총 3가지 메서드를 가지게 된다.  만약 동일한 이름을 가진 메서드가 존재한다면 해당 믹스인의 키는 무시될 것이다.

훅 함수는 병합되진 않지만 믹스인과 컴포넌트의 함수 모두 실행된다. 이때 믹스인의 훅함수가 더 높은 우선순위를 가진다. (?) 

```vue
<div id="app">
    <greeter></greeter>
    <super-greeter></super-greeter>
  </div>

  <script>
    var Greeter = {
      template: `
        <p>
          {{message}}
          <button @click="greet"> greet </button>
        </p>`,
      data () {
        return {
          message: '...'
        }
      },
      methods: {
        greet() {
          this.message = 'hello'
        }
      }
    }
    var SuperGreeter = {
      mixins: [Greeter], // 이제 Greeter객체의 message와 greet()를 사용할 수 있게되었다.
      template: `
        <p>
          {{message}}
          <button @click="superGreet"> Super greet </button>
          <button @click="greet"> greet </button>
        </p>
      `,
      methods: {
        superGreet() {
          this.message = 'Super Hello'
        }
      }
    }
    new Vue({
      el: '#app',
      components: {Greeter, SuperGreeter}
    })


  </script>
```

