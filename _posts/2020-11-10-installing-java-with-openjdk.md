---
title:  "Installing Java with OpenJDK"
excerpt: "Install OpenJDK with commandline"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/java_with_openjdk.png
categories:
  - Development
tags:
  - Development-Skills
last_modified_at: 2020-11-10T08:06:00-05:00
published: true
---

## Why use OpenJDK for Java?
There are two options for installing Java on your Mac.  
- Oracle JDK
- OpenJDK  

Either is good choice only for your local environment but for other cases, OpenJDK can be better because it free. Oracle said "Non-General Purpose Computing" can be chared


## Download OpenJDK
Go to http://jdk.java.net/archive/ and download version for Mac what you want. I got version "12.0.2".  
![OpenJDK_site](/assets/images/java_with_openjdk.png)


## Installation
```bash
# go to path for OpenJDK
# usually in Downloads
-------------------------------
mac:Downloads don$ pwd; ls -la | grep "jdk"
/Users/don/Downloads
-rw-r--r--@  1 don  staff  190273787 Oct 23 19:05 openjdk-12.0.2_osx-x64_bin.tar.gz


# extract JDK file
# tar xf [your own version of OpenJDK]
tar xf openjdk-12.0.2_osx-x64_bin.tar.gz
-------------------------------
mac:Downloads don$ tar xf openjdk-12.0.2_osx-x64_bin.tar.gz
  
mac:Downloads don$ ls -la | grep "jdk"
drwxr-xr-x@  3 don  staff         96 Jul 16  2019 jdk-12.0.2.jdk
-rw-r--r--@  1 don  staff  190273787 Oct 23 19:05 openjdk-12.0.2_osx-x64_bin.tar.gz


# move path to under Java
# sudo mv [extracted_jdk_file] [java_path]
sudo mv jdk-12.0.2.jdk /Library/Java/JavaVirtualMachines/


# check 
java -version  
-------------------------------  
mac:Downloads don$ java -version
openjdk version "12.0.2" 2019-07-16
OpenJDK Runtime Environment (build 12.0.2+10)
OpenJDK 64-Bit Server VM (build 12.0.2+10, mixed mode, sharing)
```