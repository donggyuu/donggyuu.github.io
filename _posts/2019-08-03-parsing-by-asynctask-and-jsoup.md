---
layout: post
title: "Parsing by AsyncTask and Jsoup"
description: "how to make crawlign app in android"
category: articles
tags: [Java, Parsing, Android]
comments: true
---

---  

<!-- contents -->
- [Overview](#overview)
  - [Summary](#summary)
  - [UI Design](#ui_design)
- [Development](#development)
  - [Part1. Jsoup for parsing](#jsoup_for_parsing)
  - [Part2. AsyncTask for using network in Android](#asynctask_for_using_network_in_android)
- [Released App](#released_app)

---  
Can check all sources in here : https://github.com/donggyuu/game-notice-app-tera

<div id='overview'/>
# Overview
<div id='summary'/>
## Summary

Have troubled in usual maintenance with short notice in my favorite game. This application get notice information from game site in every 5 min, then display it on List. Mainly use Jsoup and AsyncTask class.

<div id='ui_design'/>
## UI Design
![search_user_agent]({{ "/assets/img/jsoup_user_agent_20190725.png" | absolute_url }})
Show List of notices which will be updated every 5 min. Move to game-notice page if touch the each list. Push alarm if there were any updates. If not, nothing to be updated.

<div id='development'/>
# Development
<div id='jsoup_for_parsing'/>
## Part1. Jsoup for parsing
### What is Jsoup?
Parsing library on Java side. Can parsing HTML without using regex(but also can use regex). I use this getting notice from game site.  
reference : [https://jsoup.org/](https://jsoup.org/)

### Get titles and links using Jsoup
Should read this first for more details of using Jsoup   
[https://donggyulee.blogspot.com/2019/05/jsoup-parsing-library-for-java.html](https://donggyulee.blogspot.com/2019/05/jsoup-parsing-library-for-java.html)   

**getUrlConnection** method is return Document object which contains information of designated site. Set basic connection setting using Jsoup then get Document object by get().
```
private Document getUrlConnection(String url) throws IOException {

        // SET url what you want - notice board page of game site
        String connUrl = url;
        String USER_AGENT = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.152 Safari/537.36";

        // make connection using Jsoup
        Connection conn = Jsoup
                .connect(connUrl)
                .header("Content-Type", "application/json;charset=UTF-8")
                .userAgent(USER_AGENT)
                .method(Connection.Method.GET)
                .ignoreContentType(true);

        // get HTML data
        // if internet disconnected, application stop at this point
        Document doc = conn.get();
        Log.i(this.getClass().getName(), "connection done, got HTML document");

        return doc;
    }

```

getNoticeTitles method extract title information from Document derived from getUrlConnection method. Extract select method from Jsoup for getting titles only. 

```
public List<String> getNoticeTitles() {

        String noticeURL = "http://tera.nexon.com/news/noticeTera/list.aspx";
        Elements noticeElements;                   // original elements getting from html
        List<String> noticeTitles = new ArrayList<>();  // original elements -> change to List<String>

        try {
            // get html document from site
            Document doc = getUrlConnection(noticeURL);

            // get titles only from html documents
            noticeElements = doc.select(".list_title");

        } catch (IOException e) {
            noticeElements = null;
        }

        if (noticeElements == null) {

            return noticeTitles;
        }

        // show 15 notice titles
        for (int i = 0; i < 15; i++) {
            // protect fron null - exception
            if (noticeElements.size() > i) {
                noticeTitles.add(elementToString(noticeElements).get(i));
            }
        }

        return noticeTitles;
    }
```

<div id='asynctask_for_using_network_in_android'/>
## Part2. AsyncTask for using network in Android
### Why use AsyncTask?
Android system make main thread when start application. This main thread access to UI tool kit and handle tasks like waiting input by users or draw images on screen. So main thread also called as UI thread.  

Android system execute all components of application in same thread. The point is, **UI thread is not thread-safe.** For example, screen cannot be updated if some tasks like "download many files" not finished. **This kinds of things like network connection cannot be done in main thread in Android.**  

So using AsyncTask for background process. AsyncTask execute background thread as asynchronous so main thread can their work with AsyncTask do its task.  

refer : [https://webnautes.tistory.com/1082](https://webnautes.tistory.com/1082)  

### Execute Jsoup using AsyncTask
Parsing by Jsoup occur via internet connections so have to use AsyncTask. We need to write 2 method to override for using AsyncTask - **doInBackground** and **onPostExecute**. 

**In** **doInBackground,** write code Asynchronized with MainActivity class. I write getNoticeTitles method used Jsoup for getting notice titles.
```
// get html data from URL using "Jsoup" library
@Override
protected Void doInBackground(Void... params) {

// no internet connections
noticeTitles = noticeService.getNoticeTitles();
if (noticeTitles.size() == 0) {
		noticeTitles.add("인터넷 연결 상태를 확인해주세요");
		noticeTitles.add("인터넷 연결 상태를 확인해주세요");
		noticeTitles.add("인터넷 연결 상태를 확인해주세요");
		noticeTitles.add("인터넷 연결 상태를 확인해주세요");
		noticeTitles.add("인터넷 연결 상태를 확인해주세요");
}

noticeURLs = noticeService.getNoticeURLs();

return null;
}
```

onPostExecute, write code which occurred after  doInBackground was executed. I write showNoticeTitleOnScreen and this method get notice title then show it on screen.

```
@Override
protected void onPostExecute(Void result) {
		Log.i(this.getClass().getName(), "starting onPostExeute");
		showNoticeTitleOnScreen();
}
```

<div id='released_app'/>
# Released App
Can download from play store. About 14 users installed it in 07-06, 2019
![search_user_agent]({{ "/assets/img/jsoup_user_agent_20190725.png" | absolute_url }})

https://play.google.com/store/apps/details?id=com.lee.donggyu.gamenoticeapptera&hl=ko