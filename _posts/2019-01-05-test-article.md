---
layout: post
title: "DNS Cache Problems"
description: "explanation about DNS cache problems"
category: articles
tags: [Java, DNS]
comments: true
---


<!-- contents -->
- [What is DNS cache](#what is DNS cache)
- [Problems](#problems)
- [Solutions](#method)
  - [Restart VM](#vm restart)
  - [Modify java.security](#java.security)
  - [Modify SecurityManager](#SecurityManager)
- [Reference](#reference)


<div id='what is DNS cache'/>
# What is DNS cache
Java VM은 DNS를 lookup할 때 도메인 이름을 캐싱한다. VM 버전 1.6 이전에는 VM이 내려갈 때까지 캐싱(deafult=forever), 1.6이후에는 SecurityManager가 존재할 경우는 어플리케이션이 실행되는 동안 캐싱, 존재하지 않을 경우는 30초간 캐싱하는 것이 default이다.  


<div id='problems'/>
# Problems
보안상의 이유(DNS책spoofing)로 캐싱을 하지만, IP가 자주 바뀌는 요즘에는 캐시가 남아 있으면 곤란한 경우가 있다. 에를 들어, 외부에 있싱 서버의 IP가 변경되는 경우, 이전 IP로 접속하게 되어 문제가 발생한다(google의 reCAPTCHA 같은 경우 가끔 IP를 변경한다).

---

<div id='method'/>
# Solutions

<div id='vm restart'/>
## Restart VM
이미 서비스가 실행중인 경우 재가동하는 것은 그리 좋은 선택은 아니다. 개발 단계에서 DNS 캐싱 문제가 발생했다면 고려해 볼만 하다고 생각한다.

<br/>

<div id='java.security'/>
## Modify java.security
"$JAVA_HOME/jre/lib/security/java.security"의 "networkaddress.cache.ttl"을 수정한다.
```javascript
# deafult는 주석처리 되어 있음
# 30초간 캐싱
networkaddress.cache.ttl=30
```

<br/>

<div id='SecurityManager'/>
## Modify SecurityManager
java.security 수정의 경우 시스템 전체에 변경사항이 적용되는 단점이 있다. 따라서 특정 application만 캐시 설정을 적용하고 싶은 경우 유용하다. 해당 application의 초기화 코드에서 networkaddress.cache.ttl을 설정한다.
```javascript
# 30초간 캐싱
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

<br/>

<div id='reference'/>
# Reference
- AWS-JVM TTL설정: https://docs.aws.amazon.com/ko_kr/sdk-for-java/v1/developer-guide/java-dg-jvm-ttl.html
- JAVA의 DNS Cache이슈 해결: https://medium.com/@tech.yangs/msa-%EA%B3%A0%EA%B5%B0%EB%B6%84%ED%88%AC%EA%B8%B0-1-java%EC%9D%98-dns-cache-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0%EA%B8%B0-3c92fd02d6cf
- TTL이란: https://m.blog.naver.com/aka_handa/10081414226