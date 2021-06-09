---
title: "쿠팡 저품질 피하기"
excerpt: "CentOS 7에서 Selenium 설정 및 발생 가능 에러 정리"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/install-selenium-on-centos.png
categories:
  - Development 
tags:
  - Development-Skills
last_modified_at: 2020-12-07T08:06:00-05:00
published: false
---

## title : 가성비 마우스 마이크로소프트 Mobile Mouse 1850, 그리고 단점 하나


## 
가성비 마우스 추천
벌써 3번째임
처음엔 검은색 정도였는데 어느새 파란색도

블루투스라면 380m
그냥 usb무선이라면 마소 1980마우스

Wireless Mobile Mouse 1850

무선 브랜드로만 10개 이상 써본것 같고
비싼것도 많이 써봤는데
1~2만원대 
M221
M350
M337

마소
Mobile Mouse 3500
Wireless Mobile Mouse 1850

그중 이게 제일 낫다 싶은거 하나 .
Wireless Mobile Mouse 1850



## 마소 마우스 장점
싸다. 만원 내외에 브랜드 마우스임
로지텍 써봤는데 안좋은점은?
로지텍 -> 이거 빛이 안들어온다.

## 굳이 블루투스 안쓰는 이유??
윈도우 쓰면 윈도우 재설치하는 경우가 있다.
초반에는 블루투스도 드라이버 설치 전까지는 안잡힌다.
그 전에 세팅을 하는데 마우스가 필요.
그래서 윈도우 쓰는 사람은 블루투스 아닌 무선 마우스나
유선 마우스 하나 저렴한거 장만해두면 언젠가는 유용하게 쓴다.

그리고 리눅스 쓰는 사람도 초반에 블루투스 이런거
내가 알아서 잡아야 하는 경우가 많다.

로지텍은 그리고 빛 나오는 부분? 이게 안보인다.
어떤 원리인지는 잘, 빛만 안나오는데...
가끔 그래서 이게 움직이고 있는건지 헷갈릴떄가 있다.
난 안쓸때 명확히 꺼두길 바라는데, 일일이 버튼을 확인해야 함.

"사진의 이 부분. 마소는 불 들어오고 로지텍은 안들어옴"

일반적인 바닥에서는 다 잘 작동된다.

## 단점,, 이게 치명적인 단점
이게 위치에 따라 애매하다
그러니까 수신에 방해받는 것이 있다면 잘 안되는 수도 있다.

책상에 올려두고 쓰거나, 나와 마우스 사이에 장애물이 없는 것을 추천한다.
장애묾 - 전자기기 

"그림 그리기" - 본체 뒤에 동글을 꽂는 경우...






