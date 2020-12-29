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
last_modified_at: 2020-12-29T08:06:00-05:00
published: false
---


## 제네릭이 뭔데?
제네릭(Generic)은 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미한다. – 생활코딩
Generics add stability to your code by making more of your bugs detectable at compile time. – Oracle Javadoc

## 왜 쓰는가?
제네릭 지원 전 : 컬렉션에서 객체를 거낼 때마다 형변환
지원 후 : 컬렉션이 담을 수 있는 파일을 컴파일러에게 알려줌. 
컴파일러가 알아서 형변환 코드를 추가한다. -> 더 안전하고 명확


## 예제
list에 대한 것은 
https://yaboong.github.io/java/2019/01/19/java-generics-1/
블로그에 자세히 설명되어 있다. 

## 예제2
자바5 이전에는 제네릭이 없고 raw type을 사용했다. 
```Java
private final Collection stamps = ...; 

```

##### 인용
잠깐, 여기서 Collection이란, Collections와는 다르다.
Java에는 동적할당이 가능한 자료구조 
(ArrayList, LinkedList, Vector, Stack, HashSet, HashMap, Hashtable)
들이 존재하며, 
통합된 구조의 Collection Interface를 통해 element의 조회, 저장 등을 제공한다. 

Collection : https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html
Collections : https://docs.oracle.com/javase/8/docs/api/?java/util/Collections.html
자료구조 관련 클래스

이미지 : generic-in-java
##### 인용


아래와 같은 실수가 발생할 수도...
```Java
stamps.add(new Coin(...));
```
꺼내기 전까지는 모른다...

```Java
for (Iterator i = stamps.iterator(); i.hasNext(); ) {
  Stamp stamp = (Stamp) i.next(); // ClassCaseException발생
  stamp.cancel();
}
```

애초부터 Stamp인스턴스만 취급한다는 것을 초장에 명시하면 에러가 사전 체크 가능


```Java
private final Collection<Stamp> stamps = ...;
```
이렇게 작성 ㅇㅋ? 
Collection으로 객체 처리를 할 때 문제가 없다. 

