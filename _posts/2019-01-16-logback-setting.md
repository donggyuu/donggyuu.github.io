---
layout: post
title: "setup logback"
description: "Explanation loggin on JAVA and how to set"
category: articles
tags: [java, logging]
comments: true
---
*under construction*  
*修正中*  
*작성중*  

<!-- contents -->
- [what is logback](#id-section1)
- [SLF4J and facade pattern](#id-section2)
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
아래를 참고하여 md파일을 완성. 

여러가지 로깅 모듈이 있음. 
interface역할을 하는 @Slf4j이 있음. 
그리고 엥간한건 다 SL4j 를 지원하기 때문에 
우리가 쓸 때에는 @SL4j를 쓰거나 몇 줄 추가하면 끝.

1. logback.xml 설정방법
공식 : https://logback.qos.ch/
쉬운 방법: https://taetaetae.github.io/2017/02/19/logback/

여기도 참고
https://jeong-pro.tistory.com/154
스프링부트에서의 설정 방법. 

2. lombock과 같이 사용할 때 @Slf4j는 뭐임?
왜 SL4J를 사용하는가? 
https://inyl.github.io/programming/2017/05/05/slf4j.html

https://taetaetae.github.io/2017/02/19/logback/
여기의 롬복 코드도 참고하렴. 

0. SL4j는?
자바 진영에서 로깅 처리를 하는 일련의 흐름
(공식 문서에서 지원하는 그림)
facade 패턴으로 구현된 창구 일원화 시스템.
// http://jusungpark.tistory.com/23
// 영어로 된 참고 사이트(가능하면 official찾기)
lombock을 쓴다면 @slj4붙이고, log.info이런 식으로 쓴다.
장점 : 모듈을 쉽게 교체 가능(전부 replace하지 않아도 된다)  



1. what is logback?
여러가지 로깅 모들이 있다 ( 자바 진영 ), a, b 등
그 중 하나의 로깅 모듈. 
~한 이유로 많이 쓴다. 
	1.
	2.
	3.

2.SL4과 facadpattern 

2. logback.xml setting
설정 파일의 경우 쓰는 모듈에 따라, spring-boot에서 설정 가능한 파일이 다르다. 
이름만 다를 뿐 , 결론 세팅은 똑같음. 


3. 세팅 방법.

 https://jeong-pro.tistory.com/154
 요걸 보면서 설정방법 연구. 
 
++모든 것에는 반드시 official한 링크를 붙일것. 

http://wonwoo.ml/index.php/post/396
요기 logback.xml에 대한 참고도 있음. 


https://supawer0728.github.io/2018/04/07/spring-boot-logging/
여기도 참고. 
여기를 보면, spring-boot를 사용한다면 logback.xml은 더 사용할 필요 없고
.yml에서 해결 가능. 
++profile전략 -> 시간되면 업데이트. 
-->