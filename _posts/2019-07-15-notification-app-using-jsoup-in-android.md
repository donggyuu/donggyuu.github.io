---
title: "Notification App Using Jsoup in Android"
excerpt: "Developing and Releasing Android App mainly using Jsoup library"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/notification-app-1.png
categories:
  - Development 
tags:
  - Java
  - Android
last_modified_at: 2019-07-15T08:06:00-05:00
published: true
---

## Summary
Have troubled in usual maintenance with short notice in my favorite game. This application get notice information from game site in every 5 min, then display it on List. Mainly use **Jsoup** and **AsyncTask** class.
  
## UI Design
Show List of notices which will be updated every 5 min. Move to game-notice page if touch the each list. Push alarm if there were any updates. If not, nothing to be updated.  
![notification-app-2](/assets/images/notification-app-2.png)

## Development
Can check all sources in [here](https://github.com/donggyuu/game-notice-app-tera).

### What is Jsoup?
Parsing library on Java side. Can parsing HTML without using regex(but also can use regex). I use this getting notice from game site.  
reference : https://jsoup.org/

### Get titles and links using Jsoup
Should read this first for more details of using Jsoup from [here.](https://donggyulee.blogspot.com/2019/05/jsoup-parsing-library-for-java.html)   

getUrlConnection method is return Document object which contains information of designated site. Set basic connection setting using Jsoup then get Document object by get().    

getNoticeTitles method extract title information from Document derived from getUrlConnection method. Extract select method from Jsoup for getting titles only.    


### Why use AsyncTask?
 Android system make main thread when start application. This main thread access to UI tool kit and handle tasks like waiting input by users or draw images on screen. So main thread also called as UI thread.   

 Android system execute all components of application in same thread. The point is, UI thread is not thread-safe. For example, screen cannot be updated if some tasks like "download many files" not finished. This kinds of things like network connection cannot be done in main thread in Android.   

 So using AsyncTask for background process. AsyncTask execute background thread as asynchronous so main thread can their work with AsyncTask do its task.   
[-> reference](https://webnautes.tistory.com/1082)  

### Execute Jsoup using AsyncTask
Parsing by Jsoup occur via internet connections so have to use AsyncTask. We need to write 2 method to override for using AsyncTask - doInBackground and onPostExecute.   

In **doInBackground**, write code Asynchronized with MainActivity class. I write getNoticeTitles method used Jsoup for getting notice titles.   

**onPostExecute**, write code which occurred after doInBackground was executed. I write showNoticeTitleOnScreen and this method get notice title then show it on screen.  


## Releasing App
Can download from play store. About 14 users installed it in 07-06, 2019.  
[download from google play](https://play.google.com/store/apps/details?id=com.lee.donggyu.gamenoticeapptera&hl=ko)    
![notification-app-1](/assets/images/notification-app-1.png)
