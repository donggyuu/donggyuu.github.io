---
title: "웹앱 개발에서의 캐시(Cache)란 무엇인가?"
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
Cache : 캐시는 미래에 필요할 것이고 빠르게 검색할 수 있는 데이터를 담은 저장소 라고 정의 한다. 캐시는 다른 곳에 있는 데이터를 복제하거나, 계산 결과를 임시적으로 저장하는 데이터 저장소이다. 


http에서의 캐싱(브라우저)과, 어플리케이션에서의 캐싱
이 과정에서 별도의 캐시 서버를 둘 수도 있다.

## 캐싱의 개괄
HTTP 통신 사이에서의 캐싱 
어플리케이션 내부에서의 캐싱 
redis나 hazelcast같은 것에서의 캐싱 

### about redis
실제로 Update는 계속 들어와야 하는 작업이므로 변화가 없지만, Select와 Qcache_hits 는 거의 1/10 수준으로 줄어버립니다. DB서버로의 요청이 줄어버려서, 전체 서비스가 좀 더 큰 요청이 들어와도 버틸 수 있게 해줍니다.

그럼 Redis 나 Memcached 같은 Cache 서비스를 사용하는 것은 왜 일까요? 당연히 속도면에서는 각각의 서버의 메모리에 들고 있는 것이 유리합니다. 그런데, 여러 서버에 있는 데이터를 동기화 하는 것은 사실 쉬운 일이 아닙니다. 그리고 데이터량이 많으면, 결국은 한 서버에 둘 수 없어서, 여러 서버로 나뉘어야 합니다.

잘 생각해보면, 서비스의 로직 서버와 DB 서버 역시 별도록 분리되어 있습니다. 각 서버에 DB 서버를 모두 올릴 수도 있는데, 이러면, 각 DB 서버의 동기화가 필요해지겠죠.(물론, 이런식으로 하는 서비스 구조도 있습니다. 주로 읽기만 많은 서비스이며, 그 데이터의 동기화가 덜 중요할 경우…)

그래서 결국 Redis와 Memcached 를 쓰는 것은 위의 여러 가지 장점을 취하기 위해서입니다. 물론 데이터 량이 늘어나면, 이 서버들도 여러 대가 필요해지고, 이를 위해 데이터를 어떻게 찾을 지에 대한 룰도 추가로 필요해지게 됩니다.


## 언제 캐시를 사용하는가?

사용 안한다면...
CPU나 램을 확장 or 서버 장비를 더 늘리자.. -> 돈이 든다.
그리고 달리는 열차 바퀴 갈아끼우기, 잘 서비스되고 있는 서버에 영향을 준다. 

그냥 읽기도 유저 수가 많으면 느려져
DB 인덱스 걸어도 한계..

로그도 디비에 저장할 때, 캐시를 써서 모아쓰기를 하면 좋다.

그럼 캐시서버는 어디에 둘까?
보통은 비지니스 로직과 디비 사이에 둔다.


자주 찾을 것 같은 데이터를 캐싱
페북이라면? -> 로그인 위한 유저 정보, 첫 화면 보여줄 피드 몇백 , 친구관계 등 

그래도.. 유저 정보가 많아지면..용량이 커지고 .. 한 메모리에 다 올릴 수 없다. 
-> 캐시의 분배가 필요

어떻게 분배?
캐시 서버가 1대라면.. LRU등으로 자주 쓰는 녀석들만 조금

캐시서버가 여러대면 ?
- range
- modulo
- consistent hash
- indexed

서버의 목록은 zookeeper등으로 관리하면 좋음

---
Cache의 대상이 되는 정보들
단순한, 또는 단순한 구조의 정보를 -> 정보의 단순성 
반복적으로 동일하게 제공해야 하거나 -> 빈번한 동일요청의  반복
정보의 변경주기가 빈번하지 않고, 단위처리 시간이 오래걸리는 정보이고 -> 높은 단위처리비용
정보의 최신화가 반드시 실시간으로 이뤄지지 않아도 서비스 품질에 영향을 거의 주지 않는 정보 
더 많은 조건들이 있겠으나,
저 조건들 중 2개이상 포함되는 성격의 서비스와 정보라면
Cache를 적용하는 것을 적극적으로 고려해 보아도 큰 무리가 없을 것 같다.

어떤 정보들을 Cache로 사용하나?
포탈의 검색어
쇼핑몰의 핫딜상품, 베스트셀러, 추천상품등
상품의 카테고리와 카테고리별 등록상품 수
방문자수, 조회수, 추천수
1회성 인증정보 (SMS 본인인증정보, IP정보등)
공지사항, Q&A


출처: https://yonguri.tistory.com/82 [대디장의 기억저장소]




## 이전 팀의 hazelcast는 어떻게 동작하는거지?
추상화된 @Cachable을 그냥 가져다 쓰는 거면, hazelcast는 어떻게 동작하는가?
https://yonguri.tistory.com/82

- 의존성을 추가하고 설하고 구현체를 사용 가능하다

의존성 없는 기본 캐시는?


## 참고 
https://charsyam.wordpress.com/2016/07/27/%EC%9E%85-%EA%B0%9C%EB%B0%9C-%EC%99%9C-cache%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80/


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
하는 일은 비슷하나 pakacage 가 다르니 약간 다르다.


spring - @Cachable
method단위, method의 내용이 캐싱되어있으면 ok 아니면 pass
### 기본으로 지정된 @Cachable 의 구현체는 무엇인가? 그리고 2차 캐시인가?

JPA - @Cachable
JPA를 구현한게 하이버네이트라면..@Cache를 사용해 세밀한 설정을 할 수도 있다.
하이버네이트를 포함한 대부분의 JPA 구현체들은 애플리케이션 범위의 캐시를 지원하는데 이것을 공유 캐시 또는 2차 캐시라 한다.
2차 캐시, 

1차캐시는 우리가 컨트롤 하는게 아님. 트랜젝션 단위.


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




