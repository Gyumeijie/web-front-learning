# HTTP Methods
- GET
- POST
- PUT
- HEAD
- DELETE
- PATCH
- OPTIONS

# The GET Method
GET is used to request data from a specified resource. 
Note that the query string (name/value pairs) is sent **in the URL** of a GET request:
```
/test/demo_form.php?name1=value1&name2=value2
```

# The POST Method
POST is used to send data to a server to create/update a resource.
The data sent to the server with POST is stored in **the request body** of the HTTP request:
```
POST /test/demo_form.php HTTP/1.1
Host: w3schools.com
name1=value1&name2=value2
```

# The PUT Method
PUT is used to send data to a server to create/update a resource.

The difference between POST and PUT is that PUT requests are idempotent. That is, calling the same PUT request 
multiple times will always produce the same result. In contrast, calling a POST request repeatedly will have 
side effects of creating the same resource multiple times.


# The HEAD Method
HEAD is almost identical to GET, but without the response body.


# Compare GET vs. POST
The following table compares the two HTTP methods: GET and POST.

|                      |                      GET                        |                     POST                     |
|----------------------|-------------------------------------------------|----------------------------------------------|
| BACK button/Reload	   | Harmless	| Data will be re-submitted (the browser should alert the user that the data are about to be re-submitted) |
| Cached	|Can be cached|Not cached |
| Encoding type	| application/x-www-form-urlencoded	| application/x-www-form-urlencoded or multipart/form-data. Use multipart encoding for binary data |
| History	| Parameters remain in browser history |Parameters are not saved in browser history|
| Restrictions on data length |	Yes, when sending data, the GET method adds the data to the URL; and the length of a URL is limited (maximum URL length is 2048 characters) |	No restrictions |
| Restrictions on data type | Only ASCII characters allowed	No restrictions. | Binary data is also allowed |
| Security | GET is less secure compared to POST because data sent is part of the URL Never use GET when sending passwords or other sensitive information! | POST is a little safer than GET because the parameters are not stored in browser history or in web server logs|
