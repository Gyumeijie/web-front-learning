# Cross-Origin Resource Sharing

Cross-Origin Resource Sharing (CORS) is a mechanism that uses **additional HTTP headers** to tell a **browser** :star: to let a web application running at one origin (domain) have permission to access selected resources from a **server** :star: at a different origin.
> The additional HTTP headers are set by a server at a different origin. :heavy_exclamation_mark:

A web application makes a **cross-origin HTTP request** when it requests a resource that has a different origin (domain, protocol, and port) than its own origin.

For **security** reasons, **browsers** restrict **cross-origin HTTP requests** initiated from within scripts. For example, `XMLHttpRequest` and the `Fetch` API follow the **same-origin policy**. This means that a web application using those APIs can only request HTTP resources from the same origin the application was loaded from, unless the response from the other origin includes the right **CORS** headers. :star:
> Note: not all http requests initiated by XMLHttpRequest and the Fetch are cross-origin HTTP requests.


# Examples

Open `https://lbs.qq.com/webservice_v1/guide-ip.html` and console in chrome. 


### example #1

```javascript
fetch("https://apis.map.qq.com/ws/location/v1/ip?key=CFPBZ-EGOCF-PCCJF-NSHVY-CNRQQ-QCF4R&output=json")
```

```
Request Header:
  Accept: */*
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
  Connection: keep-alive
  Host: apis.map.qq.com
  Origin: https://lbs.qq.com
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
```

```
Response Header:
  Access-Control-Allow-Credentials: true
  Access-Control-Allow-Origin: https://lbs.qq.com
  Connection: keep-alive
  Content-Length: 391
  Content-Type: application/json; charset=utf-8
  Date: Tue, 10 Jul 2018 14:38:07 GMT
  Server: nginx
  X-LIMIT: current_qps=1; limit_qps=5; current_pv=46; limit_pv=10000
```

The response from the other origin(`https://apis.map.qq.com`),includes the right **CORS** headers. :heavy_check_mark:

### example #2

```javascript
fetch("https://restapi.amap.com/v3/ip?output=json&key=88c83fdd5e84af6ef4adc5b1d7f8a601")
```

```
Request Header:
  Accept: */*
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
  Connection: keep-alive
  Host: restapi.amap.com
  Origin: https://lbs.qq.com
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
```

```
Response Header:
  Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,key,x-biz,x-info,platinfo,encr,enginever,gzipped,poiid
  Access-Control-Allow-Methods: *
  Access-Control-Allow-Origin: *
  Connection: close
  Content-Length: 166
  Content-Type: application/json;charset=UTF-8
  Date: Tue, 10 Jul 2018 14:46:48 GMT
  gsid: 011183253241153123400836100046260695515
  sc: 0.004
  Server: Tengine
  X-Powered-By: ring/1.0.0
```
The response from the other origin(`https://restapi.amap.com`),includes the right **CORS** headers. :heavy_check_mark:

# example #3

Open `https://lbs.amap.com/api/webservice/guide/api/ipconfig` and console in chrome. 

```javascript
fetch("https://apis.map.qq.com/ws/location/v1/ip?key=CFPBZ-EGOCF-PCCJF-NSHVY-CNRQQ-QCF4R&output=json")

Failed to load https://apis.map.qq.com/ws/location/v1/ip?key=CFPBZ-EGOCF-PCCJF-NSHVY-CNRQQ-QCF4R&output=json: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'https://lbs.amap.com' is therefore not allowed access. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
```

```
Request Header:
  Accept: */*
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
  Connection: keep-alive
  Host: restapi.amap.com
  Origin: https://lbs.qq.com
  Referer: https://lbs.qq.com/webservice_v1/guide-ip.html
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
```

```
Response Header:
  Connection: keep-alive
  Content-Length: 391
  Content-Type: application/json; charset=utf-8
  Date: Tue, 10 Jul 2018 14:55:21 GMT
  Server: nginx
  X-LIMIT: current_qps=1; limit_qps=5; current_pv=48; limit_pv=10000
```
The response from the other origin(`https://apis.map.qq.com`),does not include **CORS** headers. :x:
