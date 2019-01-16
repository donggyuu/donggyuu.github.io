---
layout: post
title: "basic commandline"
description: "summarize basic commandline"
category: articles
tags: [linux]
comments: true
---

## what is dns cache?  
* Java VM은 DNS lookup시 TTL(time-to-live)을 쓰지 않고, 한 번 lookup한 도메인 이름은 VM이 내려갈때까지 계속 캐싱을 함(DNS Spoofing을 막기위해)   
* JVM의 버전에따라 다르지만, 1.6 이전의 경우는 Default로 Forever 1.6 이후 버전에 대해서는 30초 DNS Cache를 함 
* SecurityManager가 존재 할 경우는 Application이 켜진동안 Cache를 만료시키지 않고, SecurityManager가 존재하지 않을경우, 30초간 Cache가 default    


command for git
* git pull --ff-only


command for sql
* cat check.sql | /usr/local/mysql/bin/mysql -h ${DB_MASTER_SERVER} -u appdbadm -P 3301 ${DB_NAME} -p${DB_PASS} |& tee ${DB_MASTER_SERVER}_${DB_NAME}_before.log