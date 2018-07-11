# What is preflight request 

A CORS preflight request is a **CORS request** :star: :star: that checks to see if the CORS protocol is understood. A preflight request is automatically issued by a **browser**, when needed. 

It is an **OPTIONS request** :star: :star:, using three HTTP request headers:
- Access-Control-Request-Method
- Access-Control-Request-Headers
- Origin header.

> The `Access-Control-Request-Headers` is optional, it is set when a request includes the headers apart from set automatically by the user agent.


# Examples

First open `https://lbs.qq.com/webservice_v1/guide-ip.html` and console in chrome.

### example #1 simple cross-origin request

```javascript
fetch("https://restapi.amap.com/v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601")
```

```
Request Header:
  GET /v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601 HTTP/1.1
  Host: restapi.amap.com
  Connection: keep-alive
  Origin: https://lbs.qq.com
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
  Accept: */*
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
```

```
Response Header:
  HTTP/1.1 200 OK
  Server: Tengine
  Date: Wed, 11 Jul 2018 13:31:40 GMT
  Content-Type: application/json;charset=UTF-8
  Content-Length: 167
  Connection: close
  X-Powered-By: ring/1.0.0
  gsid: 011140057124153131590022800150587256798
  sc: 0.011
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: *
  Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,key,x-biz,x-info,platinfo,encr,enginever,gzipped,poiid
```

### example #2 preflighted request(1)

```javascript
fetch("https://restapi.amap.com/v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601", {method: "OPTIONS"})
```

```
Request Header:
  OPTIONS /v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601 HTTP/1.1
  Host: restapi.amap.com
  Connection: keep-alive
  Access-Control-Request-Method: OPTIONS
  Origin: https://lbs.qq.com
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
  Accept: */*
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
```
> Note that the request method is `OPTIONS`, and use the `Access-Control-Request-Method` header.

```
Response Header:
  HTTP/1.1 200 OK
  Server: Tengine
  Date: Wed, 11 Jul 2018 13:38:19 GMT
  Content-Type: application/octet-stream
  Content-Length: 0
  Connection: close
  Allow: GET, HEAD, POST, TRACE, OPTIONS
  X-Powered-By: ring/1.0.0
  gsid: 011140057138153131629942900175820196603
  sc: 0.004
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: *
  Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,key,x-biz,x-info,platinfo,encr,enginever,gzipped,poiid
```

### example #3 preflighted request(2)

```javascript
fetch("https://restapi.amap.com/v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601", {method: "POST", headers: {
"X-CustomHeader":"AnotherValue"}})
```

```
Request Header:
  OPTIONS /v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601 HTTP/1.1
  Host: restapi.amap.com
  Connection: keep-alive
  Access-Control-Request-Method: POST
  Origin: https://lbs.qq.com
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
  Access-Control-Request-Headers: x-customheader
  Accept: */*
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
```

> Note that the request method is `OPTIONS`, the `Access-Control-Request-Method` and Access-Control-Request-Headers headers are used.

```
Response Header:
  HTTP/1.1 200 OK
  Server: Tengine
  Date: Wed, 11 Jul 2018 13:44:18 GMT
  Content-Type: application/octet-stream
  Content-Length: 0
  Connection: close
  Allow: GET, HEAD, POST, TRACE, OPTIONS
  X-Powered-By: ring/1.0.0
  gsid: 010184063181153131665814600030408948611
  sc: 0.012
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: *
  Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,key,x-biz,x-info,platinfo,encr,enginever,gzipped,poiid
```
> Note that the `Access-Control-Allow-Headers` contains `X-CustomHeader` , so the browser will not raise an error.

If the preflighted request pass the browser's check, then an actual request will be initiated.

```
Request Header:
  POST /v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601 HTTP/1.1
  Host: restapi.amap.com
  Connection: keep-alive
  Content-Length: 0
  Origin: https://lbs.qq.com
  X-CustomHeader: AnotherValue
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
  Accept: */*
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
```
> Note that the request method of the actual request is `POST`.

```
Response Header:
  HTTP/1.1 200 OK
  Server: Tengine
  Date: Wed, 11 Jul 2018 13:52:50 GMT
  Content-Type: application/json;charset=UTF-8
  Content-Length: 167
  Connection: close
  X-Powered-By: ring/1.0.0
  gsid: 011183253238153131717026800037124803245
  sc: 0.006
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: *
  Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,key,x-biz,x-info,platinfo,encr,enginever,gzipped,poiid
```
Therefore there are total two requests issued by browser. :star: :star:

### example #4 preflighted request(3)

```javascript
fetch("https://restapi.amap.com/v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601", {method: "POST", headers: {
"X-Custom-Header":"AnotherValue"}})
```

```
Request Header:
  OPTIONS /v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601 HTTP/1.1
  Host: restapi.amap.com
  Connection: keep-alive
  Access-Control-Request-Method: POST
  Origin: https://lbs.qq.com
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
  Access-Control-Request-Headers: x-customheader
  Accept: */*
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
```

```
Response Header:
  HTTP/1.1 200 OK
  Server: Tengine
  Date: Wed, 11 Jul 2018 13:58:17 GMT
  Content-Type: application/octet-stream
  Content-Length: 0
  Connection: close
  Allow: GET, HEAD, POST, TRACE, OPTIONS
  X-Powered-By: ring/1.0.0
  gsid: 011183253243153131749735400131823358093
  sc: 0.005
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: *
  Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,key,x-biz,x-info,platinfo,encr,enginever,gzipped,poiid
```

This time the preflighted request failed, the browser raised the following error:
> Failed to load `https://restapi.amap.com/v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601`: Request header field X-Custom-Header is not allowed by Access-Control-Allow-Headers in preflight response.

Therefore no actual request will be launched for failed preflight request. 
