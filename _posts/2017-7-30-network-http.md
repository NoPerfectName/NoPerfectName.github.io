---
layout: post
title: Http协议
categories: network
tags: network
author: NoPerfectName
excerpt: 简单介绍Http协议的组成部分
---

* content
{:toc}

#### HTTP Request
（1）Method—Uniform Resource Identifier (URI)—Protocol/Version  
（2）Request headers  
（3）Entity body  

```javascript
POST /examples/default.jsp HTTP/1.1
Accept: text/plain; text/html
Accept-Language: en-gb
Connection: Keep-Alive
Host: localhost
User-Agent: Mozilla/4.0 (compatible; MSIE 4.01; Windows 98)
Content-Length: 33
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip, deflate

lastName=Franks&firstName=Michael
```

注意：  
（1）请求头的格式： 请求头名+英文空格+请求头值  
（2）请求头和请求实体之间有一个空白行（CRLF）。这是 HTTP 协议规定的格式，HTTP 服务器，以此确定请求实体从哪里开始的。  

上面的例子中，请求实体是： lastName=Franks&firstName=Michael	 



#### HTTP Response 
(1)Protocol—Status code—Description  
(2)Response headers  
(3)Entity body  

```javascript
HTTP/1.1 200 OK
Server: Microsoft-IIS/4.0
Date: Mon, 5 Jan 2004 13:13:33 GMT
Content-Type: text/html
Last-Modified: Mon, 5 Jan 2004 13:13:12 GMT
Content-Length: 112

<html>
<head>
<title>HTTP Response Example</title>
</head>
<body>
Welcome to Brainy Software
</body>
</html>
```


