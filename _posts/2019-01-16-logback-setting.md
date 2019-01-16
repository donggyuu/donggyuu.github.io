---
layout: post
title: "how to set logback.xml"
description: "Explanation loggin on JAVA and how to set"
category: articles
tags: [java, logging]
comments: true
---
**under constructor**

<!-- 목차 만들기 -->
# My Table of content
- [Section 1](#id-section1)
- [Section 2](#id-section2)

<div id='id-section1'/>
## Section 1
<div id='id-section2'/>
## Section 2


# what is logback
abcd
* Java VM은 DNS lookup시 TTL(time-to-live)을 쓰지 않고, 한 번 lookup한 도메인 이름은 VM이 내려갈때까지 계속 캐싱을 함(DNS Spoofing을 막기위해)   
* JVM의 버전에따라 다르지만, 1.6 이전의 경우는 Default로 Forever 1.6 이후 버전에 대해서는 30초 DNS Cache를 함 
* SecurityManager가 존재 할 경우는 Application이 켜진동안 Cache를 만료시키지 않고, SecurityManager가 존재하지 않을경우, 30초간 Cache가 default    

## problems?  
* 외부에 있는 서버의 IP가 변경되는 경우, 이전 IP 로 접속하는 문제가 발생할 수 있음(google의 reChPTCHA는 가끔 IP 를 변경한다고...)  

## solution
"2"의 networkaddress.cache.ttl가 주석처리되어있거나, "3"의 SecurityManager가 존재하지 않으면서 버전이 1.6 이후라면, default로 30초 DNS cache라고 생각하면 된다.  

#### 1. VM을 완전히 내렸다가 올림(재가동), WAS 의 경우 WAS shutdown/restart


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

2. logback.xml setting
세팅 방법.

 https://jeong-pro.tistory.com/154
 요걸 보면서 설정방법 연구. 
 
++모든 것에는 반드시 official한 링크를 붙일것. 



-->