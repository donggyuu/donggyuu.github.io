---
title:  "Run Nginx in a Docker"
excerpt: "Initial setup of Nginx using Docker"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/run_nginx_docker.png
categories:
  - Development
tags:
  - Setup
  - Docker
last_modified_at: 2020-04-01T08:06:00-05:00
---
<!--TODO
1. 원인 파악 : Errors - 403 Forbidden　
2. 이미지 수정 : teaser.png 
3. indentaion수정 : --name : set name 부분
-->

# Pull Nginx Image
```bash
# pull latest image
sudo docker pull nginx:latest

# check
docker images
# ----------------
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
docker.io/nginx             latest              6678c7c2e56c        a moment ago        127 MB
```
<br>

# Make Initial page
```bash
# directory for volume
mkdir -p /home/mint/share/nginx/html
# sudo mkdir -p /home/mint/share/nginx/html

cd /home/mint/share/nginx/html; pwd

vi index.html
# ----------------
<p> hello Nginx </p>
```
<br>

# Run Docker
```bash
# run Docker
docker run --name nginx_test -v /home/mint/share/nginx/html:/usr/share/nginx/html:ro -d -p 8080:80  nginx

# check
docker ps
# ----------------
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
a802b7601ad1        nginx               "nginx -g 'daemon ..."   2 seconds ago       Up 1 second         0.0.0.0:8080->80/tcp   nginx_test
```

--name : set name of docker container
-v : mount "/home/mint/share/nginx/html" in local with "/usr/share/nginx/html" in Docker
-d : run docker in background
-p : set port(8080:local, 80:nginx container)


<br>


# Confirmation
```bash
# access to below from Browsers
http://xx.xxx.xxx.xxx:8080/
# ----------------
hello Nginx
```
<br>

# ※ Errors - 403 Forbidden
```bash
# 作成中
```



403이 안되어서 그냥 -v옵션은 안 쓰는걸로.
그냥 도커 container안으로 직접 들어가서 일단 해보는것으로. 


docker run --name nginx_test -v /home/mint/share/nginx/html:/usr/share/nginx/html:ro -d -p 8080:80  nginx


docker run --name nginx -p 8080:80 nginx
// 그냥 요렇게만 하면 background 실행이 아니라 ctrl + d 하면 container 가 종료됨. 

// -d 옵션을 붙여서 background 실행
docker run --name nginx -d -p 8080:80 nginx

// 만약 그냥 서버에 설치한다면
// sudo apt-get install nginx
// aws는 RH-based의 centOS니까 yum을 사용
https://stackoverflow.com/questions/32592956/apt-get-command-not-found

// aws의 centOS에서 nginx 설치
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7

/--- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- 
컨테이너 실행과, 컨테이너 안으로 들어가기.
https://ossian.tistory.com/46

// 안으로 들어가기.
docker exec -it nginx bin/bash

// index.html을 검색. 
root@093ac34c074b:/# find / -name 'index.html'

// 아마 들어가면 vi가 설치가 안 되어있어서 설치해야..
apt-get update
apt-get install vim
https://stackoverflow.com/questions/31515863/how-to-run-vi-on-docker-container
// 출처.

// sudo 도 설치해야...
apt-get install sudo -y
// install check
which sudo

// nginx의 default파일 만들기
sudo vim /etc/nginx/sites-available/default
// 이러면 권한 문제임...
// https://unix.stackexchange.com/questions/401203/why-root-cant-open-file-for-writing
// 옵션을 주어서 vi를 하거나, mkdir 를 한 다음 만들어야 함
mkdir -p /etc/nginx/sites-available/
vi default
/--- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- 

여기부터는 그냥 서버에 설치된 nginx로 하기.

mkdir -p /etc/nginx/sites-available/
vi default

