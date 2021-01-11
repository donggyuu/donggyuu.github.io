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
  - dev_Java
last_modified_at: 2021-01-12T08:06:00-05:00
published: true
---
<script src="https://ads-partners.coupang.com/g.js"></script>
<script>
	new PartnersCoupang.G({ id:368772 });
</script>  
<br>

## Generic?
일단 사전적 정의부터 살펴보겠습니다.  
- from 생활코딩  
제네릭(Generic)은 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미한다.
- from Oracle Javadoc  
Generics add stability to your code by making more of your bugs detectable at compile time.

좀더 구체적으로는 아래와 같은 장점이 있습니다.  
(Collection Interface를 사용한다고 가정)    

- Generic 사용전   
컬렉션에서 객체를 거낼 때마다 형변환이 필요   
- Generic 사용후   
컬렉션이 담을 수 있는 파일을 컴파일러에게 알려주기 때문에 컴파일러가 알아서 형변환 코드를 추가하여 **더 안전하고 명확**해 진다.  

## 예시  
자바5 이전에는 Generic이 없었고 객체를 담기 위해서 raw type을 사용했습니다.  
(Collection interface를 예로)

```java
private final Collection stamps = ...; 
```

> 여기서 Collection는 Collections와는 다릅니다.
Java에는 동적할당이 가능한 자료구조(ArrayList, LinkedList, Vector, Stack, HashSet, HashMap, Hashtable)들이 존재하며, 통합된 구조의 Collection Interface를 통해 element의 조회, 저장 등을 제공합니다.   
From Javadoc > [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)  vs [Collections](https://docs.oracle.com/javase/8/docs/api/?java/util/Collections.html)

Collection에 Coin 타입의 객체를 담아줍니다.  
```java
stamps.add(new Coin(...));
```

그런데 raw type을 사용하면 객체를 꺼낼 때 형변환에서 문제가 발생할 수 있습니다.  
```java
for (Iterator i = stamps.iterator(); i.hasNext(); ) {
  Stamp stamp = (Stamp) i.next(); // ClassCaseException발생
  stamp.cancel();
}
```
Coin타입의 객체를 저장했기때문에 꺼내어 사용할 때도 Coin타입으로 형변환을 해야 하지만 실수로 다른 타입으로 형변환을 하여 에러가 날 수 있는 것입니다.  
형변환하기 전까지는 문제를 발견할 수 없습니다.  

따라서 Generic을 사용, 처음부터 Stamp객체만 취급한다는 것을 명시하면 사전에 에러를 체크할 수 있습니다.  
```java
private final Collection<Stamp> stamps = ...;
```
Generic으로 처음부터 Stamp타입만 받도록 명시를 하면 추후 문제가 없습니다.  

**참고**  
yaboong님의 블로그  
https://yaboong.github.io/java/2019/01/19/java-generics-1/  
<이팩티브 자바>   
https://coupa.ng/bPqc7H