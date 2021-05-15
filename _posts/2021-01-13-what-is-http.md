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
  - Development-Skills
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
Wireshark를 이용해 간단히 HTTP의 패킷을 분석해보았을 때, POST로 보낸 데이터가 평문 텍스트로 그대로 노출되어 있는 것을 볼 수 있었다. 즉, 암호화되지 않은 데이터를 전송하기 때문에 서버와 클라이언트가 주고 받는 메시지를 그대로 노출하기 때문에 보안에 아주 취약하다. 이러한 점을 보완하여 보안을 더욱 강화한 프로토콜이 HTTPS인 것이다. 

출처: https://coding-start.tistory.com/208 [코딩스타트]
자세한 동작 방식을 설명하기 전에 간단한 SSL 플로우를 이야기하자면 응용계층의 HTTP 프로토콜에서 사용자의 데이터를 받고 전송계층으로 캡슐화되기 이전에 SSL 프로토콜에 의해서 사용자의 데이터가 암호화된다. 그리고 서버는 전송계층에서 세그먼트를 받아 SSL 계층에서 데이터를 복호화하여 응용계층까지 보낸다. 즉, 우리는 SSL 프로토콜만 적용하면 마치 애플리케이션은 SSL을 TCP로 인식하고 TCP는 SSL을 애플리케이션으로 인식하는 것처럼 통신하기 때문에 우리가 별도로 통신에 대해 손댈것은 없다.

출처: https://coding-start.tistory.com/208 [코딩스타트]