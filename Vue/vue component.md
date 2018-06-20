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



## 비동기적으로 컴포넌트 로딩하기

앱이 이미실행 중일 때 컴포넌트를 로딩해야하는 경우가 있다. 일부 컴포넌트의 모양이 미리 알려지지 않은 경우에는 컴포넌트를 생성할 수 없다. 실제로 렌더링 해야하는 경우에만 컴포넌트를 로드할 수 있다. 만약 다음에도 컴포넌트를 렌더링해야할 경우가 있다면 캐시에서 반환된다.

```Vue
<div id="app">
    <span v-if="loading">loading...</span>
    <period-base></period-base>
</div>

<script>

    Vue.component('periodBase',(resolve, reject) => {
        setTimeout(() => {
            if ((new Date()).getDate() !== 6) {
                resolve({
                    template: `<div>
장미는 4만원입니다.
    </div>`,
                    mounted () { 
                        // (1)
                        this.$parent.$emit('loaded')
                    }
                })
            } else {
                reject("오늘은 일요일이라 문을 안엽니다.")
            }
        }, 1000);
    })
    new Vue({
        el: '#app',
        data: {
            loading: true
        },
        created() {
            (2)
            this.$on('loaded', () => {
                this.loading = false
            })
        }
    })
</script>
```

SetTimeout메서드는 Ajax호출을 시뮬레이션하기 위해 사용했다. 이 컴포넌트를 일요일에 실행하면 문이 닫혔다는 메세지가 뜰것이다.



### 비동기 컴포넌트

> Vue.component('comp-nam', (resolve, reject) => { ... })

일반 컴포넌트는 두 번째인자로 객체를 넘기지만 비동기 컴포넌트는 인자 2개를 받는 함수를 넘긴다.

이 때 첫번째 인자 resolve는 컴포넌트의 속성을 갖고 있는 객체가 사용가능할 때 호출된다. 두번째 인자 reject는 문자열을 인자로 받는 함수이다. 만약 비동기로 동작하는 부분이 에러가 난다면 컴포넌트가 해당 문자열로 대체된다. (이 때, 에러 메세지를 남기자)

(1)비동기 컴포넌트가 마운트되고 나서 그 정보를 부모 컴포넌트에게 보내서 (2)부모컴포넌트에게 특정 일을 시킬 수도 있다.