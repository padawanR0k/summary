## 인터페이스

---

객체에 대한 명세 역할을 한다. 개발시 추상메소드와 살짝 비슷하게 객체 구조를 잡는데 도움이 되기도 한다. ts상에서도 인터페이스를 객체에 대한 타입을 선언해주기 위해 자주 사용한다.

다른점은 ts의 인터페이스에서는 값을 지정할 수 없지만 자바에서는 상수값을 지정하여 클래스에서 확장할 때 사용할 수 있게된다. (상속받을 수 있다.)

```java
public interface Calc {
	public int MAX_NUM = 1000;
	public abstract sum(int a, int b);
}

public class Calculator implements Calc{
	public int sum(int a, int b) {
		return MAX_NUM > a + b ? a + b : MAX_NUM;
	}
}
```

### 다중 구현

---

class를 extend 때는 1개의 클래스만 가능하나, interface를 implements할때는 N개를 받을 수 있다.

```java
public interface Calc {
	public int MAX_NUM = 1000;
	public abstract sum(int a, int b);
}

public interface Calc2 {
	public abstract subtract(int a, int b);
}

public class Calculator implements Calc, Calc2 {
	public int sum(int a, int b) {
		return MAX_NUM > a + b ? a + b : MAX_NUM;
	}

	public int subtract(int a, int b) {
		return a - b;
	}
}
```

### 추상 클래스와 인터페이스의 다른점

---

- 추상 클래스
    - 인스턴스를 바로 생성할 수 는 없지만 메소드, 추상메소드 선언이 가능함
    - 클래스에서 확장가능 (extend)

        → 생성자 존재

- 인터페이스
    - 클래스에서 구현가능 (implements)
        - → 생성자 미존재
    - 오직 추상메소드만 선언가능

### 타입 변환과 다형성

---

인터페이스를 활용하여 클래스를 구현한 경우, 개발시 하위 클래스 문제가 생겼을 때 문제가 되는 클래스만 교체하면  됨으로써 쉽고 빠르게 수정할 수 있다.

```java
interface I {
	void method1();
}

// A클래스에 문제가 있다.
I i = new A();

// B클래스를 새로 만들자
I i = new B();
```

A, B 모두 같은 인터페이스를 구현하였으므로 같은 기능을 수행한다.

### 매개변수의 다형성

---

메소드를 작성할 때, 파라미터에 대한 타입이 매번 달라질때마다 메소드를 하나씩 추가하게되면 코드가 많아지며 읽기 어려워진다. 만약 특정 메소드가 클래스를 파라미터로 받으면서 클래스들이 공통적인 인터페이스로 구현하였다면, 매개변수의 타입을 그 인터페이스로 하면, 여러 메소드를 만들필요 없이 하나의 메소드로 해결이 가능하다.

```java
public interface Person {
	public void hello();
}

public class Korean implements Person { public void hello() { System.out.println("안녕"); }}
public class American implements Person {}

// 가정해보자
void talkHelloKorean(Korean p) {

}
void talkHelloAmerican(Korean p) {

}
// 굳이 이렇게 말고

void talkHello(Person p) {
	p.hello()
}

```

### 인터페이스의 상속

---

인터페이스는 클래스와 다르게 다중상속을 지원한다. 다중상속을 받은 인터페이스를 구현하는 클래스는 모든 추상메소드에 대한 실체 메소드를 구현해야 한다.

```java
public interface Second {
	public void second();
}

public interface Third {
	public void third();
}

public interface First extends Second, Third  {
		public void first();
}

public class FirstClass implements First {
	public void first() {
	}
	public void second() {
	}
	public void third() {
	}
}
```

### 디폴드 메서드

---

자바 8부터 지원하는 기능으로써, 디폴트 메서드는 인터페이스내에서 구현 코드까지 작성한 메서드를 뜻한다. 이 메서드 또한 extend 된 클래스에서 `override` 가 가능하다.

```java
public interface Test {
	default void description() {
		...
	}
}

public class Real implements Test{

}

Real real = new Real()
real.description(); // Test 인터페이스에서 작성한 default 메서드 실행됨

```

### 정적 메서드

---

