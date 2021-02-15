---
title: "MySQL의 디비 엔진"
excerpt: "Java8 전후로 Lambda의 개념에 대한 설명, 그리고 사용하면 안되는 케이스를 정리"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/generic-in-java.png
categories:
  - Development 
tags:
  - Java
last_modified_at: 2020-12-29T08:06:00-05:00
published: false
---
엔진은 plugin 방식
default는 inno DB

InnoDB
MySQL 에서 기본으로 설정되는 엔진입니다.
일반적인 DBMS 기능을 지원합니다.


MyISAM
트랜잭션을 지원하지 않고, Table 단위의 Locking 입니다.
따라서, 다수개의 세션이 동시에 작업하는 경우 성능이 저하될 수 있습니다.


ARCHIVE
로그 수집에 적합한 엔진입니다.
데이터가 메모리상에서 압축된 후, 압축된 상태로 디스크에 저장됩니다.
한번 INSERT 된 데이터는 UPDATE 나 DELETE 를 할 수 없으며, 인덱스를 지원하지 않습니다.

** 
하지만 확실한 것은 1대의 서버에 두 가지를 혼용하지 않는 것이 좋습니다. 이둘은 각기 다른 동작, 메모리 사용법을 취하므로 혼용하는 환경에서는 효율적인 CPU, 메모리 사용이 어려워집니다.

Maria vs. InnoDB 차이 :
https://mozi.tistory.com/91
https://12bme.tistory.com/95




## 로킹 관련 
https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/#:~:text=Gap%20lock%EC%9D%80%20DB%20index,index%EA%B0%80%20%EA%B1%B8%EB%A0%A4%EC%9E%88%EB%8B%A4%EA%B3%A0%20%ED%95%98%EC%9E%90.

+ pessimistic lock을 보기.