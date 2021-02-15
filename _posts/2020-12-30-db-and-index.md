---
title: "Java에서 Lambda란? 그리고 언제 사용하지 말아야 하는가"
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
next key lock
https://jeong-pro.tistory.com/114
보니까 별내용은 없구나 ㅇㅋ

mysql의 기본은 repeatable read ->
즉 항상 같은 것을 read. 팬텀 리드 없음. 

또한 gaplock을 걸지. 내가 조작을 가하려는 row의 후보군을 다른 transaction이 건들지 못하게 한다.
between일떄 !! 



https://cecil1018.wordpress.com/2016/06/09/mysql-isolation-level/


추가적으로 락에 대해서!
https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/#:~:text=Gap%20lock%EC%9D%80%20DB%20index,index%EA%B0%80%20%EA%B1%B8%EB%A0%A4%EC%9E%88%EB%8B%A4%EA%B3%A0%20%ED%95%98%EC%9E%90.

https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/#:~:text=Gap%20lock%EC%9D%80%20DB%20index,index%EA%B0%80%20%EA%B1%B8%EB%A0%A4%EC%9E%88%EB%8B%A4%EA%B3%A0%20%ED%95%98%EC%9E%90.


https://cecil1018.wordpress.com/2016/06/09/mysql-isolation-level/


## nosql ?
http://astrod.github.io/db/2017/01/22/NoSQL_1%EC%9E%A5/

https://siyoon210.tistory.com/130

그러니까 범용은 rdbms인데, mongo db같은 no sql도 있는 거네.
redis는 이러한 no sql이 in memory에 되어 있는 것이고.


## 엘라스틱 서치 :  
전문 검색(Full-text Search)
내용 전체를 색인(index)하여 특정 단어가 포함된 문서를 검색하는 것이 가능합니다.

mysql은 보통 colimn을 인덱싱 하지는 않지..보통 key가 되는value를 인덱싱.
왜냐하면 where, join에서 자주 쓰이는 것을 index해야 key값 같은 성능이 올라가.
단순 text를 인덱싱하지 않아. 그리고 그 많은 문장 단어 중에서 뭘 idexing할 것인가? 안되지 .