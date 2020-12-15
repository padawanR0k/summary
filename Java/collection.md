데이터를 효율적으로 관리하기 위한 자료 구조 사용

## 제네릭

---

변수의 타입을 지정해 줄 때, 여러 참조 자료형을 사용할 수 있도록 프로그래밍하는것

```java
// Map 클래스는 제네릭으로 key와 value를 받는다고하자.

Map<String, Interger> // 이 런경우 value에 Interger타입만 할당가능하다
Map<String, T> // 이 런경우 value에 아무타입이나 들어올 수 있다.
```

`<>` : 다이아몬드 연산자

- 제네릭으로 구현하면 컴파일러는 대입된 자료형이 쓰였는지 확인하고, class파일을 생성할 때 T를 사용한 곳에서 지정된 자료형에 따라 컴파일하므로 형변환 하지 않아도 된다.
- ts에서의 제네릭과 동일한듯하다. ts는 자바개념을 정말 많이 따온거같다...

### 제한적인 제네릭

---

제네릭으로 타입을 지정한 경우, 특정 조건을 만족하는 타입만 받고 싶은 경우가 있을 것이다. 그럴경우, 제네릭으로 지정한 변수뒤에 `extends ClassName` 을 사용하면된다.

```java
public abstract class Person {
	private abstract boolean hasMind;
	public abstract void say();
}

public class Korean extends Person {
	private boolean hasMind = true;
	public void say() {
		System.out.println("안녕");
	}
}
public class American extends Person {
	private boolean hasMind = true
	public void say() {
		System.out.println("hello");
	}

}

public Class People<T extends Person> {
	private T[] people;

	public void setPeople(T people) {
		this.people = people;
	}
}

new People([new Korean()])
new People([new American()])
```

- extends 없이 제네릭은 `Object` 클래스로  변환된다. → 기본 `Object` 메서드만 사용가능. `extends` 키워드를 사용하게되면 내부적으로 형변환되어 확장한 클래스의 메서드를 사용할 수 있다.

### 제네릭 메서드

---

메서드에도 제네릭을 사용할 수 있다.

```java
public class Point<T, V> {
	T x;
	V y;

	Point(T x, V y) {
		this.x = x;
		this.y = y;
	}

	public T getX() { return x; }
	public T getY() { return y; }
}

...

Point<Interger, Double> p1 = new Point<>(0, 0.0);
// 빈 연산자만 사용하고 타입은 입력한 값에서 추론하였다.

```

- 제네릭의 스코프는 선언한 곳에만 유효하다. 무슨 소리?

    ```java
    class Shape<T> {
    	public static <T, V> double makeRectangle(Point<T, V> p1, Point<T, V> p2)) {

    	}
    }
    ```

    - `Shape<T>` 는 `makeRectangle`  메소드의 `T` 와 다른 T임!

## 컬렉션 프레임워크

---

자바에서 프로그래밍시에 자주 쓰이는 자료구조를  제공하고 있는데, 이를 `컬렉션 프레임워크`라고 함. `java.util` 패키지를 살펴보자

### 구조

- Collection
    - List (순서존재, 증복허용)
        - ArrayList
        - Vector
        - LinkedList
    - Set
        - HashSet
        - TreeSet
- Map (순서미존재, 중복 비허용)
    - Hashtable
    - HashMap
    - TreeMap

## List 인터페이스

---

### ArrayList

---

- 객체를 순차적으로 저장할 때 사용
- 기본적으로는 동기화 지원 안함. 동기화가 필요한 경우 다음과 같이  선언해야함

    ```java
    Collections.synchronizedList(new ArrayList<String>());
    ```

### LinkedList

---

- 물리적인  메모리는 떨어져있어도 논리적인 메모리를 연결하여, 배열 크기에 제한 받지않는 자료구조 (첫 생성시에 배열크기를 신경안써도됨)
- 구조
    - [data1|다음 요소의 주소]→[data2|다음 요소의 주소] → [data3|null]
    - 마지막요소는 `null`이나 `0`을 저장한다.

