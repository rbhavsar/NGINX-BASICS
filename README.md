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





