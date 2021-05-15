---
title:  "Parsing by Jsoup"
excerpt: "How to use Jsoup library"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/parsing_jsoup.png
categories:
  - Development
tags:
  - Development-Skills
last_modified_at: 2019-05-26T08:06:00-05:00
---
# What is Jsoup?

HTML Parser for Java. Do not have to use regex when parsing.  
Official doc : [jsoup: Java HTML Parser
](https://jsoup.org/)

<br>

# Environmet Set

## 1. Import library
Set as below if using gradle for build tool.  
[sample_build.gradle](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/build.gradle#L29)

```java
    dependencies {
      compile 'org.jsoup:jsoup:1.11.3'
    }
```


## 2. Set UA
Set user agent that check user's browser. If value of user agent contains mobile's ones, then mobile page will be redirected if that site has mobile pages.  
[sample_UA](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L17)
```java
String USER_AGENT = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.152 Safari/537.36";
```
<br>

Can get value of user agent by google as below.
![when-we-sould-retry-in-rest-template_RestException](/assets/images/parsing-by-jsoup.png)



## 3. Set connection values

Set connectURL, Connection then get Document values as below. Document values will be used for parsing.  
[sample_connection_values](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L21)
```java
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
```
<br>

# Sample Usages

## Use class

For getting "*Sample Parsing Data*" from `"<p class="list_title ellipsis">Sample Parsing Data</p>"` in some HTML, do as below.  
[sample_using_class](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L41)
```java
// Set class which has values for parsing
Elements sampleParsingData = document.getElementsByClass("list_title ellipsis");

System.out.println("=====print titles=====");
    for (String data : elementToString(sampleParsingData)) {
        System.out.println(data);
}
```


## In duplicated tags

Want to get href values from HTML that many tags are duplicated.
```html
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
```
<br>

For getting href values, filter values by using tags one by one.  
[sample_duolicated_tages](https://github.com/donggyuu/spring-basic/blob/master/crawling-jsoup/src/main/java/donggyu/lee/CrawlingMain.java#L54)
```java
// Get <ol> values from "list_area2" first
Elements ol = document.select("div.list_area2 > ol");

// Then get <li> values from <ol> values 
Elements li = ol.select("li > *");

System.out.println("=====print URLs=====");
    for (int i = 0; i < li.size(); i++) {
        System.out.println(li.get(i).attr("abs:href"));
    }
```