- 요소 제거
    - A→B→C 형태인 경우, A데이터의 next  주소값을 C로 변경하면됨
- 배열과 링크드 리스트의 다른점?
    - 링크드 리스트
        - 동적인 크기
        - 자료를 중간에 추가, 삭제하는 경우 빠름
    - 배열
        - 삽입, 삭제가 많은 경우 링크드 리스트보다는 느림

            → 바로 근처에 존재하는 메모리에 다음 데이터가 존재해서, 순차적으로 접근할때는 빠르다.

- iterator
    - Collection 인터페이스를 구현한객체는 `iterator()` 메서드를 구현하고 있다. 해당 메서드를 실행하면 순회가능한 `Iterator` 클래스가 반환된다.
    - iterator는 순회할떄 두가지 메서드를 사용한다.
        - `boolean hashNext()`
            - 다음 요소의 존재 여부를 반환한다.
        - `E next()`
            - 다음 요소를  반환한다.

## Set 인터페이스

---

중복을 혀용하지 않은 집합 자료구조이다. List인터페이스와 다르게 정렬되어있지않다.  (파이썬, 자바스크립트에서도 봐왔던 자료구조)

```java
package collection;

import java.util.HashSet;

/**
 * MemberHashSet
 */
public class MemberHashSet {
	private HashSet<Member> hashSet;

	public MemberHashSet() {
		hashSet = new HashSet<Member>();
	}

	public void addMember(Member member) {
		hashSet.add(member);
	}

	public boolean removeMember(int memberId) {
		boolean removed = hashSet.removeIf(m -> (m.getMemberId() == memberId));
		return removed;
	}

}
```

### TreeSet

---

Colection 인터페이스나 Map 인터페이스를 구현한 클래스중 이름이 `Tree` 로 시작하는 클래스는 값을 정렬하여 저장한다.

- 자바에서는 정렬을 구현하기 위하여 `이진트리(binary tree)`를 사용한다.
- 이진 검색트리 `(Binary Search Tree: BST)`

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd4a5567-edd9-490f-8ac4-54c23b98a5da/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd4a5567-edd9-490f-8ac4-54c23b98a5da/Untitled.png)

    - 부모노드는 2개의 자식노드를 가진다.
    - 왼쪽 자식노드는 부모 노드보다 항상 작은 값을 가진다.
    - 오른쪽 자식노드는 부모 노드보다 항상 큰 값을 가진다.

    → 특정값을 찾을 때, 비교 범위가 1/2 만큼씩 줄어들어 효율적이다.

- **Comparable 인터페이스**
    - TreeSet 인터페이스에서 정렬하기 위해서는, TreeSet 내부에 있는 클래스에 대해서 해당 인터페이스들을 구현하여야한다. 그렇지 않은 TreeSet은 오류가 발생한다!

    ```java
    public class Member implements Comparable<Member> {
    	...
    	@Override
    	public int compareTo(Member member) {
    		return (this.memberId - member.memberId);
    	}
    }
    ```

- **Comparator 인터페이스**
    - Comparable 인터페이스와 비슷하게 `compare(E e1, E e2)` 메서드를 구현하여야한다. `compareTo(E e)`와 다르게 매개변수로 2개를 받는다.

    ```java
    public class Member implements Comparator<Member> {
    	...
    	@Override
    	public int compare(Member member) {
    		return (this.memberId - member.memberId);
    	}
    }
    ```

    - 이미 `Comparable` 을 구현했지만, 정렬방식을 커스텀 하고싶은경우 `Comparator`을 사용한다. ex) 내림차순, 2가지이상의 정렬조건
    - 이 경우, TreeSet 인스턴스를 생성할 때 해당 클래스의 인스턴스를 인자로 전달해야한다. 그래야 override가 된다.

        ```java
        public class Member implements Comparable<Member> {
        	...
        	@Override
        	public int compare(Member member) {
        		return (this.memberId - member.memberId);
        	}
        }
        ```

