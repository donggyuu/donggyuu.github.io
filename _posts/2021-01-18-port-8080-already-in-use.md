---
title: "JPA의 개념과 JPA에서의 캐시(Cache)"
excerpt: ""
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


사진 스샷 찍어둠
```bash
Web server failed to start. Port 8080 was already in use.
```

혹은 8080소리 없이..그냥 아래처럼 JavaVirtualMachines만 뜰 수도 있다.
```
Process 'command '/Library/Java/JavaVirtualMachines/jdk1.8.0_31.jdk/Contents/Home/bin/java'' finished with non-zero exit value 2
```


8080가 이미 다른 application에 돌아가고 있다는 소리니..
로컬이면 개발하다가 그럴 수도 있다. 
죽여줘야 한다.

```bash
# 8080 돌고있는지 확인
lsof -n -i4TCP:8080
---------------------
don$ lsof -n -i4TCP:8080
COMMAND   PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
java    36658  don   87u  IPv6 0xf836cdd95908282b      0t0  TCP *:http-alt (LISTEN)


# PID로 해당 프로세스를 kill
kill -9 PID
---------------------
don$ kill -9 36658


# 8080돌고 있는지 확인
don$ lsof -n -i4TCP:8080
```


다시 gradlew을 실행
```bash
./gradlew bootrun
# 혹은 IDE에서 run
```



---

에러2
```bash
***************************

APPLICATION FAILED TO START

***************************

Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class
```


SpringBoot는 어플리케이션이 시작될 때 필요한 기본 설정들을 자동으로 설정하게 되어있는데, 그중에 DataSource 설정이 자동구성 될 때 필요한 데이터베이스 정보가 설정되지 않아 발생하는 문제다.

아마도 build.gradle에 data-jpa 만 의존성 넣고 필요한 DB 관련은 안 넣었을테니 추가하자.
나는 h2디비 관련 library를 추가 
```bash
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	...
	runtimeOnly 'com.h2database:h2'
  ...
}
```
gradle을 reload해주 재실행

참고 : https://lemontia.tistory.com/586