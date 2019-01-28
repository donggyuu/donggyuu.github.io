---
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
