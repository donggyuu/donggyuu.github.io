---
title:  "When We Should Retry in RestTemplate"
excerpt: "Retry for Resttemplate Exception"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/when-we-sould-retry-in-rest-template_RestException.png
categories:
  - Development
tags:
  - Basic
last_modified_at: 2020-01-26T08:06:00-05:00
---

# Overview
API request using RestTemplate in Spring is not always successful for various exceptions. So we can consider "retrying request" in some exceptions. 

| RestException | Retry |
|:--------|:--------:|
| ResourceAccessException | O |
| HTTPServerErrorException | O |
| HTTPClientErrorException | O/X |
| UnknownHTTPStatusCodeException | X |  

<br>


# @retryable for Retry
Can use @retryable for retry in Spring. It is ok to code oneself by Java but using annotation is more clear and easy to read.

```java
@Retryable(value = {ResourceAccessException.class, HTTPServerErrorException.class}, maxAttempts = 2)
public void updateDB() throws ResourceAccessException, HTTPServerErrorException {
  post("update_data");
}
```   
[Spring-Retryでリトライ処理の実装](https://qiita.com/SotaOishi/items/f19d50794e3fabad5e95)


<br>

# Exception List in RestTemplate

Using RestTemplate in Spring can throw RestClientException. And RestClientException has some child exceptions. So...

Have to consider belows when retry

- ResourceAccessException
- HTTPServerErrorException
- HTTPClientErrorException
- UnknownHTTPStatusCodeException

![when-we-sould-retry-in-rest-template_RestException](/assets/images/when-we-sould-retry-in-rest-template_RestException.png)


[RestTemplateが投げる例外クラスまとめ](https://qiita.com/shotana/items/88b120432e694c9b63f6)


<br>

# Retry in each Exception

## ResourceAccessException
Had better retry when ResourceAccessException occur for it thrown when an I/O error occurs like time out. Many practices do retry this exception occurs.  
[→ spring-docs-ResourceAccessException](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/client/ResourceAccessException.html)

<br>

## HTTPServerErrorException

Had better retry when HTTPServerErrorException occur for it thrown when an HTTP 5xx is received.

Here are 5xx Responses and had better retry when first request failed except 505.

| Code | Status | Note |
|:--------|:--------|:--------|
| 500 | Internal Server Error | General server issue going on right now |
| 503 | Service Unavailable | server is temporarily unable to handle the request for being overloaded or down for maintenance |
| 504 | Gateway Timeout | one server did not receive a timely response from another server |
| 505 | HTTP Version Not Supported | server can emit if it doesn't support the major HTTP version the client used to make the request |

[spring-docs-HttpServerErrorException](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/client/HttpServerErrorException.html)

<br>

## HTTPClientErrorException

**not retry, add validation**

Had better NOT retry when HTTPClientErrorException occur for it thrown when an HTTP 4xx is received. 4xx is caused by wrong request from client. Unless client fix wrong request, same 4xx will return even though how many request you send.

Here are 4xx Responses  

| Code | Status | Note |
|:--------|:--------|:--------|
| 400 | Bad Request | server was unable to process the request sent by the client due to invalid syntax |
| 401 | Unauthorized | request has not been applied because it lacks valid authentication credentials for the target resource |
| 403 | Forbidden | access to the requested (valid) URL by the client is Forbidden for some reason |
| 404 | Not Found | browser was able to communicate with a given server, but the server could not find what was requested |
| 405 | Method Not Allowed | server can emit if it doesn't support HTTP method is simply not supported |  

<br>

About HTTP 400, can prevent it by adding validation to request parameter.

```java
ApiRequestParams params = new ApiRequestParams();
BindingResult bindingResult = new DataBinder(params).getBindingResult();

// call api
``` 

[spring-docs-HttpServerErrorException](https://hacknote.jp/archives/24535/)

<br>

**may ok to retry when 4xx but...**

Except HTTP 400, other 4xx errors are hardly happen. HTTP 400 itself will not happen if you add validation to request parameter. So retry when HTTPClientErrorException may ok in many cases but have to consider one more when doing like this.  

<br>

## UnknownHTTPStatusCodeException

Had better NOT retry when UnknownHTTPStatusCodeException occur for it thrown when an unknown (or custom) HTTP status code is received. Have to check the cause of this exception before you try to retry.