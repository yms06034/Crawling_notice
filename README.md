# Crawling_notice

# 크롤링의 자주 나는 오류

### 사이트 로딩 기다리는 필요성

- 사이트의 원하는 데이터, 버튼이 존재하지 않을 수 있다
    - selenium은 기본적인 사이트 로딩을 기다리는데
    HTTP Response 및 Rendering을 기다림
- 그렇기에 사이트 로딩이 필요함 (Expected Conditions)
- **selenium 기다리는 각각의 함수들 (알아두면 너무 좋음)**
    - title_is
    - title_contains
    - presence_of_element_located
    - visibility_of_element_located → O
    - visibility_of
    - presence_of_all_elements_located
    - text_to_be_present_in_element → O
    - text_to_be_present_in_element_value
    - frame_to_be_available_and_switch_to_it  → O
    - invisibility_of_element_located
    - element_to_be_clickable
    - staleness_of
    - element_to_be_selected
    - element_located_to_be_selected
    - element_selection_state_to_be
    - element_located_selection_state_to_be
    - alert_is_present → 말 그대로 alert를 기다리는 함수
    
- presence → 있으면 반환
- visibility → 보이면 반환
- text → 값이 있으면 반환
- frame → 웹 사이트에 frame을 기다릴 때 사용 (있으면 switch)

### selenium 아닌 척 하기

- 웹 서버들이 자동화된 봇을 막을 수 있음
- Stealth 라이브러리를 사용하면 가능함

[Making Chrome Headless Undetectable](https://intoli.com/blog/making-chrome-headless-undetectable/)

[selenium-stealth](https://pypi.org/project/selenium-stealth/)

```python
pip install selenium-stealth

from selenium import webdriver
from selenium_stealth import stealth
import time

browser = webdriver.Chorme(PATH)

stealth(browser,
		languages=["ko-KR", "ko"],
    vendor="Google Inc.",
    platform="Win32",
    webgl_vendor="Intel Inc.",
    renderer="Intel Iris OpenGL Engine",
    fix_hairline=True,
)

url = "https://intoli.com/blog/not-possible-to-block-chrome-headless/chrome-headless-test.html"

browser.get(url)

time.sheep(3)

browser.quit()
```

https://intoli.com/blog/not-possible-to-block-chrome-headless/chrome-headless-test.html

- 여기서 PC에서 직접 접근 한 것과 selenium으로 접근 한 것을 확인할 수 있음

### 과연 진짜 크롤링이란 뭘까?

- 과연 진짜 크롤러라고 하는건 뭘까?
단순히 bs4, selenium, request를 써서 웹 상의 데이터를 가져오는 것이 크롤링이라고 할 수 있을까?
내 생각에는 절대 그렇지 않다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/185fd82c-eecd-4959-99a0-4e43711e219a/Untitled.png)

- 위 이미지의 아키텍쳐 구조만 보더라도 **Fetcher, Parser, Frontier**가 보인다
    - **Frontier**가 탐색할 URL를 **Fetcher**에게 넘기면 해당 DOM을 **Parser**에게 넘긴다
    - 그 이후 Dup URL Elim에서 이미 방문한 URL과 중복 URL들을 제거한 채로 **Frontier**에게 전달
        - 이럴 경우 중복방지를 진행할 수 있게 된다
    - 그렇게 따지면 이전에 배웠던 DFS, BFS와 동일한 로직이 완성된다
        - 매일마다 최신화 되고 있는 웹상에서 어떻게 해야지만 최적화를 진행할 수 있을까?
    
- 구글에 크롤링에 대해 조금만 검색해보더라도 실제 크롤러라고 불리우는 사람들은
DNS서버 까지 따로 두기도 한다고 한다
- https://blog.hubspot.com/website/what-is-dns-server // What is a DNS server?
    
    [Web_Crawler_A_Review.pdf](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f556b716-184e-4a58-83d9-a8051b4ef7d1/Web_Crawler_A_Review.pdf)
    
- https://www.youtube.com/watch?v=i5qLt0ShJSg
- https://www.microsoft.com/en-us/research/wp-content/uploads/2009/09/EDS-WebCrawlerArchitecture.pdf
