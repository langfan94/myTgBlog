---
title: 从输入URL到页面加载完成的过程中都发生了什么
date: 2018-05-28 10:38:20
tags:
---

# 从输入URL到页面加载完成的过程中都发生了什么?

1. browser checks cache; if requested object is in cache and is fresh, skip to #9(浏览器检查缓存，若缓存中存储着要请求的内容，并且内容是最新的，直接跳转到第9步)
2. browser asks OS for server's IP address(浏览器请求操作系统（OS）解析服务器的IP地址)
3. OS makes a DNS lookup and replies the IP address to the browser(操作系统做DNS解析，查找并返回IP地址给浏览器)
4. browser opens a TCP connection to server (this step is much more complex with HTTPS)(浏览器与服务器建立TCP连接（若使用的是https协议，连接过程会更加的复杂)
5. browser sends the HTTP request through TCP connection(浏览器通过TCP连接发送http请求)
6. browser receives HTTP response and may close the TCP connection, or reuse it for another request(浏览器接收到http相应后，关闭（断开）TCP连接或者利用TCP发送其他http请求)
7. browser checks if the response is a redirect (3xx result status codes), authorization request (401), error (4xx and 5xx), etc.; these are handled differently from normal responses (2xx)(浏览器检测http相应是否是重定向（http状态码为3xx），授权相应（401），错误相应（4xx和5xx）等；浏览器对这些状态码的处理与正常的相应（2xx）是不同的)
8. if cacheable, response is stored in cache(若可以被缓存，则将相应存储到缓存中)
9. browser decodes response (e.g. if it's gzipped)(浏览器解压相应（比如页面被gzip压缩过）)
10. browser determines what to do with response (e.g. is it a HTML page, is it an image, is it a sound clip?)(浏览器决定以什么样式的方式解析http相应（比如他可能是个html网页，可能是一张图片，或者可能是个音频短片等）)
11. browser renders response, or offers a download dialog for unrecognized types(若相应的是浏览器不能解析的格式，则下载该文件；否则浏览器就解析相应)

<a href="https://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser" target="_blank">参考链接</a>