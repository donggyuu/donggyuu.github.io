---
title: "Spring에서 HATEOAS 구현하기"
excerpt: "성숙도 모델, HATEOAS의 개념과 Spring에서의 구현"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/spring-hateoas.png
categories:
  - Development 
tags:
  - Java
  - Spring
last_modified_at: 2021-01-07T08:06:00-05:00
published: true
---
## HATEOAS란?
REST는 어떠한 기술 표준이 아닌, HTTP에 대한 제약 사항이라고 볼 수 있습니다. 따라서 REST API가 어느정도의 REST원칙을 준수하는지는 저마다 다릅니다.   

때문에 리차드슨은 성숙도 모델을 통해 API가 어느정도 REST원칙을 지키고 있는지 등급으로 나타내고자 했습니다.  
- **레벨0 : POX(Plain Old XML)**   
- **레벨1 : 자원**   
관련된 명사를 구별하기 위해 여러개의 URI를 사용하는 API를 구현했지만 HTTP의 다른 기능은 사용하지 않음
- **레벨2 : HTTP동사**    
HTTP의 전송 기능으로서의 특성(HTTP동사,상태 코드 등)을 이용한 API를 구현
- **레벨3 : 하이퍼미디어컨트롤**   
서비스가 제공하는 자원에 접근하기 위해 아무런 사전지식도 요구하지 않는 수준의 API를 구현

성숙도 모델의 레벨3에 해당하는 개념이 HATEOAS입니다. 클라이언트가 어떤 요청을 할때, 해당 요청과 함께 해당 요청에 관련된 URI도 같이 응답해 주는 것을 말합니다. 

쉽게 요약하면  
유저가 POST요청을 할때 그 응답으로 조회, 수정, 삭제하는 엔드포인트도 함께 보내 유저가 API문서를 읽지 않고도 POST이외의 요청을 알 수 있도록 하는 것을 말합니다.


## Spring boot에 HATEOAS 도입
간단한 api 작성을 통해 spring에서 hateoas를 도입해 보겠습니다.  
[전체 코드는 여기에서 확인할 수 있습니다.](https://github.com/donggyuu/spring-basic/blob/master/restapi/src/main/java/com/example/restapi/controller/BoardController.java#L57)

### Dependency설정
maven을 사용할 경우 아래와 같이 의존성 추가를 해줍니다.
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```

### Code작성
Controller에서 응답을 반환할때 hateoas 설정을 추가해줍니다.   

```java
public ResponseEntity createBoard(@RequestBody @Validated CreateBoardParam param) {
    
    ...생략...

    EntityModel<Board> entityModel = EntityModel.of(
            board,
            linkTo(BoardController.class).slash(board.getSequence()).withSelfRel(),
            linkTo(BoardController.class).slash(board.getSequence()).withRel("get"),
            linkTo(BoardController.class).slash(board.getSequence()).withRel("delete"),
            linkTo(BoardController.class).slash(board.getSequence()).withRel("edit"));

    return ResponseEntity.created(uri).body(entityModel);
}
```
EntityModel을 반환하기 전에 안에 관련된 URI를 넣어줍니다.  
createBoard는 controller안의 method로, 클라이언트에서 게시판 생성을 요청하면 실행되는 method입니다.   

게시판을 생성하려고 요청을 보내는 클라이언트가 게시판을 조회(get), 삭제(delete), 수정(edit)하는 URI도 알고 있다면 API사용 문서가 없어도 추후 생성 이외의 다른 기능을 이용할 수 있을 것입니다. 따라서 다른 URI도 같이 응답을 돌려줍니다.

### Test
PostMan에서 확인해보겠습니다.  
![spring-hateoas-2](/assets/images/spring-hateoas-2.png)

위 그림에서는 POST로 userName과 contnet를 게시판에 삽입하는 요청을 보냅니다.  

아래 그림에서는 게시판의 4번째 요소로 추가되었다는 응답을 확인할 수 있습니다. 아울러 hateoas도 추가하였기 때문에 조회, 삭제, 수정을 위한 URI가 "boards/4"임도 같이 응답해 주었습니다.  
