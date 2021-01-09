---
title: "도커(docker)란 무엇인가"
excerpt: "docker의 기본 잡기 - 사용이유와 생명주기"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/why-use-docker.png
categories:
  - Development 
tags:
  - dev_Others
last_modified_at: 2021-01-04T08:06:00-05:00
published: false
---

## docker를 사용하는 이유
인터넷에서 프로그램을 다운받아 설치하는 경우, 서버, 패키지 버전, OS 등에 따라 설치 과정 중 많은 오류가 발생할 수 있습니다. 분명 블로그를 보고 MySQL이나 Redis 등을 설치하려 했는데 잘 되지 않는 경우가 많죠.   

docker는 container의 개념으로 위의 문제를 해결합니다. 프로그램과 그것의 실행 환경, 코드의 종속성 등을 container라는 일종의 박스 단위로 추상화합니다. container안에 프로그램을 실행하는데 필요한 모든 구성들을 넣고, 이렇게 함으로써  프로그램의 배포 및 관리를 단순하게 하고 어떤 환경에서도 잘 돌도록 할 수 있는 것입니다.    

"사진"

한마디로 도커란, container기반으로 가상화 플랫폼을 사용, application을 더 쉽게 만들고 배포, 실행 및 운영할 수 있도록 설계된 일종의 도구입니다. 



## 이미지와 컨테이너의 개념 
이미지에는 명령어와 스냅샷이 들어간다 
이미지를 가지도커를 실행하면 container가 다음과 같이 만들어지고 container를 사용 간으 .


사진 : docker #1 의 "도커 이미지와 도커 컨테이너의 정의"

docker image를 사용해 container를 생성, docker container로 프로그램을 실행한다. 




## 이미지로 컨테이너 만들기
이미지 : 실행될 명령어 + 파일 스냅샷
docker run hello-world
> container안의 hard-disk 에 hello-world설치파일을 넣음
container안에 명령어(run hello-world)를 넣음


docker version 입력해보면 linux가 나온다.
왜? container안에는 linux vm이 되어 있다.
내 컴은 mac이지만 docker환경 자체는 linux, linux커널 기반, 그래서 Cgroup, name-space를 사용가능해서 container를 나누기가 가능했다.


** docker run vs exec
```bash
mac:~ don$ docker ps
CONTAINER ID   IMAGE     COMMAND            CREATED          STATUS          PORTS     NAMES
41c7b9d0af92   alpine    "ping localhost"   16 seconds ago   Up 15 seconds             wonderful_panini
mac:~ don$
mac:~ don$ docker exec 41c7b9d0af92 ls
```


## docker의 생명주기


### create, start, running

### stopped, deleted

