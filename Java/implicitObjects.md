
## 도큐먼트

---

- 과거 7버전 (썬라이트)

    [Java Platform SE 7](https://docs.oracle.com/javase/7/docs/api/)

- 최신 15버전 (오라클)

    [JDK 15 Documentation - Home](https://docs.oracle.com/en/java/javase/15/)

자바에서 기본으로 제공하는 클래스, 인터페이스들의 도큐먼트

## Object

---

자바에서의 모든 클래스는 Object 클래스를 상속받는다.  다른 클래스를 상속받지않으면 암시적으로 `java.lang.Object` 를 상속하게된다. (js에서도 모든 것의 부모 prototype은 Object였는데, 비슷한 구조다...)

### `Object.equals()`

- 두 객체를 얕은 비교하여 값이 동등한지 확인한다. 주소값이 달라도 논리적인 값이 동등하면 `true`를 반환한다.

### `Object.hashCode()`

- 객체의 해시코드란 객체를 식별하는 하나의 정수값을 뜻한다. `.equals()`  가 `true` 여도 실제 해시코드값이 다르면 다른객체이다.

### `Object.toString()`

- 객체의 정보를 문자열로 표기한 값을 반환한다. 기본적으로는  `클래스이름@16진수해쉬코드` 를 반환한다.

### `Object.clone()`

- 객체의 멤버변수의 값을 그대로 복사하여 새로운 인스턴스를 만들어 리턴한다.
- 만약 이 메소드를 override 한 경우, `Clonealbe`을 `extedns` 해야하고 `CloneNotSupportedExcepetion` 예외처리를 해주어야한다.

## System 클래스

---

자바는 운영체제단에서 실행되는게아니라 JVM위에서 실행된다. 따라서 운영체제의 모든기능을 사용하긴 어렵다.하지만 System 클래스를 사용하면 운영체제의 일부 기능들을 이용할 수 있다. *System 클래스의 모든 필드와 메소드는 정적(static)이다.*

### `System.exit(status)`

- 현재 실행되고 있는 프로그램을 강제종료시킨다
- exit 메서드는 `종료상태값` 매개변수를 전달받는데, 일반적으로 0(정상종료)을 전달한다

### `System.currentTimeMillis()` , `System.nanoTime()`

- 코드가 실행되는데 걸린 시간을 측정한다. 첫 번째로 실행된 경우 측정시간의 시작이며 두번 째로 실행된 겨우 측정시간의  끝이다. 두 값의 차이가 걸린 시간이 된다.

## String 클래스

---

자바의 문자열은 java.lang에 소속된 String 클래스로 관리한다.

### `String 생성자`

- 전달하는 매개변수의 따라 4개의 생성자가 존재한다.
    - `new String(byte[] 문자)`
        - 모든 문자를 String객체로 생성
    - `new String(byte[] 문자, String charset)`
        - 지정한 문자셋으로 디코딩
    - `new String(byte[] 문자, offset, length)`
        - offset위치부터 지정한 길이만큼 String객체로 생성
    - `new String(byte[] 문자, offset, length, charset)`
        - offset위치부터 지정한 길이만큼 String객체로 생성 + 지정한 문자셋으로 디코딩

### `String 메서드`

- 문자열의 추출, 비교, 찾기, 분리, 변환등 다양한 메소드를 가지고 있다.

[자바 String 클래스의 메소드](https://kutar37.tistory.com/entry/%EC%9E%90%EB%B0%94-String-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%9D%98-%EB%A9%94%EC%86%8C%EB%93%9C)

[https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F232F4E4955F77F890B](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F232F4E4955F77F890B)

- String은 원시타입처럼 쓰이지만 참조형 클래스이며 불변객체이다. `+` 연산으로 글자를 합칠 수는 있지만, 기존 데이터가 변경되는것이 아니라 새로운 String 인스턴스를 생성한다.
- String 인스턴스가 값이 동일하다면 같은 객체를 참조하게끔 되어있다. === 메모리의 번지수가 동일하다.

    ```java
    String a1 = new String("a");
    String a2 = "a";
    String a3 = "a";

    a1 == a2 // false
    a2 == a3 // true

    // equal 메서드는 값만 비교한다.
    a1.equal(a2) // true
    a2.equal(a3) // true
    ```

## Wrapper 클래스

---

자바는 기본 타입의 값을 갖는 객체를 생성할 수 있다. wrapper객체의 특징은 내부에 있는 기본 타입값은 외부에서 변경할 수 없다는 것이다. 변경을 위해서는 새로운 wrapper 객체를 만들어야한다.

제네릭은 primitive type을 받지않는다. 래퍼클래스를 이용하여 제네릭을 사용할 수 밖에 없으니 박싱을 사용한다.

[https://t1.daumcdn.net/cfile/tistory/2578CC465739F8840A](https://t1.daumcdn.net/cfile/tistory/2578CC465739F8840A)

```java
public class TimeTest {
	public static void main(String[] args) {
		long start = 0;
		long end = 0;
 
		//primitive type
		start = System.currentTimeMillis();
		fibonacci1(30);
		end = System.currentTimeMillis();
		System.out.println("int를 이용한 피보나치 연산::" + (end - start));
 
 
		//Wrapper Class
		start = System.currentTimeMillis();
		fibonacci2(30);
		end = System.currentTimeMillis();
		System.out.println("Integer를 이용한 피보나치 연산::" + (end - start));
	}
 
	public static int fibonacci1(int value) {
		if (value == 0 || value == 1)
			return 1;
		elsereturn fibonacci1(value - 1) + fibonacci1(value - 2);
	}
 
	public static Integer fibonacci2(int value) {
		if (value == 0 || value == 1)
			return 1;
		elsereturn fibonacci2(value - 1) + fibonacci2(value - 2);
	}
}
```

- int 사용: 8
- Interger 사용: 33

- 박싱으로 데이터가 생성되게 되면 힙영역 값이 저장되게된다. → 같은 값으로 저장하여도 서로의 address값이 다르다.

    ```java
    Interger obj1 = 100;
    Interget obj2 = 100;

    obj1 == obj2; // false
    obj1.initValue() == obj2.initValue(); // true
    obj1.equals() == obj2.equals(); // true
    ```

- **wrapper class는 왜 사용하는 걸까 ?**

- 특정클래스 내부의 정보 가져오기

    ```java
    import java.lang.reflect.Constructor;
    import java.lang.reflect.Field;
    import java.lang.reflect.Method;

    Class cls = Class.forName("java.lang.String")

    Constructor[] cons = cls.getConstructors();
    Field[] fields = cls.getFields(); // 해당클래스의 모든 멤버변수
    Method[] methods = cls.getMethods() // 해당클래스의 모든 메소드

    ```

### newInstance() 클래스 생성

---

```java
public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
	Person person1 = new Person(); // 일반적인 생성자로 인스턴스 생성방법

	Class pClass = Class.forNAme("class.Person");
	Person person2 = (Person)pClass.newInstance(); // newInstance() 메소드로 생성 후 원래 클래스로 다운캐스팅
}
```

## Math 클래스

---

자바도 다른언어들과 동일하게 수학을 위한 메소드들을 Math 클래스에서 제공한다. 다른언어와 비슷하니 이건 필요할때 자세히 찾아보자.