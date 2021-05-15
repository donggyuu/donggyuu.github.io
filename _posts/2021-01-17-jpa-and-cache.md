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
http에서의 캐싱(브라우저)과, 어플리케이션에서의 캐싱



## 캐싱의 개괄
HTTP 통신 사이에서의 캐싱 
어플리케이션 내부에서의 캐싱 
redis나 hazelcast같은 것에서의 캐싱 

## 이전 팀의 hazelcast는 어떻게 동작하는거지?
추상화된 @Cachable을 그냥 가져다 쓰는 거면, hazelcast는 어떻게 동작하는가?
https://yonguri.tistory.com/82

- 의존성을 추가하고 설하고 구현체를 사용 가능하다

의존성 없는 기본 캐시는?



## 언제 application 내에서 캐시를 사용해야 하는가?
이거 그러면 언제 변경이 된 것을 알지? 이것도 spring 내부적으로 아는가?
@Cacheable 을 구현한 구현체에 따라 다르겠지..

사실 캐싱하는 쪽에서는 디비를 일일이 안 찔러보는게 목적이기 때문에 캐셔블의 관심사는 아님.
비즈니스 요건에 따라서 얼마나 ttl를 설정하는지가 개발자의 몫이라고 생각합니당

카페인 캐시랑 redis 캐시
실제 캐셔블에 적용되는 구현체를 어딘가에서 설정해놔야 할거고
캐싱 되는 단위를 프로그래머블하게 설정할 수 있는데
캐셔블 애노테이션이 method에 적용 가능해서
메소드 단위로 키를 주고 이에따라 ttl 같은게 설정이 가능하고
같은 시그니처에 같은 파라미터가 들어온걸
가령 LRU 구현체라면
계속 트래킹을 하다가 evict하고 넣고 빼는걸 반복하는 것으로 알고 있습니다

카페인 캐시가 메모리 기반이라서 인프라 없이 설정이 가능한데
예제 따라해보면서 테스트 케이스 작성해보면 감이 올겁니다


메트릭 어떻게 봤고
ttl 어떻게 조정했고 했는지
일단 히팅수가 높을거 같은 데이터 들이 있고 아닌 애들이 있고

메인 디비 부하 줄이더라도 레디스 찰랑찰랑하게 해두면 그것도 좋지 않기 때문에 중도를 잘 찾는 애티튜드와 고민을 보여주면 좋지 않을까 합니다

사실 레디스 같은거 넘어가면
클러스터링이라든가
뭐 이런 인프라적인 것들도 보면 좋지만

session은 어떻게 구현하는가?
session 캐시 쿠키 차이점.

이런건 히팅율이 낮기 때문에 ㅋㅋ
인프라 설정까지 개념만 봐두면 도움될거라 봅니다


## @Cachable는 spring과 JPA가 서로 다른가?


## 1, 2차 캐시와 JPA에 대한 정리

JPA에서는 EntityManager로 entity를 CRUD한다. 
1차 캐시는 영속성 컨텍스트 내부에 있다. 엔티니 매니저로 조회하거나 변경하는 모든 엔티티는 1차 캐시에 저장된다.

entity manager?
https://bamdule.tistory.com/49#:~:text=%EC%98%81%EC%86%8D%EC%84%B1%20%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80%20%EC%97%94%ED%8B%B0%ED%8B%B0%EB%A5%BC,%EC%8B%9C%ED%82%A4%EB%8A%94%20%ED%99%98%EA%B2%BD%EC%9D%84%20%EC%9D%98%EB%AF%B8%ED%95%A9%EB%8B%88%EB%8B%A4.

## JPA와 하이버네이트? 애초에 JPA가 뭔데?
차이점 기술
https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/

spring hateoas 에 대한 글 정리중 ...

참고중 : 
https://engkimbs.tistory.com/866
https://sas-study.tistory.com/369

https://cla9.tistory.com/97
여기가 제일 자세한 것 같은데 이해가 잘...


https://chaibin0.tistory.com/entry/Spring-HATEOAS-%EC%A0%81%EC%9A%A9
여기를 보고 적용 
response entity가 무엇인지는 raxy 코드를 보고 학습
왜 response entity를 쓰는지?
락시 코드에는 왜 getSearchResults 여기만 들어가있고
나머지는 그냥 dto 클래스를 반환하는지 ?


일단 인강에서 쓰는 Resource는 deprecated 되었음.
Entitymodel을 쓰는데, 블로그를 보면 ResponeEntity랑 같이 쓴다.
왜 그렇지? 어떨때 쓰고 안쓰지? 락시는 왜 하나만 있지?
그리고 타고 들어가서 코드 짜여진거 읽는 방법 및 api doc 보는법 다시!

```
    @GetMapping("/test1")
    public ResponseEntity<EntityModel<Domain>> basicHateoas() {

        Domain domain = domainService.getDomain();
        return ResponseEntity.ok().body(
                EntityModel.of(domain)
                        .add(linkTo(methodOn(DomainController.class).basicHateoas()).withSelfRel())  // /hateoas/test1 - self
                        .add(linkTo(DomainController.class).slash("test").withRel("test"))    // /hateoas/test - test
                        .add(linkTo(methodOn(DomainController.class).dummy()).withRel("dummy"))      // /hateoas/dummy - dummy
                        .add(linkTo(methodOn(DomainController.class).dummy2(1L)).withRel(IanaLinkRelations.TAG))  // /hateoas/1 - tag
        );
    }
```


환경 : maven

본인 환경에 맞는 버전을 다운
```bash
		<dependency>
			<groupId>org.springframework.hateoas</groupId>
			<artifactId>spring-hateoas</artifactId>
		</dependency>
```
여담으로  버전까지 명시해서 빌드하는 것이 좋다. 
https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-hateoas

https://docs.spring.io/spring-hateoas/docs/1.0.1.RELEASE/reference/html/
(사진)
여기를보면 이름 바뀌는 수정도 있어...


"사진"
프로젝트>[프로젝트 이름]>마우스 오른쪽 버튼으로 클릭>Maven>다시 가져 오기


메이븐 빌드. 인텔리제이 재시작 필요할수도 있다. 
```bash
./mvnw clean package
```




