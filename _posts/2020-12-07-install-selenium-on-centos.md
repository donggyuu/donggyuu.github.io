---
title: "CentOS에서 Selenium설정하고 실행하기"
excerpt: "CentOS 7에서 Selenium 설정 및 발생 가능 에러 정리"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/install-selenium-on-centos.png
categories:
  - Development 
tags:  
  - Development
last_modified_at: 2020-12-07T08:06:00-05:00
published: true
---

# 다루는 내용  
[이전 포스팅](https://donggyuu.github.io/development/like-click-bot)에서는 Local에서 셀레니움을 이용해 간단한 봇을 만들어보았다. 봇을 서버에 올려 정해진 시간에 동작하도록 하면 수동으로 실행하는 것보다 간단해진다.   
본 글에서는 CentOS7서버에서 셀레니움 봇을 동작하도록 하는 설정을 다룬다.  

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-3803765505787724"
     crossorigin="anonymous"></script>
<!-- develop_blog_infeed -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-3803765505787724"
     data-ad-slot="4883898076"
     data-ad-format="horizontal"
     data-full-width-responsive="false"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 설정  

## Chrome headless
리눅스 상에서 동작할 것이므로 브라우저의 화면이 보일 필요가 없다. Chrome headless는 브라우저 화면의 표시 없이 브라우저를 실행하도록 해준다.   
headless의 설정을 위해 [아래의 코드](https://github.com/donggyuu/like-click-bot/blob/master/like-click-bot.py#L70)를 추가한다.   
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
특정 서비스는 접속 로그의 UA를 보고 사람인지 로봇인지를 판단한다. 따라서 로봇으로 인식되지 않도록 임의의 UA도 설정. 

## CentOS 서버 설정  
미리 준비해둔 CentOS서버에 접속한다. 필자는 Lightsail를 사용하므로 Lightsail에 접속하겠다.
```bash
# 권한으로 접속이 안되면 sudo를 붙여본다
ssh -i /Users/don/aws_keypair_seoul.pem centos@xx.xx.xx.xx
```

CentOS7에 python2는 이미 설치되어 있지만 대세를 따라  python3을 사용하겠다.  
python3의 설치는 yum을 사용하는 방법, 직접 다운로드하는 방법이 있다. 개인적으로 패키지 관리툴의 사용을 선호하므로 yum으로 설치하겠다([이곳](https://www.liquidweb.com/kb/how-to-install-python-3-on-centos-7/)을 참고). 다만 현 시점에서 [yum은 python3.6까지만 지원](https://stackoverflow.com/questions/50408941/recommended-way-to-install-pip3-on-centos7)한다는 단점이 있다.   

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

[pip은 python3.6부터는 같이 포함](https://linuxize.com/post/how-to-install-pip-on-centos-7)되어있기 때문에 따로 설치하지 않아도 된다. 
pip을 사용하여 Selenium을 설치한다. 명령어 "python3.6 -m pip"을 사용하겠다. 

```bash
sudo python3.6 -m pip install selenium
--------------------------------------------
Successfully installed selenium-3.141.0 urllib3-1.26.2
```
[이전 포스팅](https://donggyuu.github.io/development/like-click-bot)에서 만든 봇은 yml에서 설정값을 읽어온다. 따라서 yaml관련 라이브러리도 설치해준다. 
```bash
sudo python3.6 -m pip install pyyaml
--------------------------------------------
Successfully installed pyyaml-5.3.1
```

이제 봇을 위치시킬 경로를 만들어준다.
```bash
mkdir -pv /home/centos/app/like-click-bot
--------------------------------------------
mkdir: created directory ‘/home/centos/app/like-click-bot’
```

## Selenium bot 실행 확인  
로컬에 다운로드해둔 봇을 CentOS7서버로 업로드한다(아래는 Lightsail을 사용하는 경우의 scp 명령어).   
```bash
sudo scp -i /Users/don/aws_keypair_seoul.pem -r /Users/don/like-click-bot centos@xx.xx.xx.x:/home/centos/app/
```

CentOS에서 봇이 잘 업로드 되었음을 확인한다.  
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

최종적으로 Selenium을 사용한 봇을 실행시켜본다. 
```bash
python3 like-click-bot.py
```

# 에러 핸들링  
진행하다보면 2가지 에러가 나올 수 있다. 아래의 조치를 취한 후 재실행해 본다.  

## case_1
```bash
... 생략 ...
OSError: [Errno 8] Exec format error
```
[위의 에러](https://stackoverflow.com/questions/38833589/oserror-errno-8-exec-format-error-selenium)는 운영체제와 다른 chromedriver를 사용했을때 발생. [여기](https://sites.google.com/a/chromium.org/chromedriver/downloads)에서 Linux용으로 다운받아 사용한다.

## case_2
```bash
... 생략 ...
selenium.common.exceptions.WebDriverException: Message: Service ./chromedriver unexpectedly exited. Status code was: 127
```
위의 에러는 크롬이 설치되어있지 않을때 발생하는 에러. chromedriver를 사용하기 위해서는 당연하게 Chrome도 필요하다([참고](https://blog.miyam.net/104)).  
아래와 같이 yum으로 크롬 브라우저를 설치해준다.
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

## case_3
```bash
...생략...
selenium.common.exceptions.WebDriverException: Message: unknown error: session deleted because of page crash
from unknown error: cannot determine loading status
from tab crashed
  (Session info: headless chrome=87.0.4280.88)
```
서버 사양이 부족하면 발생하는 에러. 크롬은 메모리를 많이 잡아먹기에, 적어도 ram 1g 이상의 서버를 사용하는 것이 좋다. 위의 에러는 ram 512mb에서 자주 발생하였다.  