// ec2 <---> 로컬 : 파일 전송하기.
https://www.sallys.space/blog/2017/11/28/aws-scp/
#파일 전송시
scp -i [pem file] [upload file] [user id]@[ec2 public IP]:~/[transfer address]
#예시
scp -i Desktop/amazon/juhyung.pem Desktop/pant.py ubuntu@~~~~:~/
#폴더 전송시
scp -i [pem file] -r [upload folder] [user id]@[ec2 public IP]:~/[transfer address]
#예시
scp -i Desktop/amazon/juhyung.pem -r Desktop/example ubuntu@~~~~:~/

// 나의 예시.. 
sudo scp -i /Users/donggyu/Dropbox/git/aws_keypair.pem -r /Users/donggyu/Dropbox/git/is-fukushima/build centos@13.231.208.104:/home/centos/is-fukushima/

// permission denied난다.
// 권한을 줘야..  http://miner.hatenablog.com/entry/7
sudo chmod -R 777 /home/user/myapp/build


// path가없으면 만든다.
[centos@ip-172-26-8-226 sites-available]$ sudo mkdir -p /etc/nginx/sites-enabled/
[centos@ip-172-26-8-226 sites-available]$ touch /etc/nginx/sites-enabled/myapp.conf


// mkdir -p 
mkdir -p /etc/nginx/sites-enabled
sudo ln -s /etc/nginx/sites-available/myapp.conf /etc/nginx/sites-enabled/myapp.conf

sudo ln -s /etc/nginx/sites-available/is-fukushima.conf /etc/nginx/sites-enabled/is-fukushima.conf

sudo systemctl stop nginx
sudo systemctl start nginx
다 했는데 왜 안돌아가징?
// 아마 권한 문제인듯.. 해당 build가 있는 파일에, 외부의 접근 권한이 없어서?
// 기본포으 80을 안 연 것이 아 닐까?-> 그건 아님 : 
netstat -ntpa | grep 80



docker 로만 하는 방법. 
https://codechacha.com/ko/dockerizing-react-with-nginx/


// 없으면 설치해야..
yum install lsof

// 요거도 아니었음
https://stackoverflow.com/questions/16021481/nginx-not-listening-to-port-80

// include /etc/nginx/sites-enabled/*; 요거도 아님.. 
https://serverfault.com/questions/563494/nginx-is-running-but-not-serving

// nginx -t -c /etc/nginx/nginx.conf 이 명령어로 인한 권한 문제가 아닐까?
run/nginx.pid" failed (13: Permission denied)


//아래의 에러가 나오면
nginx.service: Failed to read PID from file /run/nginx.pid: Invalid argument

// https://stackoverflow.com/questions/42078674/nginx-service-failed-to-read-pid-from-file-run-nginx-pid-invalid-argument
여기를 봐서 해결
sudo su
printf "[Service]\nExecStartPost=/bin/sleep 0.1\n" > /etc/systemd/system/nginx.service.d/override.conf
systemctl daemon-reload
systemctl restart nginx 

근데 81포트로 바꿔봐도 접속이 안돤다..

왜 안되지...
해외 사이트를 찾아서 다시 해보기.

오늘 해보니까... 애초에 포트로 접속이 안되는구나...  aws의 설정 문제인가?
방화벽 설정의 문제인가..? 
Nginx의 default페이지 설정부터 봐야 할듯..

방화벽 설정을 직접 해줘야하나?
https://gsk121.tistory.com/389
[root@ip-172-26-8-226 sites-enabled]# firewall-cmd --permanent --zone=public --add-service=http
success

방화벽을 열었는데도 왜 안되지?
nginx.conf파일에 ,, file-allow를 해줘야 하는건가? 
애초에 80아닌 81번 열먼 또 적용이 안됨...


https://openshortcuts.com/archives/1274
여기로도메임 등록함. 

https://codeburst.io/how-to-setup-nginx-for-react-a504f38f95ed
설정은 여기를 참고함. 

뭐가문제지 잘되네.. 조금은 시간이 걸릴수도 있나봄.
nginx -t
