
# FRONT-END

## Web 기술의 설정값

- HTML
>구조설계
- css
>꾸밈
- Javascript
>만들어진것에 숨을 불어넣음

## 웹표준 Web Standards
- 싸이월드와 페이스북의 차이
- 권고안 일뿐.
- 책추천: 제프리 젤드만의 웹표준 가이드

## 웹접근성 Web Accessibility
- 웹의 힘은 보편성에 있다.
- 모든 사람을 위해 웹에 접근할 방법을 제공해주는 것.

### 장애에 대한 이해
> 어려운게 아니라 익숙하지 않은것이다.
> 틀린게 아니라 다를뿐이다.
- 시각 장애 - 전맹, 저시력
- 청각 장애
- 지체 장애 - 절단 및 지체기능 장애
- 뇌병변 장애

## 웹호환성
- 다양한 플랫폼
- 크로스 브라우징
- 검색최적화 (SEO); 접근성 + 표준성 
- 저사양 또는 저속회선 (우리나라는 고속회선 강국일뿐이다.)


# HTML
- Markup (언제나 논리적으로 마크업해야함)
- API

## HTML의 탄생
- HTML4는 너무 느슨한 규칙을 가지고있어서 개발자마다 코딩스타일이 달랐다. 그러다보니 나온게 XML
- XHTML은 규칙이 엄격한 XML을 HTML에 적용함.

xml
```xml
    <fastcampus>
        <teacher>
        </teacher> 
    </fastcampus>
```

html4
```html
    <HTML>
        <p>
        <p> 
    </HTML>
```
> html4에서는 태그를 대문자,소문자를 마음대로 써도되고 어떤 태그들은 닫는태그를 안써도 됬엇다.

- 과거에는 html 태그에 DTD 명시를 해야만 그걸 토대로 태그들이 유효한가를 판단했다.


--- 

## HTML5의 서식
- HMTL5는 HTML4.01이나 XHTML1.0 문법을 모두 허용하기 떄문에 기존에 사용하던 마크업 문법을 그대로 사용 수 있음

- 허용되는 범위에서 무었을 택하느냐는 자신에게 달렸다.

- 자신만의 코딩컨벤션을 만들어라
 
 ### 종료태그의 처리
- HTML5는 종료 태그를 생략할 수도 있음. 그러나 모든 종료 태그를 생략해도 되는 것은 아니기 떄문에 종료태그가 생략할 수 있는 요소를 사전에 확인해야함 
- 그러나 HTML5에서 종료태그를 생략하는 것이가능하다고 하더라도 기존 XHTML1.0의 규칙처럼 시작과 종료태그를 정확히 명시하여 정형식(Well-Formed) 구조로 마크업할 것을 권장함.


```html
    <p><img src="images/back.gif" alt="뒤로"></p>  // o
    <p><img src="images/back.gif" alt="뒤로" /></p>  // o
```

### 대소문자의 사용
- HTML5 시작 및 종료 태그는 물론 속성에 대문자 또는 소문자를 사용할 수도 있음. 그러나 기존 XTML1.0 규칙처럼 소문자를 사용할 것을 권장함
```html
    <p><img src="images/back.gif" alt="뒤로"></p>  // o
    <P><img src="images/back.gif" ALT="뒤로" />></P>  // o
```

### 빈(Empty) 요소

- HTML에서 &lt;meta&gt;, &lt;link&gt;, &lt;img&gt;, &lt;br&gt; ,&lt;input&gt; 등 종료 태그를 가지고 있지 않은 요소를 빈요소라고 하는데, 기존 HTML4.01에서는 <img> 형식으로, XHTML1.0에서는 <img /> 형식으로 선언해야  하며, html5에서는 두가지 방식 모두 허용하고있음
 ```html
    <p><img src="images/back.gif" alt="뒤로"></p>  // o
    <p><img src="images/back.gif" alt="뒤로" /></p>  // o
```
### attribute 와 Value

- 논리 속성의 경우 속성값을 지정,생략 할수있음 
```html
    <select mutiple></select> // o
    <select mutiple="multiple"></select> // o
```
### 잘못된 중첩사용 불가
- 시작태그와 종료태그의 중첩에 문제가 발생하지 않도록 해야 함
``` html
<p><strong>text</strong></p> // o
<p><strong>text</p></strong> // x
```
---
### 문자참조 
- "&lt;", "&gt;", "&amp;" 등과 같은 특수문자는 그대로 치지말고 Characters Entity Code로 변환 하여 마크업해야함
- 특수문자를 엔티티코드로 표시해놓은 사이트 https://dev.w3.org/html5/html-author/charref

## 에디터를 켜기전에 고민을 하자
1. 구조
2. 의미에 맞는 마크업 (Semantic Markup)  
3. 접근성 
4. 네이밍


### 네이밍 
- MainContent  (파스칼케이스)
- mainContent  (카멜케이스)
- main-content  (케밥케이스)
- main_content 