## Map 인터페이스

---

- 쌍으로 관리하는데 필요한 메서드가 정의되어있으며, key-value쌍으로 이루어진 객체
- 특징
    - 검색속도가 빠르지만, 다른key값에 같은 index가 반환되는 충동일 발생하는 경우도 있다.
    - null key와 null value를 모두 허용합니다.
    - 내부적으로 데이터에 접근할 때 동기화를 보장하지 않습니다.
    - 데이터의 순서를 보장하지 않습니다.
    - 중복된 key값을 허용하진 않지만, 중복된 값은 갖을 수 있습니다.

### HashMap

---

- **put()**

`put()`은 인자로 key와 value를 받습니다. 전달된 인자는 HashMap에 key-value 관계로 저장이 됩니다.

`public V put(K key, V value)`

아래 코드의 HashMap은 key를 String으로, value를 Integer로 사용합니다. `put()`으로 데이터를 저장하였습니다. null은 key, value 모두 허용되며 중복된 key는 마지막에 저장된 값으로 업데이트됩니다.

```java
Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);
fruits.put(null, 4);
fruits.put("kiwi", 5);
System.out.println("fruits: " + fruits);

// fruits: {banana=2, null=4, apple=1, kiwi=5}
```

- **HashMap.putAll()**

`putAll()`은 인자로 전달된 Map에 대한 데이터를 모두 저장합니다.

`public void putAll(Map<? extends K, ? extends V> m)`

다음은 putAll로 두개의 Map을 합치는 예제입니다.

```java
Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);

Map<String, Integer> food = new HashMap<>();
food.put("coffee", 1);
food.put("hamburger", 2);
food.put("sandwich", 3);

food.putAll(fruits);
System.out.println("food: " + food);

// food: {banana=2, apple=1, kiwi=3, coffee=1, sandwich=3, hamburger=2}
```

- **get()**

`get()`은 인자로 전달된 key에 해당하는 value를 리턴해 줍니다. key가 존재하지 않으면 `null`을 리턴

`public V get(Object key) {`

```java
Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);

System.out.println("get(apple): " + fruits.get("apple"));
System.out.println("get(kiwi): " + fruits.get("kiwi"));
System.out.println("get(undefined): " + fruits.get("undefined"));

// get(apple): 1
// get(kiwi): 3
// get(undefined): null
```

- **remove()**

`remove()`는 인자로 전달된 key에 해당하는 데이터를 삭제. 삭제가 되면 value가 리턴됩니다. 존재하지 않는 데이터라면 `null`이 리턴

`public V remove(Object key)`

```java
Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);

System.out.println("remove(apple): " + fruits.remove("apple"));
System.out.println("remove(kiwi): " + fruits.remove("kiwi"));
System.out.println("remove(undefined): " + fruits.remove("undefined"));
System.out.println("fruits: " + fruits);

// remove(apple): 1
// remove(kiwi): 3
// remove(undefined): null
// fruits: {banana=2}`
```

- **clear(), isEmpty()**

`clear()`는 HashMap의 모든 데이터를 삭제합니다.

`public void clear()`

`isEmpty()`는 HashMap의 데이터가 비어있다면 `true`를 리턴하고 아니라면 `false`를 리턴합니다.

`public boolean isEmpty()`

```java
Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);
System.out.println("fruits: " + fruits);
System.out.println("is empty? " + fruits.isEmpty());

fruits.clear();
System.out.println("fruits: " + fruits);
System.out.println("is empty? " + fruits.isEmpty());

