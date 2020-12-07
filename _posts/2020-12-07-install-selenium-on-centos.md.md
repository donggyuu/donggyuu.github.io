---
title: "CentOS에서 Selenium설정하고 실행하기"
excerpt: "CentOS 7에서 Selenium 설정 및 발생 가능 에러 정리"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/like-click_bot_2.png
categories:
  - Development 
tags:
  - Utilities
  - Setup
  - Linux
last_modified_at: 2020-12-07T08:06:00-05:00
published: false
---
## 개요
[이전 포스팅](https://donggyuu.github.io/development/like-click-bot)에서는 Local에서 셀레니움을 이용해 간단한 봇을 만들어보았습니다. 봇을 서버에 올려 정해진 시간에 동작하도록 하면 수동으로 실행하는 것보다 간단하겠죠.   
본 포스팅에서는 CentOS7서버에서 셀레니움 봇을 동작하도록 하는 설정을 하겠습니다.

## Chrome headless
리눅스 상에서 동작할 것이므로 브라우저의 화면이 보일 필요가 없습니다. Chrome headless는 브라우저 화면의 표시 없이 브라우저를 실행하도록 해줍니다.   
headless의 설정을 위해 아래의 코드를 추가했습니다.  
[코드는 여기서 확인](https://github.com/donggyuu/like-click-bot/blob/master/like-click-bot.py#L70)     
```python
options = webdriver.ChromeOptions()
# 브라우저 창 안 띄우겠다
options.add_argument('headless')
# 보통의 FHD화면을 가정
options.add_argument('window-size=1920x1080')
# gpu를 사용하지 않도록 설정
options.add_argument("disable-gpu")
# headless탐지 방지를 위해 UA를 임의로 설정
options.add_argument("user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6)")
```
특정 서비스는 접속 로그의 UA를 보고 사람인지 로봇인지를 판단합니다. 따라서 로봇으로 인식되지 않도록 임의의 UA도 설정해 주었습니다. 
