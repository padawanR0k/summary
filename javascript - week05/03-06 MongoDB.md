

# 1. Introduction

mongoDB는 SQL로 데이터에 접근하지않고 메소드로 접근한다.

Document는 RDMS의 record와 유사한 개념으로 JSON objects 형태의 key-value의 쌍으로 이루어진 데이터 구조로 구성된다..

![MongoDB Document](http://poiemaweb.com/img/mongodb-document.png)

```code
{
  _id: ObjectId("5099803df3f4948bd2f98391"),
  name: { first: "Alan", last: "Turing" },
  birth: new Date('Jun 23, 1912'),
  death: new Date('Jun 07, 1954'),
  contribs: [ "Turing machine", "Turing test", "Turingery" ],
  views : NumberLong(1250000)
}
```

![MongoDB Structure](http://poiemaweb.com/img/mongodb-structure.png)



# 2. RDMS와 MongoDB의 비교
예전에는 RDB - 집합의 개념을 구현한것 

NoSQL 형태의 MongoDB

- RDB만큼의 성능을 못냄



File I/O 

> 컴퓨터가 가장 힘들어하는 작업( 파일을 열고 그 파일을 읽어들이는 것 )
>
> 어떤 두 유저가 동일한 데이터 테이블에 접근하려고 할때 DB는 충돌을 피하게 해줘야한다.



분산되어 있는 서버에서 여러에서 동시에 데이터를 가져와야할때 mongoDB는 RDB보다 성능이 떨어진다. 

