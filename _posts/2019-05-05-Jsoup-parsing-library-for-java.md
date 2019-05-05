---
layout: post
title: "Jsoup - Parsing Library for Java"
description: "How to set and use Jsuop for parsing site of HTML"
category: essay
tags: [Java, Parsing]
comments: true
---
# Jsoup - Parsing Library for Java

# What is Jsoup?

HTML Parser for Java. Can get some parts you want from HTML files.

details : [https://jsoup.org/](https://jsoup.org/)

# Basic setting

## Import library

Set dependencies as below If you use "gradle" as build tool.

    dependencies {
      ...
      // jsoup HTML parser library @ https://jsoup.org/
      compile 'org.jsoup:jsoup:1.11.3'
      ...
    }

git: [https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/build.gradle#L29](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/build.gradle#L29)

## Set user agent

Set user agent which checking user's browsers. If value of user agent contains mobile's ones, then mobile page will be redirected if that site has mobile pages.

    String USER_AGENT = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.152 Safari/537.36";

git: [https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L17](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L17)

Can get value of user agent by google as below.

![](get_user_agent-649dc547-6c48-4438-a6c1-80283d42db3c.png)

## Set connection values

Set connectURL, Connection then get Document values as below. Document values will be used for parsing.

    // URL you want to parse
    String connectURL = "http://tera.nexon.com/news/events/list.aspx";
    
    // Make connection and set values
    Connection conn = Jsoup
    		.connect(connectURL)
    		.header("Content-Type", "application/json;charset=UTF-8")
    		.userAgent(USER_AGENT)
    		.method(Connection.Method.GET)
    		.ignoreContentType(true);
    
    // Get document value from connections
    // Will parsing from this document value
    Document document = conn.get();

git: [https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L21](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L21)

# Sample Usages

## Get values using class

To get "*Sample Parsing Data*" from "*<p class="list_title ellipsis">Sample Parsing Data</p>*" in some HTML, do as below.

    // Set class which has values for parsing
    Elements sampleParsingData = document.getElementsByClass("list_title ellipsis");
    
    System.out.println("=====print titles=====");
        for (String data : elementToString(sampleParsingData)) {
            System.out.println(data);
    }

git: [https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L41](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L41)

## Get values from duplicated tags

Here is HTML codes of duplicated many tags and want to get href values.

    <div class="list_area2">
      <ol class="list_event clearFix">
        <li>
          <a href="/events/2019/0502/goldweek.aspx">
          <a href="/events/2019/0502/newReturn.aspx">
          ...
          <a href="/events/2019/0425/spring.aspx ">
        </li>
      </ol>
    </div>

To get href values from duplicated tags, filter values by using tags one by one as below.

    // Get <ol> values from "list_area2" first
    Elements ol = document.select("div.list_area2 > ol");
    
    // Then get <li> values from <ol> values 
    Elements li = ol.select("li > *");
    
    System.out.println("=====print URLs=====");
        for (int i = 0; i < li.size(); i++) {
            System.out.println(li.get(i).attr("abs:href"));
        }

git: [https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L54](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L54)
