---
title: "웹에서 캐싱(WEB Cache)하는 방법"
excerpt: "http caching을 중심으로 설명"
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

## 웹에서 페이지 속도를 빠르게 하는 방법
- 코드 압축 (Javascript를 minify 하던지 gzip으로 압축 한다던지 등)
- 이미지 처리 (CDN, Split 등)
- Lazy Loading (느린 로딩 기법)
- Critical Rendering Path
- 캐싱 처리 (WebCache 다양한 웹 개시 사용방법이 존재, 캐시 서버 도입 또는 소스에서 내부 캐싱 처리 등, DBCache 등)

출처: https://goddaehee.tistory.com/171?category=281064 [갓대희의 작은공간]


## 웹에서 캐싱하는 개략적인 방법

개념적으로 보면 크게 2가지다 
-  개인용 : 브라우저가 저장

-  여러 사람용 : 캐시 서버 (프록시 서버)를 두어서 캐싱
  - 이때 캐싱을 돕는 미들웨어가 redis나 memcache같은 것들임(spring 의 캐싱 어노테이션도 이들을 이용해서 동작)

redis등을 쓰기 전에 먼저 개인용 브라우저 캐싱http캐싱을 개선해보자 .


## 브라우저 캐싱에 대해서 좀더 자세히...
브라우저가 데이터를 저장하는데... 원래 데이터가 변경되었을 수도 있고 항상캐싱하면 안된다.
어떻게 하면 좋을까? 


모든 브라우저에는 HTTP 캐시 구현이 포함
response header를 보면 힌트를 볼 ㅅ ㅜ있ㄷ ㅏ.

1. 만료일을 명시한다 
브라우저에서 요청을 주면, 해당 캐싱 대상 데이터에 대해서 만료일을 정할 수 있다.
cache control : max-age
Expires : 

기간이 지났다면 -> 서버에서 새 것을 검사한다.
바뀌었담ㄴ -> 컨텐츠 갱신
아니라면? -> 날짜만 갱신

2. 조건부로 검사한다. 
rest api -> get을 해도 변경이 있을 것이다..

2.1. if modified since...
캐싱된 데이터는 날짜가 변경되어 있을 것.
클라잉너트 (브라우저)가 서버에 요청한다 -> 이것은 날짜 기준으로 변경? 
yes -> 갱신해서 반환 
no -> 304 not modified
304정도만 보내주니까 yes보다는 빠르다.

2.2. if-none-match인
- 날짜로 판단 애매한 경우(새로 쓰여지지만 내용은 그대로)
- 사소한 변경 (철자 나 주석 등 ..)
- 최근 변경을 화ㅇㅇㄴ 분ㄹ가능한 경우-
- 초단위로 갱신되는 경우 

퍼블리셔가 문서 변경하ㅁ녀 쓰는 entity tag를 이용한다.
"만료일 "과 마찬가지로 302를 반호나하거나 그럼.


3. 언제 조건부? 언제 만료일 ?
클라이언트는 서버가 뭘 보내는지에 따라 맞추고 
서버는 되도록이면 조건부 엔티티 ㅐㅌ그를 반환하도록 짜는게 좋다. 둘다 반환하도록 해도 OK



참고한 문서들 
참고도서 : HTTP 완벽 가이드 **정말이지 완벽한 가이드다...

## lazy loading
https://scarlett-dev.gitbook.io/all/it/lazy-loading
상세한 설명 : https://helloinyong.tistory.com/297


웹 캐싱 정리 : 
https://goddaehee.tistory.com/171?category=281064
https://goddaehee.tistory.com/169?category=281064


HTTP 캐싱 : 
https://medium.com/@bbirec/http-%EC%BA%90%EC%89%AC%EB%A1%9C-api-%EC%86%8D%EB%8F%84-%EC%98%AC%EB%A6%AC%EA%B8%B0-2effb1bfab12
