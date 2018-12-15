## 서비스 워커란?

서비스 워커는 브라우저가 백그라운드에서 실행하는 스크립트로, 웹페이지와는 별개로 작동하며, 웹페이지 또는 사용자 상호작용이 필요하지 않은 기능에 대해 문호를 개방한다. 현재 [푸시 알림](https://developers.google.com/web/updates/2015/03/push-notifications-on-the-open-web?hl=ko) 및 [백그라운드 동기화](https://developers.google.com/web/updates/2015/12/background-sync?hl=ko)와 같은 기능은 이미 제공되고 있다. 

이것은 오프라인 환경을 완벽히 통제할 수 있는 권한을 개발자에게 부여한다.

서비스 워커 이전에는 웹에서 사용자에게 오프라인 경험을 지원하는 [AppCache](https://www.html5rocks.com/en/tutorials/appcache/beginner/)라는 API가 있었다. AppCache의 주요 문제는 실제로 존재하는 [문제의 수](http://alistapart.com/article/application-cache-is-a-douchebag)와 디자인이 단일 페이지 웹 앱에는 특히 잘 작동하지만 여러 페이지로 구성된 사이트에는 그다지 훌륭하게 작동하지 않는다는 사실이다. 서비스 워커는 이러한 일반적인 문제점을 피하도록 설계되었습니다.

다음은 서비스 워커와 관련된 유의 사항입니다.

- 서비스 워커는 [자바스크립트 Worker](https://www.html5rocks.com/en/tutorials/workers/basics/)이므로 DOM에 직접 액세스할 수 없다. 대신에 서비스 워커는 [postMessage](https://html.spec.whatwg.org/multipage/workers.html#dom-worker-postmessage) 인터페이스를 통해 전달된 메시지에 응답하는 방식으로 제어 대상 페이지와 통신할 수 있으며, 특정 페이지는 필요한 경우 DOM을 조작할 수 있다.
- 서비스 워커는 프로그래밍 가능한 네트워크 프록시이며, 페이지의 네트워크 요청 처리 방법을 제어할수 있다.
- 서비스 워커는 사용하지 않을 때는 종료되고 다음에 필요할 때 다시 시작되므로 서비스 워커의 `onfetch` 및 `onmessage` 핸들러의 전역 상태에 의존할 수 없다. 보관했다가 다시 시작할 때 재사용해야 하는 정보가 있는 경우 서비스 워커가 [IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)에 대한 액세스 권한을 가진다.

## 서비스 워커 수명 주기

서비스 워커의 수명 주기는 웹페이지와 완전히 별개입니다.

서비스 워커를 사이트에 설치하려면 페이지에서 자바스크립트를 이용하여 등록해야 합니다. 서비스 워커를 등록하면 브라우저가 백그라운드에서 서비스 워커 설치 단계를 시작합니다.

일반적으로 설치 단계 동안 정적 자산을 캐시하고자 할 것입니다. 모든 파일이 성공적으로 캐시되면 서비스 워커가 설치됩니다. 파일 다운로드 및 캐시에 실패하면 설치 단계가 실패하고 서비스 워커가 활성화되지 않습니다(즉, 설치되지 않음). 이런 상황이 발생하더라도 걱정하지 마세요. 다음에 다시 시도할 것입니다. 그러나 이는 설치가 이루어지면 정적 자산이 캐시됨을 의미합니다.

활성화 단계 후에 서비스 워커는 해당 범위 안의 모든 페이지를 제어하지만 서비스 워커를 처음으로 등록한 페이지는 다시 로드해야 제어할 수 있습니다. 서비스 워커에 제어 권한이 부여된 경우 서비스 워커는 메모리를 절약하기 위해 종료되거나, 페이지에서 네트워크 요청이나 메시지가 생성될 때 fetch 및 message 이벤트를 처리합니다.

## 사전 요구사항

### 브라우저 지원

브라우저 옵션은 성장하고 있습니다. Firefox와 Opera가 서비스 워커를 지원합니다. Microsoft Edge는 현재 [공적 지원을 표명](https://developer.microsoft.com/en-us/microsoft-edge/platform/status/serviceworker/)하고 있습니다. Safari도 [향후 개발 예정](https://trac.webkit.org/wiki/FiveYearPlanFall2015)임을 밝혔습니다. Jake Archibald의 [is Serviceworker ready](https://jakearchibald.github.io/isserviceworkerready/) 사이트에서 모든 브라우저의 진행 상황을 확인할 수 있습니다.

### HTTPS 필요

개발 중에 `localhost`를 통해 서비스 워커를 사용할 수 있지만 사이트에 배포하려면 서버에 HTTPS 설정을 해야 합니다.

서비스 워커를 사용하여 연결을 가로채고 조작하고 응답을 필터링할 수 있습니다. 강력한 기능입니다. 해당 기능을 유익하게 사용하고 싶겠지만 중간자는 그렇지 않을 수 있습니다. 이를 피하기 위해 HTTPS로 제공되는 페이지에만 서비스 워커를 등록할 수 있습니다. 따라서 브라우저가 수신하는 서비스 워커는 네트워크 통신 중에 변조되지 않습니다.



## 참고 

- Google Web

