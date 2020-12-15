
## 내부 클래스

---

클래스 내부에  선언된 클래스

- 왜?
    1. 내부 클래스와 외부 클래스가 커플링이 강한 경우
    2. 다른 클래스와 협력되어 사용할 일이 없는 경우

- 인스턴스 내부 클래스

    ```java
    class OutClass {
    	private Inclass inClass;
    	public OutClasS() {
    		inClass = new InClass();
    	}

    	class InClass {
    		int inNum = 100;
    		void inTest() {
    			System.out.println("내부 클래스의 함수입니다.")
    		}
    	}

    	public void usingClass() {
    		inClass.inTest();
    	}
    }
    ```

    1. `OutClass`의 인스턴스가 생성될 때, 생성자 내부에서 `InClass` 인스턴스도 생성하기 때문에, 이후에 `OutClass.usingClass()` 를 실행하면 내부클래스의 메소드를 사용하게 된다. 이 말인즉슨 내부 클래스의 메서드를 생성하기전에 생성이 먼저 되게 해야한다. 그렇게 하지않는 경우, 방어코드를 않으면 오류가 발생할 수 있다.

        (뭔가 이상한 문법같다... 이렇게 내부에 작성하게되면 가독성이 더 떨어질거 같은 느낌)

    2. 인스턴스 내부 클래스에는 정적 메소드, 멤버변수를 선언할 수 없다. 외부 클래스가 먼자 생성되어야 사용할 수 있기 때문이다.

- 정적 내부 인스턴스

    ```java
    class OutClass {
    	private Inclass inClass;
    	public OutClasS() {
    		inClass = new InClass();
    	}

    	static class InClass {
    		int inNum = 100;
    		void inTest() {
    			System.out.println("내부 클래스의 함수입니다.")
    		}
    	}

    	public void usingClass() {
    		inClass.inTest();
    	}
    }
    ```

    - 내부 클래스가 외부 클래스생성과 무관하게 사용할 수 있어야하는 경우, 정적 내부 클래스를 사용하면 된다.
    - 정적 내부 클래스의 메서드는 private이 아니라면 다른 클래스에서 바로 사용가능하다.
- 지역 내부 클래스
    - 지역변수처럼 클래스 내부에 지역변수로 생성한 클래스를 말한다.
    - 지역 내부 클래스에서 사용하느 메서드의 지역변수는 상수로 바뀐다.

        ```jsx
        class OutClass {
        	private Inclass inClass;
        	public OutClasS() {
        		inClass = new InClass();
        	}

        	public void method1(int i) {
        		int num = 1;

        		class InClass {
        			int inNum = 100;
        			void inTest() {
        				i = 10; // 오류 발생! 매개변수는 상수처리되기 때문
        				num = 100; // 오류 발생! 상위 메소드 내부 변수는 상수처리 되기때문
        			}
        		}

        		return new InClass();
        	}

        	public void usingClass() {
        		inClass.inTest();
        	}
        }
        ```

- 익명 내부 클래스
    - 익명 내부 클래스는 여러번 재활용 되지않지만, 클래스를 변수로 사용하여야 할 때 쓴다.
    - 단 하나의 인터페이스, 또는 단하나의 추상 클래스를 바로 생성 할 수 있다.

    ```java
    class OutClass {
    	private Inclass inClass;
    	public OutClasS() {
    		inClass = new InClass();
    	}

    	public void method1(int i) {
    		int num = 1;

    		Runnable runner = new Runnable() {
    			int inNum = 100;


    			@Overrid
    			public void run() {
    				System.out.println("Runnable 구현")
    			}

    			void inTest() {
    				i = 10; // 오류 발생! 매개변수는 상수처리되기 때문
    				num = 100; // 오류 발생! 상위 메소드 내부 변수는 상수처리 되기때문
    			}
    		};

    		return Runnable;
    	}

    	public void usingClass() {
    		inClass.inTest();
    	}
    }
    ```

    → 클래스를 따로 생성하지않고 `Runnable` 인터페이스를 확장한 익명 클래스를 `method1` 메서드 내부에 작성하였다. 기존 클래스 작성시 클래스의 코드블럭 끝난 후에 `;` 를 안붙히지만 익명클래스를 작성한 경우 붙여야한다.

    - 보통 익명 내부 클래스는 UI 이벤트를 처리하는데 많이 사용한다. 현재는 안드로이드 위젯에서 이벤트처리 핸들러를 구현할 때 사용한다.

