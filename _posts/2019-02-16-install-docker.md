---
layout: post
title: "setup lightsail for test server"
description: "setup lightsail"
category: articles
tags: [test, server]
comments: true
---

이미지 가져와서 쓰는 것 까지는 했음.
내가 이미지 만드는 방법을 나중에 포스팅을 ㅗ알아야!! 

TODO : docker 설치, jenkins설치
-->왜 jenkins따로 설치 안하는가 ? 이유 설명



https://docs.docker.com/install/linux/docker-ce/centos/
공식 doc

docker install 
using AWS / Centos  + and add jenkins

Q : install -> docker  설치위치를 usr/ lib 정도에 ? 


커맨드로 설치후 아래 에러
Docker can't connect to docker daemon
--> 다시 재시작하면 됨.
sudo service docker stop  
sudo service docker start
이후에, sudo docker version  /// 으로 client server 둘 다 출력을 확인
--> Q : 리눅스 유저에게 권한 주는 방법 알기. 

https://www.leafcats.com/215
여기를 참고해서 jenkins를 설치.  


---설치

https://docs.docker.com/install/linux/docker-ce/centos/
여기 접속해서 (구글에는 docker for centos 검색  

// 먼저 pository를 세팅해야 하는듯.  
Install using the repository
이 부분의 
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  를 설치. 

  Use the following command to set up the stable repository.
의 커맨드를 설치. 

sudo yum install docker-ce docker-ce-cli containerd.io
레포지토리를 설정했으면 위으 ㅣ명령어로 무료버전인 docker ce를 설치. 


설치 끝나면 아래 명령어로 도커 start
sudo systemctl start docker

시스템 부팅하면 도커도 같이 시작하도록 세팅

sudo systemctl enable docker

이상태로 docker ps 하면 안 먹으ㄴ니까 , 접속 유저에게 권한 주기
sudo usermod -aG docker centos
(유저 이름은 여기서 centos )
whoami로 확인하면 유저 확인 가능. 
이후 안되면 리부팅. 

이후 docker compose도 설치를 해주어야 한다.  
what is docker compose?
http://avilos.codes/infra-management/virtualization-platform/docker/docker-compose/
여기를 참고  


https://docs.docker.com/compose/install/
여기서 설치한다. 
커맨드 2개 (1. 설치 , 2. 실행파일 권한 주기.)

이후 커맨드에서 docker-compose 처보면 나온다. 


다음엔 docker설치하는 곳에 jenkins를 install 


구글에 docker jenkins image 검색
들어가서 커맨드를 복붙 실행..

sudo docker pull jenkins

다운받은게 어디 위치인지? 커맨드에 docker images치면, 다운받은 이미지 확인 가능. 

이즘에서 , 도커가 어디에 설치되어 있는지?
docker info | grep -i root 
// -i는 대소문자를 구분하지 않음..  


- /home 디렉토리의 전체 사용량을 MB단위로 출력
du -sh /home 

sudo du -sh /var/lib/docker
방금 받은 이미지를 포함 얼마나 도커에서 용량을 차지하는지 알 수 있따.  

------여기까지 설치, 아래부터는 compose 파일 세팅 등등 ----


compose를 세팅하기 위해
home/jenkins/jenkins-data
디렉토리를 만든다.  디렉토리로 들어가서
vi docker-compose.yml


여기 를 참고해서
https://heechan.me/posts/how-to-use-docker-compose/
docker-compose의 의미와 그 세팅을 하자. 


volumes:
      - "$PWD/jenkins_home:/var/jenkins_home"

이건, docker-compose.yml 과 같은 위치에 jenkins_home 폴더를 만들고,
위의 의미는, jenkins를 실행해서 등등 발생한 것은 모두 이 jenkins_home에 넣겠따는 의미. 

sudo chown 1000:1000 jenkins_home -R
이짓을 왜 하는지? 

스샷을 참고.. 

// 중간의 세세한 설정들 -> 검색하서 이해하고 넘어가기. 

이렇게 세팅을 하고
[centos@ip-172-26-8-226 jenkins-data]$ ls
docker-compose.yml  jenkins_home
[centos@ip-172-26-8-226 jenkins-data]$ docker-compose up -d
로 docker-composeyml을 적용하기.   


[centos@ip-172-26-8-226 jenkins-data]$ docker logs -f jenkins
이거 치면 얼추 실행되는듯. 근데 이건 로그 보는거 같은데 왜?? 
// 여기에 passowrd가 표시됨. 
여기서 jenkins는, .yml파일에 써져 있는 container_name임. 

// 자기 포트번호 : 8080 들어가서, 비밀번호 임력하고 초기 세팅 진행하기.. 


//강의에서는 dns 파일 이름 따로 그냥 로컬로 설정하던데 이거 안 사도 되나? 


이렇게 세팅된 도커의 젠킨스를 키고 끄는 명령어
// docker -compose가 세팅된 디렉토리로 이동해서.
docker-compose stop
docker-compose star
// 재시작
docker-compose restart jenkins

// docker-compose down 
도커 전부 끄는 듯. 

여기까지 하고 jenkins_home에 들어가보면, 
젠킨스 실행에 관련된 모든파일이 저장되어 ㅣㅆ는 것을 볼 ㅅ ㅜ있음. 