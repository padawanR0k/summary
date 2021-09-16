## 7. 레코드 캡슐화하기 p.236

- 가변데이터 -> 객체
- 불변데이터 -> 시작과 끝과 길이를 구해 레코드에 저장

### 7.1 간단한 레코드 캡슐화

```javascript
const organization = {
  name: "애크미 구스베리",
  country: "GB",
};

// 기본
const result1 = `<h1>${organization.name}</h1>`;
```

```javascript
const organization = {
  name: "애크미 구스베리",
  country: "GB",
};

class Organization {
  constructor(data) {
    this._name = data.name;
    this._country = data.country;
  }

  get name() {
    return this._name;
  }
  get country() {
    return this._country;
  }
  set name(str) {
    this._name = str;
  }
  set country(str) {
    this._country = str;
  }
}
const organ1 = new Organization(...organization);
const result2 = `<h1>${organ1.name}</h1>`;
```

- 레코드를 참조하다가 캡슐화를 깰만한 요소가 있을때 좋음

### 중첩된 레코드 캡슐화

```javascript
const custormerData = {
  1920: {
    name: "마틴 파울러",
    id: 1920,
    usages: {
      2016: {
        1: 50,
        2: 55,
      },
      2015: {
        1: 70,
        2: 63,
      },
    },
  },
};
function compareUsage(custormerId, laterYear, month) {
  const later = custormerData[custormerId].usages[laterYear][month];
  const ealier = custormerData[custormerId].usages[laterYear - 1][month];
  return {
    laterAmount: later,
    change: ealier - later,
  };
}
console.log(compareUsage(1920, 2016, 2));
```

> 데이터의 뎁쓰가 깊어 구조 안으로 계속 파고 들어가야함

- 해결방법
  - 값을 읽는 코드를 독립 함수로 추출하기
    - 고객 정보, 연도부분을 단위로 나뉘어 할 수 있을듯
  - 레코드 캡슐화를 재귀적으로 하기
    - 고객 정보 레코드를 클래스로 변경하고, 게터 세터를 혼합하여 함부로 원본데이터에 접근해 갱신할 수 없게함

### 7.2 컬렉션 캡슐화하기 p.46

가변 데이터를 캡슐화 할 때 배열같은 컬렉션은 set할 때 주의해야함. 값 자체르 변경해버리면 문제가 생길수 도 있음
그래서 setter부분을 따로 메서드로 만들어 컬렉션 값자체를 바꾸지 못하게하거나, 사본을 반환하고 바꾸게하거나, 프록시 방식을 쓴다. (중요! 이 중 하나를 택했으면 혼란을 주지 않도록 일관성있게 사용해야함)

```javascript
class Person1 {
  constructor(data) {
    this._age = data.age;
    this._list = data.list;
  }

  getList() {
    return this._list;
  }

  setList(list) {
    this._list = list;
  }
}
const p1 = new Person1({ age: 20, list: [1, 10, 1000] });
p1.setList([1, 10, 1000, 10000]);

class Person2 {
  constructor(data) {
    this._age = data.age;
    this._list = data.list;
  }

  getList() {
    return this._list;
  }

  addItem(item) {
    this._list.push();
  }

  removeItem(id) {
    this._list = this._list.filter((item) => item.id !== id);
  }
}

const p2 = new Person1({ age: 20, list: [1, 10, 1000] });
p2.addItem(10000);
```

### 7.3 기본형을 객체로 바꾸기

서비스를 운영하다보면 초기에 설정한 타입보다 다양한 타입이 추가되고, 타입에 대한 우선순위도 다양해진다. 이부분들을 값으로 그냥 두게되면 코드의 가독성은 나빠진다.

```javascript
const customers = [
  {
    id: 0,
    priority: "super",
  },
  {
    id: 1,
    priority: "high",
  },
  {
    id: 2,
    priority: "middle",
  },
  {
    id: 3,
    priority: "row",
  },
];
// vip의 조건이 다양해지게되면 이 부분이 복잡해진다.
const vip = customers.filer(
  (a) => a.priority === "high" || a.priority === "super"
);

const priorities = ["super", "high", "middle", "row"];
class Priority {
  constructor(value) {
    this.value = value;
  }

  get index() {
    priorities.findIndex((item) => item === this.value);
  }

  higherThan(priority) {
    return this.index >= priority.index;
  }
}

const vipPriority = new Priority("high");
customers.filer((a) => new Priority(a.priority).higherThan(vipPriority));
```

### 7.4 임시 변수를 질의 함수로 바꾸기
어떤 연산을 통해 만들어지는 임시변수가 중복되거나 그럴 가능성이 있으면 질의 함수로 빼내자
```javascript
class Price {
	constructor({basePrice, sale}) {
		this.basePrice = basePrice;
		this.sale = sale;
	}

	get price() {
		return this.discounted;
	}

	get membershipPrice(member) {
		return member.priority * this.discounted;
	}

	get discounted() {
		return this.basePrice * this.sale
	}
}
```

### 7.5 클래스로 추출하기
클래스는 명확하게 추상화하고 주어진 역할의 이상을 처리하면 안된다. 서비스를 운영하다보면 클래스는 비대해지게된다. 내부에서 데이터와 메서드를 묶을 수 있을정도가 되었다면 분리할 때가 왔다는것이다. 그런 부분들을 다른 클래스로 분리하자

### 7.6 클래스 인라인하기
클래스의 남은 역할이 거의 없어진 경우, 7.5와 반대로 다른 클래스에 해당 클래스가 하던 역할을 추가하자

### 7.7 위임 숨기기
제대로된 모듈화 설계를 위해 캡슐화를 하자. 위윔 객체 필드를 숨기자
```javascript
class Department { ... }
class Person {
	constructur(name) {
		this._name = name;
	}
	get name() { return this.name }
	get department() { return this._department; }
	set department(arg) { this._department = arg; }
	// 추가
	get manager() { return this._department.manager; }
}

// 기존
manager = aPerson.department.manager
// 이후 (department가 manager정보를 제공한다는 사실을 몰라도 됨)
manager = aPerson.manager
```

### 7.8 중개자 제거하기
7.7 의 반대. 위임을 숨기게되는 경우 위임객체의 또 다른 기능을 쓰려고 할 때마다 위임메서드를 계속 추가해야한다. 그러다보면 단순히 전달만하는 역할을 하는 메서드가 많아진다. 이럴땐 위임 객체를 직접 호출하는 것이 나을 수 도 있다.