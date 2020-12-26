---
title: "CentOS에서 Selenium설정하고 실행하기"
excerpt: "CentOS 7에서 Selenium 설정 및 발생 가능 에러 정리"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/install-selenium-on-centos.png
categories:
  - Development 
tags:
  - Java
last_modified_at: 2020-12-07T08:06:00-05:00
published: false
---

spring hateoas

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




