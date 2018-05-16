<div dir="rtl">بنام خدا</div>

- [Nginx](#nginx)


# Nginx

- in http:
```nginx
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }
```
- some server config:
```nginx
   ssl    on;
   server_tokens off;
   ssl_session_timeout  5m;
   ssl_protocols  SSLv2 SSLv3 TLSv1;
   ssl_ciphers  ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
   ssl_prefer_server_ciphers   on;
   client_max_body_size 21m;
   ssl_certificate        /Path to .crt;
   ssl_certificate_key    /Path to .key;
```
- some location:
```nginx
    location /WebApp/ {
     auth_basic "Restricted";
     auth_basic_user_file /Path to passwd;
     rewrite ^/WebApp/(.*)$ /$1 break;
     proxy_pass http://localhost:Port;
     proxy_redirect http://localhost:Port/ $scheme://$host:4443/rstudio/;
     proxy_http_version 1.1;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection $connection_upgrade;
     proxy_read_timeout 20d;
     }
```
- some parameters:
  - $scheme --> return which type of web request by client(http,https,ftp,...)
  - $request_uri --> return the subpage requested by client after /Location/ palce
  - keep order : _rewrite_,_proxy\_pass_,_proxy\_redirect_.
  - very very careful on Trailing sign. ("/").
  - use awesome **echo** module.
  
  
[top](#top)
  
