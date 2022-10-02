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
  - Development
last_modified_at: 2020-04-01T08:06:00-05:00
published: false
---
<!--TODO
1. 원인 파악 : Errors - 403 Forbidden　
OK 2. 이미지 수정 : teaser.png 
OK 3. indentaion수정 : --name : set name 부분
-->

# 1.Pull Nginx Image
```bash
# pull latest image
sudo docker pull nginx:latest

# check
docker images
# ----------------
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
docker.io/nginx             latest              6678c7c2e56c        a moment ago        127 MB
```

# 2.Make Initial page
```bash
# directory for volume
mkdir -p /home/mint/share/nginx/html
# sudo mkdir -p /home/mint/share/nginx/html

cd /home/mint/share/nginx/html; pwd

vi index.html
# ----------------
<p> hello Nginx </p>
```

# 3.Run Docker
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


# 4.Confirmation
```bash
# access to below from Browsers
http://xx.xxx.xxx.xxx:8080/
# ----------------
hello Nginx
```
