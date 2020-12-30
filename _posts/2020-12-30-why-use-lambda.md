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

## Java8 이전에는 Lambda가 없고 익명클래스
추상 method를 하나만 담은 인터페이스.
특정 함수 혹은 동작을 나타내는데 사용.
함수 객체를 만드는 주요 수단은 익명 클래스였다. 

```Java
책의 collections 에 , inerface를 추가해서 기록

```
함수형 인터페이스 -> 
https://juyoung-1008.tistory.com/48
여기에 functional interface의 개념이 있다.

runnable도 functional intercae임. 
https://donggyulee.blogspot.com/2020/02/lambda-in-anonymous-class.html
를 참고

JAva8에서는 추상 메서드 하나짜리 특별대우 -> 람다식으로 인스턴스를 만들수있게 된것.

```Java
책의 코드
```
컴파일러가 문맥을 추로낳여 해줌
python처럼.
따라서 타입을 명시해야 코드가 더 명확할 때를 제오히ㅏ면 람다의 모든 매개변수 타입은 생략하자.

## 항상 람다 써? 
람다 -> 이름 없고 문서도화도 못함 -> ㅋ드 자체로 동작이 명확하게 설명되지 않거나 
줄수가 너무 많아지면 쓰지 말아야..
람다는 함수형 인터페이스에서만 쓰임 -> 추상 클래스의 인터페이슬르 만들 때 람다 못 쓰니 익명 클래스를 써야. 추상 method가 여러개인 인터페이스으 ㅣ인스턴스를 만들 떄도 익명 클래스

함수 객체가 자기 자신을 참조해야 한다면 반드시 익명 클래스
```Java
https://mommoo.tistory.com/16
의 코드 예제 에 this 나온다 
```

+ 람다 직렬ㄹ화는 삼가야...
<이팩티브 자바> 258 page


## 활용하여 8부터는 stream()도 있다
https://velog.io/@litien/Stream-API
를 참고하여 작성.
맛보기 정도로

참고 : 
https://mommoo.tistory.com/16

https://donggyulee.blogspot.com/2020/02/lambda-in-anonymous-class.html