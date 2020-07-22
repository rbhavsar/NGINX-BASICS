# NGINX-BASICS
Understand Nginx Basic Features

Configuration terms
- Context
- Directive

![](/nginx/screenshots/context-directive.jpg)

Install, Start/Stop - https://www.javatpoint.com/installing-nginx-on-mac


**Location block** 
- Think of it is intercepting request based on it’s value
```
server {
        
        listen       8080; # To listen on 8080
        server_name  localhost;
        
        root   html/sites/demo;
          location /greet {
          return 200 'Hello world from nginx /greet location.';
        }
}
```
Access : http://127.0.0.1:8080/greet ( It will show "Hello world from nginx /greet location.” On page ) 

**Prefix Match***

http://127.0.0.1:8080/greet OR http://127.0.0.1:8080/greeting
```
location /greet {
          return 200 'Hello world from nginx /greet location.Prefix match';
        }
```

**Exact match**
```
location = /greet {
          return 200 'Hello world exact match';
        }
```

**Regex match**
http://127.0.0.1:8080/greet2 ( case sensitive )

```
location ~ /greet[0-9] {
          return 200 'Hello world regex match';
        }
```

**Preferential prefix**
http://127.0.0.1:8080/Greet2

```
location ^~ /Greet2 {
          return 200 'Hello world regex match';
        }
```


**Variables**
- Configuration - set $var 'something'
- NGINX module variable - $http , $uri, $args

Example 1 :
```
location /greet {
          return 200 '$host\n$uri';
        }
```
Output : 
127.0.0.1 /greet

Example 2:
```
 if ( $arg_apikey != 1234 ){
          return 401 "Incorrect key.";
        }
```

Output : 
- Load http://127.0.0.1:8080/ page —> it will return Incorrect Api key
- Load http://127.0.0.1:8080?apikey=1234 -> page will load correctly


**Return**


Create simple redirect with return directive 
Goal : when we access  http://127.0.0.1:8080/logo —> it should redirect to http://127.0.0.1:8080/thumb.png

```
location /logo {
          return 307 /thumb.png;
        }
```

**Rewrite**

It will return the response as per rule set but it will keep original url on browser

```
rewrite ^/user/\w+ /greeting;

        location /greeting {
          return 200 'Hello users';
        }
```

With above code : access http://127.0.0.1:8080/user/ravi(any word) —> it will print ‘Hello users’ but it will stay on http://127.0.0.1:8080/user/ravi url

***Headers & Expires**
Add Response headers + manupulate the headers

```
location = /thumb.png {

          add_header my_header "Hello";  
          add_header Cache-Control public;
          expires 1M;
 }
```

```
USSFNRBHAV01:~ rbhavsar$ curl -I http://127.0.0.1:8080/thumb.png
HTTP/1.1 200 OK
Server: nginx/1.19.0
Date: Mon, 29 Jun 2020 05:57:27 GMT
Content-Type: image/png
Content-Length: 12627
Last-Modified: Wed, 11 Apr 2018 20:31:37 GMT
Connection: keep-alive
ETag: "5ace70a9-3153"
Expires: Wed, 29 Jul 2020 05:57:27 GMT
Cache-Control: max-age=2592000
my_header: Hello
Cache-Control: public
Accept-Ranges: bytes
USSFNRBHAV01:~ rbhavsar$ 
```


**Reverse proxy**

![](/nginx/screenshots/reverse-proxy.jpg)

Completely remote site proxying via our server 

location /google {
         add_header proxied nginx;
          proxy_pass 'https://www.google.com/';
        }



![](/nginx/screenshots/google.jpg)






