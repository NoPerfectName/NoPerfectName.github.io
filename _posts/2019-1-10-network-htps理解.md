---
layout: post
title: https的简单理解
categories: network
tags: network
author: NoPerfectName
excerpt: 理解https的加密解密过程
---

* content
{:toc}


#### https的理解

https = http + TLS/SSL

https的加密过程，其实一个解决一个个问题的过程。

主要内容如下

* 为了防止篡改，使用散列函数处理明文，常见的有MD5、SHA1、SHA256。但是该处理无法解决第三方重新生成摘要和明文来直接调包内容。
* 为了解决上述问题，需要使用对称加密的方法加密，而且还可以防止内容被窃听，常见的算法有AES-CBC、DES、3DES、AES-GCM等。但是这样无法做到服务器一对多的访问模式。
* 为了解决对称加密的问题，于是有了非对称加密，公钥加密的内容只能用私钥解密，私钥加密的内容只能由公钥解密。通过非对称加密来使浏览器和服务器协商生成对称加密的密钥，常见的算法有RSA算法、ECC、DH等。但是非对称加密无法验证公钥的身份。
* 为了解决非对称加密的问题，于是有了CA（Certificate Authority）证书的出现。CA使用用户生成的**公钥和证书信息**生成CSR证书，通过CA的私钥签名生成SSL证书。浏览器内置的CA公钥会验证其用户公钥的合法性。