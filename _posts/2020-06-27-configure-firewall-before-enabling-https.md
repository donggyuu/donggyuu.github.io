---
title:  "Configure a Firewall Before Enabling https on Nginx"
excerpt: "Check Firewall if SSL Certificatino doesn't works"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/firewall_erro_pages.png
categories:
  - Development
tags:
  - Setup
  - Linux
last_modified_at: 2020-06-27T08:06:00-05:00
published : true
---

# Overview
I try to Enable https by "Let's Encrypt" on by CentOS server but cannot access to https on browser with "Unable to connect".    
![firewall_erro_pages.png](/assets/images/firewall_erro_pages.png)
※ environment  
-CentOS 7 (with amazon lightsail)  
-Nginx 1.16.1  
※ Manual for "Let's Encrypt" setup  
-https://qiita.com/HeRo/items/f9eb8d8a08d4d5b63ee9

In this case, We need to configure a firewall first before SSL certification setup

<br>
  
  
# Configure a Firewall for Lightsail
Go to "amazon lightsail" console -> Networking -> Firewall -> Add rule -> Add HTTPS. User can access AWS instance using port 443(HTTPS).
![aws_instance_port 443](/assets/images/aws_instance_port_443.png)

<br>

# Configure a Firewall for CentOS 7
From CentOS 7, we can use "firewalld" instead of "iptables". Open https using firewalld.  
```bash
# open http
sudo firewall-cmd --add-service=http
-------------------------------------
Warning: ALREADY_ENABLED: 'http' already in 'public'
success
  
# open https
sudo firewall-cmd --add-service=https
-------------------------------------
success
```
  
# References
https://qiita.com/HeRo/items/f9eb8d8a08d4d5b63ee9  
https://qiita.com/kenjjiijjii/items/1057af2dddc34022b09e  
https://heni.tistory.com/24