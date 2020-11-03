# 14장 - 웹 스크래핑

날짜: Nov 2, 2020

# 웹 스크래핑

소프트웨어 기술을 활용해 웹 사이트 내에 있는 정보를 추출하는것.

파이썬에는 웹상에 존재하는 데이터를 가져오거나 처리하기 위한  다양한 패키지들이 존재하여 비교적 쉽게 웹 스크래핑을 위한 코드를 작성할 수 있다.

### 라이브러리

- [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
    - response 받아온 데이터에서  원하는 정보를 추출함
    - SPA웹인 경우 response가 온전하지 못하기 때문에 크롤링하기 까다로움
- [scrapy](https://scrapy.org/)
    - 크롤링을 위하여 개발된 라이브러리임
    - 미들웨어, 파이프라인,  js render 지원, xpath등 다양한 기능을  지원한다.
- [selenium](https://www.selenium.dev/documentation/en/)
    - 웹 드라이버를 사용해 실제 유저가 행동하는듯한 행위를  자동화 할 수 있는 라이브러리
        - 그 때문에 다른 라이브러리에 비해 속도가 느리다
            - 멀티프로세스를 사용하여 좀 개선할 수는 있음
    - 실제로 과정을 바로 확인 할 수가 있어서 디버깅이 직관적이다.

간단한 개발같은 경우는 Beautiful soup, SPA대응하면서 자동화를 위하는 기능은 selenium, 빠르고 대용량 데이터 크롤링을 위하서는 scrapy를 사용할 듯함