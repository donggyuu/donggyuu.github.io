---
title: "Selenium으로 네이버 블로그 좋아요 클릭봇 만들기"
excerpt: "Python의 Selenium으로 좋아요 클릭 자동화 하는 법 및 코드 설명"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/like-click_bot_2.png
categories:
  - Development 
tags:
  - Python
last_modified_at: 2020-12-06T08:06:00-05:00
---

# 다루는 내용  
자동으로 이웃블로그의 포스팅에 좋아요를 눌러주는 봇을 만들어보기로 하였다.  
<br>
봇은 아래와 같은 동작을 한다.  
"네이버 로그인"→"원하는 키워드로 최신 포스팅 검색"→"최신 포스팅에 좋아요 클릭"  
  
사진으로 보면 아래와 같다.  
![like-click_bot_2](/assets/images/like-click_bot_2.png)   

또한 Mac환경에서 동작확인을 하였다.

<br>

# 사용 방법
전체 코드를 [여기](https://github.com/donggyuu/like-click-bot)서 클론한다.  

실행 전에 config.yml를 편집하여 본인의 네이버 아이디와 비밀번호, 그리고 검색을 원하는 키워드를 넣어준다.
```bash
# account
id: your_id
pw: your_password

# search-option
keyword: words_you_want_to_find
```

이후 python스크립트를 실행해준다.
```bash
# Your download path : ~/~/like-click-bot
$ python like-click-bot.py
# ---------------------------------------
# Selenium will be executed
```

간혹 웹드라이버의 버전이 맞지 않아 실행되지 않을 수도 있다. 본 스크립트는 크롬 브라우저 87.0.4280.88 버전에 맞는 웹드라이버를 사용하고 있다. 버전이 맞지 않으면 [여기](https://sites.google.com/a/chromium.org/chromedriver/downloads)에서 올바른 버전을 받으면 된다.  

<br>

# 코드 설명 
기본적인 Selenium이기때문에 크게 어려운 부분은 없지만 몇가지 주의점이 있다.

```python
driver.get(search_keywords)
time.sleep(2)

# 이미지가 많으면 페이지 로딩시간이 길 수 있으니 5초 정도 여유를 줌
driver.find_element_by_class_name("api_txt_lines").click()
time.sleep(5)

# move to current page
# 새롭게 연 블로그 페이지로 포인터 이동
driver.switch_to.window(driver.window_handles[-1])
time.sleep(1)
```
위의 코드는 키워드로 최신 블로그 글을 검색 후 첫 번째 글을 새창에서 여는 코드이다. **switch_to.window를 하지 않으면 Selenium은 새창이 아닌 기존의 "최신 블로그 글을 검색한 페이지"에 포커스를 둔다.** 새창으로 열린 글의 좋아요를 클릭하는 것이 목적이므로 처리해주어야 한다.


또한 새창으로 열린 블로그 글은 HTML의 body가 iframe으로 둘러싸여 있다(왜지...).  
![like-click_bot](/assets/images/like-click_bot.png)  

**이상태로 Selenium은 iframe안의 요소들을 찾을 수 없다.** 따라서 아래처럼 body안의 iframe에서 찾겠다고 명시해주어야 한다.

```python
# 네이버 블로그는 전체적으로 iframe으로 감싸져 있기에 처리해줌
mainFrame= driver.find_element_by_id("mainFrame")
```

→ 참고한 
[stackoverflow](https://stackoverflow.com/questions/52751923/unable-to-locate-element-within-an-iframe-through-selenium/52752249)  