// fruits: {banana=2, apple=1, kiwi=3}
// is empty? false
// fruits: {}
// is empty? true
```

### **keySet(), values()**

`keySet()`은 HashMap에 저장된 key들을 Set 객체로 리턴

`public Set<K> keySet()`

`values()`는 HashMap에 저장된 value들을 Collection 객체로 리턴

`public Collection<V> values()`

다음은 key와 value들을 각각 출력하는 예제입니다.

```java
`Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);
System.out.println("keySet(): " + fruits.keySet());
System.out.println("values(): " + fruits.values());

Set<String> keys = fruits.keySet();
for (String key : keys) {
    System.out.println("key: " + key);
}

Collection<Integer> values = fruits.values();
for (Integer value : values) {
    System.out.println("value: " + value);
}

// keySet(): [banana, apple, kiwi]
// values(): [2, 1, 3]
// key: banana
// key: apple
// key: kiwi
// value: 2
// value: 1
// value: 3`
```

- **containsKey(), containsValue()**

`containsKey()`는 HashMap에 인자로 전달된 key가 존재하면 `true`를 리턴하고 그렇지 않으면 `false`를 리턴

`public boolean containsKey(Object key)`

`containsValue()`는 HashMap에 인자로 전달된 key가 존재하면 `true`를 리턴하고 그렇지 않으면 `false`를 리턴

`public boolean containsValue(Object value)`

```java

`Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);

System.out.println("containsKey(apple): " + fruits.containsKey("apple"));
System.out.println("containsKey(undefined): " + fruits.containsKey("undefined"));
System.out.println("containsValue(1): " + fruits.containsValue(1));
System.out.println("containsValue(0): " + fruits.containsValue(0));

// containsKey(apple): true
// containsKey(undefined): false
// containsValue(1): true
// containsValue(0): false`
```

- **replace()**

`replace()`는 인자로 전달된 key의 value를 인자로 전달된 value로 교체해 줍니다. 교체되어 삭제되는 value는 리턴됩니다. 존재하지 않는 key가 인자로 전달되면 `null`이 리턴

`public V replace(K key, V value)`

다음은 존재하는 key와 존재하지 않는 key에 대해서 replace를 시도하는 코드입니다.

```java
Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);
System.out.println("fruits: " + fruits);

System.out.println("replace(apple, 10): "  + fruits.replace("apple", 10));
System.out.println("replace(undefined, 10): "  + fruits.replace("undefined", 10));
System.out.println("fruits: " + fruits);

// fruits: {banana=2, apple=1, kiwi=3}
// replace(apple, 10): 1
// replace(undefined, 10): null
// fruits: {banana=2, apple=10, kiwi=3}
```

인자와 리턴 타입이 다른 `replace()` 메소드도 있습니다. 인자를 보시면 3개가 있습니다. 첫번째는 key, 두번째는 oldValue, 세번째는 newValue입니다. 저장된 key의 value가 oldValue와 동일할 때만 newValue로 변경해 줍니다. 교체가 되면 `true`를 리턴하며, oldValue와 동일하지 않으면 교체되지 않고 `false`가 리턴됩니다.

`public boolean replace(K key, V oldValue, V newValue)`

```java
Map<String, Integer> fruits = new HashMap<>();
fruits.put("apple", 1);
fruits.put("banana", 2);
fruits.put("kiwi", 3);
System.out.println("fruits: " + fruits);

System.out.println("replace(apple, 1, 10): "  + fruits.replace("apple", 1, 10));
System.out.println("replace(banana, 1, 10): "  + fruits.replace("banana", 1, 20));
System.out.println("fruits: " + fruits);

// fruits: {banana=2, apple=1, kiwi=3}
// replace(apple, 1, 10): true
// replace(banana, 1, 10): false
// fruits: {banana=2, apple=10, kiwi=3}`
```

### HashMap

---

HashMap과 동일한 내부구조를 가지고 있으나, 동기화된 메소드로 구성되어있다는 점이다. 멀티 스레드가 동시에 HashTable의 메소드들을 실행 할 수 없다. 멀티스레드 환경에서 안전하게 객체를 추가, 삭제하기위해서 HahsTable을 사용한다.