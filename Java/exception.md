## 예외처리

---

예외에는 컴파일 과정시에 해당하는 예외인 일반예외, 실행시 예측할 수 없이 갑자기 발생하는 실행예외가 있다

- 자바에서는 모든 예외를 클래스로 관리한다. 예외처리에 사용되는 모든 클래스는 `java.lang.Exception`  클래스를 상속받는다.
- 실행예외 클래스인경우 `java.alng.RuntimeException` 클래스를 상속받는다.

### NullPointException

---

참조한 객체가 null값인데, `.` 연산자로 참조하였을 때 발생함

### ArrayIndexOutOfBoundsException

---

배열에서 인덱스 범위를 초과할 경우 발생함

### NumberFormatException

---

문자열을 숫자형으로 파싱할 때, 문자열 내부에 숫자형으로 변경할 수 없는 문자가 들어가 있는 경우 발생함

- Interget.parseInt
- Double.parseDouble

### ClassCastException

---

클래스 타입변환이 이루어질 수 없는 형태로 시도되었을 때 발생함

- 케이스1
    - B, C클래스가 A클래스를 extends 한 상태
    - A로 클래스형변환 하고 B클래스로 인스턴스x 를 생성
    - x를 다시 C클래스로 형변환시도
    - 에러발생! (A클래스로 형변환하지않았음)

## 예외처리하기

---

`try-catch-finally`  구문은 에러가 발생했을 때, 프로그램이 멈추지 않게 예외처리를 할 수 있게끔 도와준다.

```java
try {
	...
} catch (ExceptionClassName e) {
	...
} finally {
	...
}
```

- catch 구문은 다중으로 사용가능하다.
    - catch 구문의 매개변수로 받는 에러의  클래스에 따라 어떤 catch 블럭이 실행될지 결정된다.
- catch 구문은 `if-else` 구문처럼 순차적으로 실행되기 때문에, 모든 예외처리를 받을 수 있는 `Exception` catch 구문은 가장 마지막에 써야한다.

### 예외 처리 떠넘기기

---

메서드내부에 예외처리코드를 작성하고 싶지 않은 경우 메소드 뒤에 `throws ExceptionClass` 구문을 추가하여 외부 코드로 예외처리를 떠넘길 수 있다.



```java
public void method1() {
	try {
		// 이렇게 내부에 써야하는데
	} catch(ClassNotFoundExceptione e) {
	}
}

public void method1() throws ClassNotFoundException {
	// 이런식으로 외부로 떠넘길 수 도있음
	...
}
```