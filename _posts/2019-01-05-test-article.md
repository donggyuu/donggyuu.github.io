---
layout: post
title: "test article"
description: "An AMQP demo using RabbitMQ to send messages from Java to Node.js and then to a browser via Socket.io"
category: articles
tags: [java, test1, test2]
comments: true
---

## what is dns cache?  
* Java VM은 DNS lookup시 TTL(time-to-live)을 쓰지 않고, 한 번 lookup한 도메인 이름은 VM이 내려갈때까지 계속 캐싱을 함(DNS Spoofing을 막기위해)   
* JVM의 버전에따라 다르지만, 1.6 이전의 경우는 Default로 Forever 1.6 이후 버전에 대해서는 30초 DNS Cache를 함 
* SecurityManager가 존재 할 경우는 Application이 켜진동안 Cache를 만료시키지 않고, SecurityManager가 존재하지 않을경우, 30초간 Cache가 default    

## problems?  
* 외부에 있는 서버의 IP가 변경되는 경우, 이전 IP 로 접속하는 문제가 발생할 수 있음(google의 reChPTCHA는 가끔 IP 를 변경한다고...)  

## solution
"2"의 networkaddress.cache.ttl가 주석처리되어있거나, "3"의 SecurityManager가 존재하지 않으면서 버전이 1.6 이후라면, default로 30초 DNS cache라고 생각하면 된다.  

#### 1. VM을 완전히 내렸다가 올림(재가동), WAS 의 경우 WAS shutdown/restart

#### 2. java.security파일을 수정  
* $JAVA_HOME/jre/lib/security/java.security의 networkaddress.cache.ttl를 수정.
```
# default는 주석처리 되어있음. 단위는 sec
networkaddress.cache.ttl=0
```

#### 3. SecurityManager 설정  
"2"만으로 전체변경 적용해도 되지만 특정 application만 적용하고 싶은 경우가 있다.  
application initialize과정에서 아래처럼 설정
```
# static 없이 java.security.Security.setProperty ("networkaddress.cache.ttl" , "60"); 도 가능?
static {
    java.security.Security.setProperty ("networkaddress.cache.ttl" , "60");   
}
```

参考にした  
→https://medium.com/@tech.yangs/msa-%EA%B3%A0%EA%B5%B0%EB%B6%84%ED%88%AC%EA%B8%B0-1-java%EC%9D%98-dns-cache-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0%EA%B8%B0-3c92fd02d6cf
→https://www.lesstif.com/pages/viewpage.action?pageId=17105897
