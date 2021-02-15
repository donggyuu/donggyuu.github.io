---
title: "도커(docker)란 무엇인가"
excerpt: "docker의 기본 잡기 - 사용이유와 생명주기"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/why-use-docker-3.png
categories:
  - Development 
tags:
  - Basic
last_modified_at: 2021-01-04T08:06:00-05:00
published: true
---
<script src="https://ads-partners.coupang.com/g.js"></script>
<script>
	new PartnersCoupang.G({ id:368772 });
</script>  
<br>

## docker를 사용하는 이유
인터넷에서 프로그램을 다운받아 설치하는 경우, 서버, 패키지 버전, OS 등에 따라 설치 과정 중 많은 오류가 발생할 수 있습니다. 분명 블로그를 보고 MySQL이나 Redis 등을 설치하려 했는데 잘 되지 않는 경우가 많죠.   

docker는 container의 개념으로 위의 문제를 해결합니다. 프로그램과 그것의 실행 환경, 코드의 종속성 등을 container라는 일종의 박스 단위로 추상화합니다. container안에 프로그램을 실행하는데 필요한 모든 구성들을 넣고, 이렇게 함으로써  프로그램의 배포 및 관리를 단순하게 하고 어떤 환경에서도 잘 돌도록 할 수 있는 것입니다.     

한마디로 도커란, container기반으로 가상화 플랫폼을 사용, application을 더 쉽게 만들고 배포, 실행 및 운영할 수 있도록 설계된 일종의 도구입니다. 



## 이미지와 컨테이너의 개념 
image는 container를 만드는데 필요한 일종의 설계서라고 할 수 있습니다. 

이미지에는 아래의 2가지가 들어있습니다.
- container시작시 실행될 명령어 
- 파일 스냅샷

hello-world 이미지를 예로 들면,  
이미지로부터 아래와 같이 container를 만들 수 있습니다.
```bash
docker run hello-world
```
그림으로 그려보면 아래와 같이 hello-world container가 만들어지고 실행된 상태가 됩니다.    

![why-use-docker-3](/assets/images/why-use-docker-3.png)



## docker의 생명주기
docker-container의 생명 주기는 위에서 아래로 다음과 같습니다.  
- create
- start
- running
- stopped
- deleted

![why-use-docker-4](/assets/images/why-use-docker-4.png)

### create, start, running

**create**   
create는 image를 바탕으로 container를 생성만 하고 실행은 하지 않습니다. image의 파일 스냅샷이 container의 하드 디스크에 올라간 상태입니다.  
```bash
docker create "이미지 이름"
--------------------------
# container가 생성되어 고유 hash-value가 출력된 모습
mac:donggyuu.github.io don$ docker create hello-world
8d0e9a06aa06eee6ca4beadb5ac1f50fc0dfdafaa29c1deea9321a51d851faba
```
**start**   
create로 container를 생성했다면 start로 실행할 수 있습니다. image에 있는 "시작 시 실행될 명령어"가 container에 올라가는 시점입니다.  
```bash
docker start "컨테이너 아이디 or 이름"
--------------------------
# container의 아이디(hash-value)로 start
mac:donggyuu.github.io don$ docker start 8d0e9a06aa06
```

**run**  
run은 "create+start"입니다.
```bash
docker run "이미지 이름"
--------------------------
docker run hello-world
```


### stopped, deleted
**stopped**  
container를 멈추는 명령어는 2가지가 있습니다.  
```bash
# 방법1
docker stop "중지할 컨테이너 아이디 or 이름"

# 방법2
docker kill "중지할 컨테이너 아이디 or 이름"
```
stop은 진행중인 작업이 있으면 해당 작업까지 완료하고 container를 멈춥니다(graceful하게 멈춤). 

반면 kill은 진행중인 작업과 상관없이 즉시 container를 멈춥니다. 

**deleted**  
conatainer를 삭제하기 전에 먼저 중지를 해야 합니다. 중지 후에는 아래 명령어로 삭제할 수 있습니다. 
```bash
docker rm "삭제할 컨테이너 아이디 or 이름"
```