## 개요
[이전 포스팅](https://donggyuu.github.io/development/like-click-bot)에서는 Local에서 셀레니움을 이용해 간단한 봇을 만들어보았습니다. 봇을 서버에 올려 정해진 시간에 동작하도록 하면 수동으로 실행하는 것보다 간단하겠죠.   
본 포스팅에서는 CentOS7서버에서 셀레니움 봇을 동작하도록 하는 설정을 하겠습니다.

## Chrome headless
리눅스 상에서 동작할 것이므로 브라우저의 화면이 보일 필요가 없습니다. Chrome headless는 브라우저 화면의 표시 없이 브라우저를 실행하도록 해줍니다.   
headless의 설정을 위해 [아래의 코드](https://github.com/donggyuu/like-click-bot/blob/master/like-click-bot.py#L70)를 추가했습니다.   
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

## CentOS 설정
미리 준비해둔 CentOS서버에 접속합니다. 저는 Lightsail를 사용하므로 Lightsail에 접속하겠습니다.
```bash
# 권한으로 접속이 안되면 sudo를 붙여본다
ssh -i /Users/don/aws_keypair_seoul.pem centos@xx.xx.xx.xx
```

CentOS7에 python2는 이미 설치되어 있지만 대세를 따라  python3을 사용하겠습니다.  
python3의 설치는 yum을 사용하는 방법, 직접 다운로드하는 방법이 있습니다. 개인적으로 패키지 관리툴의 사용을 선호하므로 yum으로 설치하겠습니다([이곳](https://www.liquidweb.com/kb/how-to-install-python-3-on-centos-7/)을 참고하였습니다). 다만 현 시점에서 [yum은 python3.6까지만 지원](https://stackoverflow.com/questions/50408941/recommended-way-to-install-pip3-on-centos7)한다는 단점이 있습니다.   

```bash
# python2 is already installed on CentOS7
$ python --version
-----------------------
Python 2.7.5

# install python3
sudo yum update -y
sudo yum install -y python3

# confirmation
$ python3 --version
-----------------------
Python 3.6.8
```

[pip은 python3.6부터는 같이 포함](https://linuxize.com/post/how-to-install-pip-on-centos-7)되어있기 때문에 따로 설치하지 않아도 됩니다. 
pip을 사용하여 Selenium을 설치합니다. 명령어 "python3.6 -m pip"을 사용하겠습니다. 

```bash
sudo python3.6 -m pip install selenium
--------------------------------------------
Successfully installed selenium-3.141.0 urllib3-1.26.2
```
[이전 포스팅](https://donggyuu.github.io/development/like-click-bot)에서 만든 봇은 yml에서 설정값을 읽어옵니다. 따라서 yaml관련 라이브러리도 설치해줍니다. 
```bash
sudo python3.6 -m pip install pyyaml
--------------------------------------------
Successfully installed pyyaml-5.3.1
```

이제 봇을 위치시킬 경로를 만들어줍니다.
```bash
mkdir -pv /home/centos/app/like-click-bot
--------------------------------------------
mkdir: created directory ‘/home/centos/app/like-click-bot’
```

## Selenium bot 실행
로컬에 다운로드해둔 봇을 CentOS7서버로 업로드합니다(아래는 Lightsail을 사용하는 경우의 scp 명령어).   
```bash
sudo scp -i /Users/don/aws_keypair_seoul.pem -r /Users/don/like-click-bot centos@xx.xx.xx.x:/home/centos/app/
```

CentOS에서 봇이 잘 업로드 되었음을 확인합니다.  
```bash
# file check
$ pwd; ll
/home/centos/app/like-click-bot
total 16332
-rwxr-xr-x. 1 centos centos 14057472 Dec  7 09:07 chromedriver
-rw-r--r--. 1 centos centos       76 Dec  7 09:06 config.yml
-rw-r--r--. 1 centos centos     3129 Dec  7 09:06 like-click-bot.py
-rw-r--r--. 1 centos centos      135 Dec  7 09:06 README.md
```

최종적으로 Selenium을 사용한 봇을 실행시켜봅니다. 
```bash
python3 like-click-bot.py
```

## 에러 핸들링  
진행하다보면 2가지 에러가 나올 수 있습니다. 아래의 조치를 취한 후 재실행해 봅시다.  

### case_1
```bash
... 생략 ...
OSError: [Errno 8] Exec format error
```
[위의 에러](https://stackoverflow.com/questions/38833589/oserror-errno-8-exec-format-error-selenium)는 운영체제와 다른 chromedriver를 사용했을때 발생합니다. [여기](https://sites.google.com/a/chromium.org/chromedriver/downloads)에서 Linux용으로 다운받아 사용하면 됩니다.

### case_2
```bash
... 생략 ...
selenium.common.exceptions.WebDriverException: Message: Service ./chromedriver unexpectedly exited. Status code was: 127
```
위의 에러는 크롬이 설치되어있지 않을때 발생하는 에러입니다. chromedriver를 사용하기 위해서는 당연하게 Chrome도 필요합니다([참고](https://blog.miyam.net/104)).  
아래와 같이 yum으로 크롬 브라우저를 설치해줍니다.
```bash
# 환경 설정
sudo vi /etc/yum.repos.d/google-chrome.repo
-----------------------------------
[google-chrome] 
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
enabled=1
gpgcheck=0
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
-----------------------------------

# install
sudo yum install google-chrome-stable

# check
# chromedriver의 버전과 같아야 함
[centos@ip-172-26-3-214 yum.repos.d]$ google-chrome --version
Google Chrome 87.0.4280.88
```

### case_3
```bash
...생략...
selenium.common.exceptions.WebDriverException: Message: unknown error: session deleted because of page crash
from unknown error: cannot determine loading status
from tab crashed
  (Session info: headless chrome=87.0.4280.88)
```
서버 사양이 부족하면 발생하는 에러입니다. 크롬은 메모리를 많이 잡아먹기에, 적어도 ram 1g 이상의 서버를 사용하는 것이 좋습니다. 위의 에러는 ram 512mb에서 자주 발생하였습니다.  