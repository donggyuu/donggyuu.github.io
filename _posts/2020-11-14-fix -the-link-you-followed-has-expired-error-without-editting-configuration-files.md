---
title: "Fix 'The Link You Followed Has Expired' Error Without Editting Configuration Files"
excerpt: "Fix Link Expired Error only use scp commandline"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/error_link_has_expired.png
categories:
  - Development
tags:
  - Error 
last_modified_at: 2020-11-14T08:06:00-05:00
---

## When does the error occur?
"The Link You Followed Has Expired" error occur when you try to uplaod files which have bigger size than configured setup on your serverside file. 
In my case, I try to upload my newspaper theme file(.zip) but faced an error.
![error_link_has_expired](/assets/images/error_link_has_expired.png)


## Why they way of editting files is not good
Many other pages said editting htaccess file or something like that. I don't feel like doing that for I do not want to modify default setting of files. Modifying defaults always have chances of making trouble. And some people might have no right to modify files on server side.  

Please refer to following page If you want to fix this error by editting files.  
https://www.wpbeginner.com/wp-tutorials/how-to-fix-the-link-you-followed-has-expired-error-in-wordpress/


## Fix error using scp
Upload your file by yourself using scp commandline as below. I use Amazon AWS so added option for using Amazon key pairs.
```bash
# ------------------
# format
# ------------------
sudo scp -i [EC2_key_pair_path] -r [your_file_path] [server_user_name]@[server_upload_path]

## [EC2_key_pair_path] using key pair of amazon EC2
## [your_file_path] files you want to upload
## [server_user_name] user name of your wordpress server
## [server_upload_path] paht you want to upload your file

# ------------------
# usage
# ------------------
sudo scp -i /Users/don/xxx/xxx/aws_keypair.pem -r /Users/don/Downloads/NewspaperTheme bitnami@11.111.111.110:/home/bitnami/apps/wordpress/htdocs/wp-content/themes/
```