자바 8부터 지원하는 기능으로써, static property와 비슷하게 인스턴스 생성없이 바로 사용가능한 메서드이다. (약간 함수처럼 사용하면 될듯??)

```java
public interface Test {
	static void hello() {
		...
	}
}

Test.description(); // Test 인터페이스에서 작성한 static 메서드 실행됨. 인스턴스 생성없이!

```

### private 메서드

---

자바 9부터 지원하는 기능으로써, 실행블록이 존재하는 메서드이다. private 메서드는 해당 인터페이스안에서만 사용가능하며, 구현한 클래스에서 사용하거나 override가 불가능하다.

```java
public interface Test {
	private void hello() {
		...
	}
}

Test.description(); // Test 인터페이스에서 작성한 static 메서드 실행됨. 인스턴스 생성없이!

```

### interface 다중 구현

---

클래스와는 다르게 인터페이스끼리는 한번에 여러 인터페이스를 확장할 수 있다.

```java
interface A {}
interface B {}
interface C extends A,B {}
```

### interface구현, class 상속 같이 쓰기

---

인터페이스를 구현하면서 클래스를 확장하는것도 가능하다.



```java
interface A{}
class B {}

class C extends B implements A {}
```

## 중첩

---

자바에서 인터페이스와 클래스는 중첩하여 사용하는것도 가능하다.

### 중첩 클래스

---

- 멤버 클래스
    - 정적 멤버
        - 다른 스태틱 멤버 변수처럼 상위 클래스에서 직접 접근 가능
    - 인스턴스 멤버
        - 다른 멤버변수처럼 인스턴스가 생성되기전까지는 접근 불가능
- 로컬 클래스
    - 메소드 내부에 존재하는 클래스이며, 메소드의 실행블록이 곧 라이프사이클이여서 해당 메소드가 실행될때만 접근가능한 클래스

그런데, 로컬 클래스를 사용하는 경우가 있을까?
인스턴스 멤버로 클래스를 사용하는것보다는 일반 클래스를 만드는게 더 낫지않나?

### 멤버 클래스 접근제한

---

멤버 클래스에서 상위 클래스의 모든 멤버 변수에 접근가능하다.

```java
public class A {
	int field1;
	void method1() {}

	static int field2;
	static void method2() {}

	class B {
		void method() {
			field1 = 10;
			method1();

			field2 = 10;
			method2();
		}
	}
}
```

static 멤버 클래스에서 상위 클래스의 static 멤버 변수에만 접근가능하다.

```java
public class A {
	int field1;
	void method1() {}

	static int field2;
	static void method2() {}

	static class C {
		void method() {
			field1 = 10; // 불가능
			method1(); // 불가능

			field2 = 10;
			method2();
		}
	}
}
```

### 중첩 클래스에서 부모 클래스 참조얻기

---

중첩 클래스의 `this`  는 중첩클래스 자기 자신을 뜻하기 때문에 부모클래스에 접근하려면 `부모클래스이름.this`로 접근하여야 한다

```java
public class A {
	void hi() {}
	public class B {

		void method1() {
			A.this.hi()
		}
	}
}

...

public class Test {
	public void main (String[] args){
		A a = new A();

		A.B b = a.new B()
		b.method1()
	}

}
```

- 이런 방식으로 리스너같은것을 구현해보자

```java

public class Button {
	OnClickListener listner;
	void setOnclickListener (OnClickListener listner) {
		this.listner;
	}

	void touch() {
		listener.onClick();
	}

	static interfce OnClickListener{
		void onClick();
	}
}

// 중첩된 정적 인터페이스를 구현해야한다.
// onClick메소드를 구현함
public class Listener implements Button.OnClickListener {
	@Overrid
	public void onClick() {
		System.out.print("onClick");
	}
}

...

// Button 클래스에는 무조건 리스너와 리스너를 수정하는 메소드가 존재한다
// 리스너에 대한 코드는 매개변수로 전달된 값이 할당되기 때문에
// 어떤 값을 주느냐에 따라 버튼에 대한 리스너를 본인이 마음대로 정할 수 있다.
class Test {
	public void main (String[] args){
		Button btn = new Button();
		btn.setOnClickListener(new CallListener());

		btn.touch();
	}

}
```