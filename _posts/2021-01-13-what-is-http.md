---
title: "Java Generic의 개념과 사용하는 이유"
excerpt: "Collection interface를 예시로 Generic에 대해 설명"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/generic-in-java.png
categories:
  - Development 
tags:
  - Java
last_modified_at: 2021-01-13T08:06:00-05:00
published: false
---

서버에 자원을 요청하는 프로토콜.
- 이미지, html파일 같은 ...

http 콜론 그리고 뒤에 주소 찍고 가져올 리소스를 적는다.

http 자체는 단순한 줄 단위의 텍스트이다.
시작줄(request일떄는 get /text/....jpeg, response일때는 200 ok같은)
헤더(content type, content lengh같은)
그리고 본문(html 등 )

http는 application 레벨이고 그 아래
데이터 전송을 위해서 TCP/IP를 사용한다
TCP : 데이터를 패킷 단위로 쪼개서, 순서가 바뀌어도 원래 순서를 유지하면서 전송이 가능한 프로토콜 
IP는 주소를 찾는 것
TCP : 3-way handshaking
- syn -> syn, ank -> ack
- https://kychul98.tistory.com/34



UDP : 
TCP와 달리 UDP는 비연결형 프로토콜입니다. 즉, 연결을 위해 할당되는 논리적인 경로가 없는데, 
그렇기 때문에 각각의 패킷은 다른 경로로 전송되고, 각각의 패킷은 독립적인 관계를 지니게 되는데 이렇게 데이터를 서로
다른 경로로 독립적으로 처리하게 되고, 이러한 프로토콜을 UDP라고 합니다. 


# HTTPS?