## 람다식

---

함수형 프로그래밍 방식 (Lambda  expression)

기존 자바에서는 지원하지 않았지만 최근 함수형 프로그래밍의 장점이 대두되면서 해당 문법을 지원하게됨

기존

```java
int add(int x, int y) {
	return x + y;
}
```

람다식

```java
(int x, int y) -> {
	return x + y;
};
```

- 문법
    - 매개변수1개일 때 소괄호생략가능

        ```java
        str -> {System.out.println(str);}
        ```

- 코드블럭이 1줄일 때 중괄호생략가능

    ```java
    str -> System.out.println(str);
    ```

- `return` 구문 생략가능, 위와동일

---

- 리마인드 - 언어별 익명함수
    - js

        ```jsx
        () => {
        	console.log("익명함수")
        }
        ```

    - python

        ```python
        arr = [1,2,3]
        list(map(lambda x: x*2, arr))
        # [2,4,6]
        ```

---

- 함수형 프로그래밍은 순수함수를 지향하여 사이드이펙트를 주지 않도록 구현하는 방식이다. 함수가 입력받은 데이터 이외에 사이드이펙트가 적기 때문에 자료를 동시에 처리하는 병렬처리에 적합하다.

### 함수형 인터페이스

---

자바에서 람다식은 메서드 이름이 없고 메서드를 실행하는데, 필요한 매개변수와 매개변수를 활용한 실행 코드를 구현하는것이다.

인터페이스에 람다식 메소드를 선언하고, 람다식을 구현한 곳에서 타입으로써 사용한다.

`@Functionallnterface`  어노테이션은 함수형 인터페이스를 구현한 곳에 사용하여, 2개 이상의 메서드를 선언하지 못하게 한다.

```java
package lambda;

@Functionallnterface
public interface MyNumber {
	int getMax(int num1, int num2);
}

...

MyNumber max = (x, y) -> (x>=y) ? x : y;
max.getMax(10, 100);
// 해당 람다식은 getMax를 구현하였다.
```

- 람다식또한 익명 내부 클래스처럼 지역 변수가 상수(final)로 변하기 때문에, 수정하려하면 오류가 발생한다!

## 스트림

---

배열, 컬렉션의 자료를 일관성 있게 처리하게끔 도와주는 패키지

자료에 따라 기능을 새로 구현하지않고, 같은 방식으로 메서드를 호출할 수 있다.

### 스트림 연산

---

- filter()
    - return된 값이 true인 경우만 추출함
- map()
    - return된 값이 새로운 배열들의 값이 됨
- count()
    - 요소의 개수 카운팅
- forEach()
    - 요소를 한개씩 iter함
- sum()
    - 요소의 합
- reduce()
    - 요소를 iter하면서 accumulate 값을 가지면서 최종적으로 하나의 결과값을 가짐

stream은 최종연산을 하게되면 요소가 소모된다. → 여러번하려면 스트림을 여러번 만들어야한다.

```java
List<String> sList =  new ArrayList<String>();
sList.add("a");

String<String> stream = sList.stream();
stream.forEach(s -> System.out.println(s + "1"));

String<String> stream2 = sList.stream();
stream.forEach(s -> System.out.println(s + "2"));
```

### 스트림의 특징

- 추상화된 연산수행; 여러 자료구조에 대해 일관성있게 처리 할 수 있는 메서드 제공
- 재사용 불가

    `Exception in thread "main" java.lang.IllegalStateException: stream has already been operated upon or closed`

    → 왜 이렇게 설계했을까..? js나 python에서는 보지 못한 구조다

- 원본 데이터를 변경하지 않음
- 중간연산, 최종연산으로 나누어져 있다.