---
published : false
layout: post
title: "setup lightsail for test server"
description: "setup lightsail"
category: articles
tags: [test, server]
comments: true
---
*under construction*  
*修正中*  
*작성중*  
젠킨스 세팅하기 -> aws에서..
aws에서 centos를 설치하고 기본적인 아파치랑 톰캣 설정하는 것 적기. 

http://blog.freezner.com/archives/1249
여기를 참고해서 접속하면 됨.
나의 경우 cenots -> muse-mac:aws muse$ ssh -i ./aws_keypair.pem centos@3.112.30.140


jenkins를 설치하려고 접속했떠니 wget 치면  command not found
-> have to install wget in centos like below
yum install wget

그런데 또 아래의 에러가...  
Fail to set locale, defaulting to C
아마  centos 청므 설치하면 이러는듯.
https://www.cyberciti.biz/faq/failed-to-set-locale-defaulting-to-c-warning-message-on-centoslinux/
여기를 참고해서 고치렴.   

근데.. 기본적인 것들이 들어있어야 함
java라던가...yum패키지..이런 것들..! 

추가로, 
[centos@ip-172-26-6-154 ~]$ sudo systemctl start jenkins
[centos@ip-172-26-6-154 ~]$ sudo systemctl status jenkins
로 jenkins가 active인지 확인.
active (exited)  이면 문제가 있음. -> 딱히 문제가?
--> 톰캣..같은 기본적인 http서버 등은 설치되어 있는 것을 전제로!! 
아..기본 톰갯 등은 없어도 경량 서버가 자체적으로 깔려있어서 상관없음
다만 aws를 쓰는 경우, aws내에서 방화벽 설정을 따로 해주어야 한다. 




ligshtsail의 대시보드 화면에서, networking에서 static ip를 발급받고  
그 이후에 networking page에서 권한 설정을 해 주어야 함.

톰캣은 8080이라서 잘 들어가지는데 젠킨스는 왜 안됨? 
그리고 jenkkins_port로 해햐지
jenkins https port를 같이 수정하면 안되는 경우가 있어..
하라는 대로 하기.

sudo cat 경로 
잘 되었는데 또 안되면 jenkins restart 명령어를 쳐본다. 

an Error occured 화면이 뜨는 경우
https://issues.jenkins-ci.org/browse/JENKINS-45388
위의 페이지의 에러가 뜨는 경우, 
pw를 새로 편집해 줄 필요가 있음.
아래처럼! 테쥰에따라서 진행한다. 
```
The symptom can be accomodated by modifying the contents of /var/lib/jenkins/secrets/initialAdminPassword before first contact with a browser.

For example, change:

b0dc273fe0b745708ec1cb91f88eda81
to

passwd:b0dc273fe0b745708ec1cb91f88eda81
Then set up Jenkins:

Direct browser to <appliance>:8080
Login as user "admin" with the original contents of initialAdminPassword
click [install suggested plugins]
click [continue as admin]
click [start using Jenkins] - The "cannot connect" error appears
click [retry] - The "cannot connect" error is resolved
```
-->앞에 passwd붙인 다음,
해당 포트로 들어가서 로그인은 admin / passwd:의 뒤의 것, 즉 passwd:를 뺀 원래 암호를 입력하면 됩니다. 
amdin으로 로그인을 하면, 
위의 링크는 install suggested plugins이지만, 그거 누르면 또 에ㅓㄹ뜸

http://www.teknotrait.com/blog/article/Making-Parameterized-Build-On-Jenkins
여기에 따라서, select plugin을 누르고 위의 링크에 맞는 플러그인을 선택후 설치한다. 

중간에 에러 뜨면,,다시 젠킨스 재시작 하고 다시 서버 접속해서 반복..
걍 인터넷 속도 문제인거같기도.. 
다시 반복해서 restart해서 접속해서 들어가면, resume버튼 있을것. 


중간에 에러 뜨고 서버에 접속이 안되는 것은
서버 상의 메모리 문제일 수도 있음

systemctl stop firewalld.service
로 한 후 젠킨스  restart해보면 됨.
https://stackoverflow.com/questions/44858275/jenkins-refused-to-connect-on-port-8080


https://zetawiki.com/wiki/CentOS_JDK_%EC%84%A4%EC%B9%98
센토스 자바설치 
와.. 램이 512메가이면 설치에 에러가 뜨네.. 힙영역 메모리 꽉차서..ㅅㅂ..

<!-- contents -->
- [what is logback](#id-section1)
- [facade pattern and SLF4J](#id-section2)
- [setup logback](#id-section3)


<div id='id-section1'/>

# what is logback
Logback is one of logging modules in Java. **Logback** and **Log4j2** are usually used.  

<!--또한 log4j(2)를 기반으로 보완한 것이 logback이니 이걸 사용하는 것이 좋음.  
왜 써야 하는지 이유는 아래 참고 작성  
logback.xml즐찾 폴더 -> 사용해야 하는 이유 
-->
*official : https://logback.qos.ch/*

<div id='id-section2'/>

# facade pattern and SLF4J
<!--facade pattern이랑 순서 바꾸셈. -->
## facade pattern
facade pattern의 일반적인 예   
그림 첨부 ( official)  
```
전화거는 사진 -> 출처는 아래 링크. 
```
여러 문의를, 우리는 고객담당자에게 전화하면 된다.   
창구 일원화 시스템. 

상세하게 보고싶다면 ? https://sourcemaking.com/design_patterns/facade

SLF4J로 보는 facade pattern.
```
내가 직접 그려 -> 전화거는 사진, cs는SLF4J, 아래 나머지는 로깅 모듈 
```
SLF4J만 있으면, 아래의 로깅 모듈에 상관없이 쓸 수 있음.   
interface를 쓰는 이유와 동일 

## SLF4J
The Simple Logging Facade for Java (SLF4J) serves as a simple facade or abstraction for various logging frameworks (e.g. java.util.logging, logback, log4j) allowing the end user to plug in the desired logging framework at deployment time.  
official : https://www.slf4j.org/index.html  
즉, 이걸 쓰면 다양한 모듈을 갈아끼우는데 아무런 문제가 없다.  
아래의 예제를 보자. 
```
기존의 코드. import 를 하고 있기 때문에 문제
```

```
바뀐 코드. getClass로 모듈의 path만 지정해주면 된다. 문제 없음. 
```

```
lombock을 사용하면 좀더 심플해짐
https://taetaetae.github.io/2017/02/19/logback/
```
interface를 쓰는 이유와 동일  
이것은 즉 아래 facade 패턴 ,창구 일원화 임.  

<div id='id-section3'/>

# setup logback

*아래 링크 이해후 정리*

https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-logging.html
여기를 보면 모듈별 지원하는 xml이 있음.

세팅 참고 url  
https://jeong-pro.tistory.com/154
http://wonwoo.ml/index.php/post/396  


발전된, .xml말고 .yml로 세팅하는 방법.   
https://supawer0728.github.io/2018/04/07/spring-boot-logging/  
여기를 보면, spring-boot를 사용한다면 logback.xml은 더 사용할 필요 없고
.yml에서 해결 가능. 
++profile전략 -> 시간되면 업데이트. 


<!--
각종 환경 테스트를 위해 docker ..등등 
실제 간단한 server를 만들고, 하나 3.5달러 플랜이 생겼고 대중성 있으니,
해보자.
기업에서 많이 쓰는 centos


아래는 centos버전 확인 (리눅스 버전 확인)
https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EC%A2%85%EB%A5%98_%ED%99%95%EC%9D%B8,_%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%B2%84%EC%A0%84_%ED%99%95%EC%9D%B8


-->
