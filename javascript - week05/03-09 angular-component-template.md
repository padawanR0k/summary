# 1. 템플릿(Template)이란?



![template](http://poiemaweb.com/img/template.png)

Component Class의 동적데이터와 template의 정적데이터가 컴파일되어 view가 나온다.

Component class 는 언제든지 변할 가능성이 있다. => template을 변화 시키기위하여 존재한다. ( 꼭 그렇지만은 않다)

Angular는 **템플릿**과 **컴포넌트 클래스**로 뷰와 모델(데이터와 비즈니스 로직)을 분리한다.



![mvc-mvvm](http://poiemaweb.com/img/mvc-mvvm.png)



#### Model

애플리케이션에서 사용되는 데이터의 형식, 데이터를 컨트롤하는 로직의 모음



#### View

사용자에게 모델(데이터)을 표시



#### Controller

모델과 뷰의 상호 작용을 감시하고 업데이트



#### ViewModel

MVC 패턴에서는 **컨트롤러**가 *모델*과 *뷰* 간의 상호 작용을 담당하였지만 MVVM 패턴에서는 해당 뷰가 데이터 바인딩을 통해 컨트롤러의 역할



## 1.2 Component의 흐름

![angular-view-model](http://poiemaweb.com/img/angular-view-model.png)

DOM의 상태변화를 View가 먼저받는다. 그 변화를 Model에게 이벤트 바인딩으로 보낸다. 

모델에서 로직으로 가공된 데이터를 다시 View에게 보낸다. View는 DOM을 새로운 데이터로 업데이트시킨다.

> Angular는 변화 감지(Change detection) 메커니즘 위에서 동작하는 데이터 바인딩(Data binding)을 통해 템플릿과 컴포넌트 클래스를 긴밀히 연결하고 동기화를 유지한다.



![databinding & change detection](http://poiemaweb.com/img/databinding-changedetection.png)



변화감지가 쉽게 이루어지기 위해서는 데이터의 address가 변화해야한다. 되도록이면 재할당을 하는 코딩을 해야 프레임워크가 하는일이 줄어든다.