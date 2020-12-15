# 저장소 분류



## 저장소를 분석할 수 있는 몇 가지 차원

### 데이터 모델

- 구조적

  - SQL기반 DB관리 시스템과 마찬가지로 유연하고 동적인 쿼리에 적합함. 구조적 데이터 저장소의 예로는 브라우저의 IndexDB가 있음

- key / value

  - 고유한 키로 색인이 생성된 구조화되지않은 데이터를 저장 및 검색하는 기능을 제공함. 
  - 색인이 생성된 불투명한 데이터에 대한 액세스를 항상 허용한다는 점에서 해시 테이블과 비슷함

- 바이트 스트림

- 가변길이의 불투명한 바이트문자열로 저장하여 애플리케이션 계층에 내부조직의 형태를 유지함


### 지속성

- **세션 지속성:** 이 범주의 데이터는 단일 웹 세션 또는 단일 브라우저 탭이 활성 상태를 유지하는 동안만 보존됩니다. 세션 지속성이 있는 저장소 메커니즘의 예로는 Session Storage API가 있습니다.

- **기기 지속성:** 이 범주의 데이터는 특정 기기 내 여러 세션 및 여러 브라우저 탭/창에 걸쳐 보존됩니다. 기기 지속성이 있는 저장소 메커니즘의 예로는 Cache API가 있습니다.

- **전역 지속성:** 이 범주의 데이터는 여러 세션 및 여러 기기에 걸쳐 보존됩니다. 따라서 데이터 지속성 유형 중에 가장 강력합니다. 전역 지속성이 있는 저장소 메커니즘의 예로는 Google Cloud Storage가 있습니다.

  ​	

### 비교

|                                                              |               |        |                                                   |          |             |
| ------------------------------------------------------------ | ------------- | ------ | ------------------------------------------------- | -------- | ----------- |
| API                                                          | 데이터 모델   | 지속성 | 브라우저 지원                                     | 트랜잭션 | 동기/비동기 |
| [File system](https://developer.mozilla.org/en-US/docs/Web/API/FileSystem) | 바이트 스트림 | 기기   | [52%](http://caniuse.com/#feat=filesystem)        | 아니요   | 비동기      |
| [Local Storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) | 키/값         | 기기   | [93%](http://caniuse.com/#feat=namevalue-storage) | 아니요   | 동기        |
| [Session Storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) | 키/값         | 세션   | [93%](http://caniuse.com/#feat=namevalue-storage) | 아니요   | 동기        |
| [Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) | 구조적        | 기기   | 100%                                              | 아니요   | 동기        |
| [WebSQL](https://www.w3.org/TR/webdatabase/)                 | 구조적        | 기기   | [77%](http://caniuse.com/#feat=sql-storage)       | 예       | 비동기      |
| [Cache](https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage) | 키/값         | 기기   | [60%](http://caniuse.com/#feat=serviceworkers)    | 아니요   | 비동기      |
| [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) | 하이브리드    | 기기   | [83%](http://caniuse.com/#feat=indexeddb)         | 예       | 비동기      |
| [Cloud Storage](https://cloud.google.com/storage/?hl=ko)     | 바이트 스트림 | 전역   | 100%                                              | 아니요   | 모두        |



### 지원상세 현황

https://caniuse.com/#feat=indexeddb



### 저장 가능한 양은?

| 브라우저 | 제한                 |
| -------- | -------------------- |
| Chrome   | 여유 공간의 6% 미만  |
| Firebox  | 여유 공간의 10% 미만 |
| Safari   | 50MB 미만            |
| IE10     | 250MB 미만           |

## 

### 유지기간

브라우저에 영구적임



### 라이브러리

- https://dexie.org/





#### 참고

- [웹저장소개요](https://developers.google.com/web/fundamentals/instant-and-offline/web-storage/?hl=ko)
- [IndexedDb 기본개념](https://developer.mozilla.org/ko/docs/IndexedDB/Basic_Concepts_Behind_IndexedDB)
- [Progressive Web App용 오프라인 저장소](https://developers.google.com/web/fundamentals/instant-and-offline/web-storage/offline-for-pwa?hl=ko)
- [indexedDB 사용시 주의 사항](http://jodu.tistory.com/48)
- https://github.com/wonism/TIL/blob/master/front-end/javascript/client-storage.md#indexeddb





