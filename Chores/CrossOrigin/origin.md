
# Origin header

The Origin **request header** :star: indicates where a fetch originates from. It doesn't include any path information, but only the server name. It is sent with **CORS** requests, as well as with **POST** requests. It is similar to the `Referer` header, but, unlike this header, it doesn't disclose the whole path.

# Examples

The following examples are a series of experiments done under chrome. Begin to exercise, we need to open `https://lbs.amap.com/api/webservice/guide/api/ipconfig` and chrome console(`F12` for linux or windows; `option+command+i` for mac). 

### exercise #1 

Simple non-cross-origin **GET** request. 
> Normal http request

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://lbs.amap.com/dev/home/get-raty-info?postId=646', true);  
xhr.send(null);
```

```
Request Header:
  :authority: lbs.amap.com
  :method: GET
  :path: /dev/home/get-raty-info?postId=646
  :scheme: https
  accept: */*
  accept-encoding: gzip, deflate, br
  accept-language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
  cookie: _uab_collina=153113361523606201399293; AMAPID=5d3519478c805aa6084a65812b25bef5; UM_distinctid=1647ea429339-0117d0b2235146-25004775-1fa400-1647ea42935fd; passport_login=NTg5ODUyODEsYW1hcF8xODE3MDg3OTgxOXVta3ZhcDVwLGJsMjJxazQzbWhkdWtiZTYzbGxpbWRnbnI3dTVma3lxLDE1MzExMzMzMDQsTWpVMU1USmhNMlppTlRrM1pURmxPREEwWldOak1URTFZMkpsWkRrNVlXST0%3D; guid=fb92-4763-0ede-1d12; cna=HFpPE/57m3ACAbedoCujBZNc; isg=BGdnS18v8D24anSxFJ2BOfWs9pLvITqoVp3deznUgvYdKIfqQbzLHqUJTmATwBNG; PHPSESSID=fife89qm79r1g2jsmeg04rjqp7; dev_help=UPyPZip8R1%2BrTMVKiiPnXDc0NTljNDNmY2I4MWIwYzU4OGJlZjY5ZGY0ZTFmYTczZmE2NWY1MWMyNzM1MDgzN2M0NDEyNzk4ZjZkNGM3NGXzue6b2OXE6X6SQir9QemnaDYmzce3fzJRlLBDsDs90RjmigqHyMTIi0B%2BI1cclv%2BNr6NhGiB2wmdO9RyQ%2FHPcV33czItZsfePbMeXbofpD%2FT%2Bpw0b4EwXpNzNzTcKQ8o%3D; CNZZDATA1255621432=869501234-1531131124-%7C1531223675
  referer: https://lbs.amap.com/api/webservice/guide/api/ipconfig
  user-agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
```

### exercise #2  

Simple non-cross-origin **POST** request. 
> Normal http request

```javascript
var xhr = new XMLHttpRequest();
xhr.open('POST', 'https://lbs.amap.com/dev/home/get-raty-info?postId=646', true);  
xhr.send(null);
```

```
Request Header
  :authority: lbs.amap.com
  :method: POST
  :path: /dev/home/get-raty-info?postId=646
  :scheme: https
  accept: */*
  accept-encoding: gzip, deflate, br
  accept-language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
  content-length: 0
  cookie: _uab_collina=153113361523606201399293; AMAPID=5d3519478c805aa6084a65812b25bef5; UM_distinctid=1647ea429339-0117d0b2235146-25004775-1fa400-1647ea42935fd; passport_login=NTg5ODUyODEsYW1hcF8xODE3MDg3OTgxOXVta3ZhcDVwLGJsMjJxazQzbWhkdWtiZTYzbGxpbWRnbnI3dTVma3lxLDE1MzExMzMzMDQsTWpVMU1USmhNMlppTlRrM1pURmxPREEwWldOak1URTFZMkpsWkRrNVlXST0%3D; guid=fb92-4763-0ede-1d12; cna=HFpPE/57m3ACAbedoCujBZNc; isg=BGdnS18v8D24anSxFJ2BOfWs9pLvITqoVp3deznUgvYdKIfqQbzLHqUJTmATwBNG; PHPSESSID=fife89qm79r1g2jsmeg04rjqp7; dev_help=UPyPZip8R1%2BrTMVKiiPnXDc0NTljNDNmY2I4MWIwYzU4OGJlZjY5ZGY0ZTFmYTczZmE2NWY1MWMyNzM1MDgzN2M0NDEyNzk4ZjZkNGM3NGXzue6b2OXE6X6SQir9QemnaDYmzce3fzJRlLBDsDs90RjmigqHyMTIi0B%2BI1cclv%2BNr6NhGiB2wmdO9RyQ%2FHPcV33czItZsfePbMeXbofpD%2FT%2Bpw0b4EwXpNzNzTcKQ8o%3D; CNZZDATA1255621432=869501234-1531131124-%7C1531223675
  origin: https://lbs.amap.com
  referer: https://lbs.amap.com/api/webservice/guide/api/ipconfig
  user-agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
```

We can obviously note that the **POST** request has a **Origin** header, while the **GET** request doesn't.

### exercise #3

A cross-origin GET request.
> Cross-origin http request

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://apis.map.qq.com/ws/location/v1/ip?key=CFPBZ-EGOCF-PCCJF-NSHVY-CNRQQ-QCF4R&output=json'); 
xhr.send(null);
```

```
Request Header:
  Accept: */*
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
  Connection: keep-alive
  Host: apis.map.qq.com
  Origin: https://lbs.amap.com
  Referer: https://lbs.amap.com/api/webservice/guide/api/ipconfig
  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/66.0.3359.181 Chrome/66.0.3359.181 Safari/537.36
```

Still we can notice that the Origin header is sent with the **CORS** request.

# Conclusion

A request initiated by XMLHttpRequest or Fetch may be a ***normal http request*** or a ***cross-origin http request***. :star:

A web application makes a cross-origin HTTP request when it requests a resource that has a different origin (domain, protocol, and port) than its own origin.

XMLHttpRequest and the Fetch API follow the same-origin policy. This means that a web application using those APIs can only request HTTP resources from the same origin the application was loaded from, unless the response from the other origin includes the right CORS headers.
