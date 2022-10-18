# Nginx常用配置

## 0. 默认配置
``` nginx
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    include servers/*;
}
```

## 1. 多个config配置
``` nginx
# 在conf文件夹下新建vhosts文件夹
include vhosts/*.conf;
```

## 2. 配置上传文件大小限制
``` nginx
#上传文件大小限制
client_max_body_size 1024M; 
#设置为on表示启动高效传输文件的模式
sendfile on; 
#保持连接的时间，默认65s
keepalive_timeout 1800;
```

## 3. SSL配置
``` nginx
server {
    listen 443 ssl;
    server_name qinlei.xyz;
    charset utf-8;
    
    ssl_certificate "ssl/********.pem";  
    ssl_certificate_key "ssl/********..key";
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3; 
    ssl_prefer_server_ciphers on;
    
    location / {
        root   /home/project/dist;
        try_files $uri $uri/ /index.html;
        index  index.html index.htm;
    }
		
    location /prod-api/ {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://localhost:8080/;
    }
}
```

## 4. 强制跳转https
``` nginx
server {
    listen 80;
    server_name qinlei.xyz;
    rewrite ^(.*)$ https://$host$1;
    location / {
        index index.html index.htm;
    }
}
```

## 5. location / 区别详解
``` nginx
前置测试访问域名：www.test.com/api/upload

1.location和proxy_pass都带/，则真实地址不带location匹配目录
location /api/ {
    proxy_pass http://127.0.0.1:8080/;
}
访问地址：www.test.com/api/upload-->http://127.0.0.1:8080/upload

2.location不带/，proxy_pass带/，则真实地址会带/
location /api {
    proxy_pass http://127.0.0.1:8080/;
}
访问地址： www.test.com/api/upload-->http://127.0.0.1:8080//upload


3.location带/，proxy_pass不带/，则真实地址会带location匹配目录/api/
location /api/ {
    proxy_pass http://127.0.0.1:8080;
}
访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/api/upload


4.location和proxy_pass都不带/，则真实地址会带location匹配目录/api/
location /api {
    proxy_pass http://127.0.0.1:8080;
}
访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/api/upload


5.同1，但 proxy_pass带地址
location /api/ {
    proxy_pass http://127.0.0.1:8080/server/;
}
访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/server/upload


6.同2，但 proxy_pass带地址，则真实地址会多个/
location /api {
    proxy_pass http://127.0.0.1:8080/server/;
}
访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/server//upload


7.同3，但 proxy_pass带地址，则真实地址会直接连起来
location /api/ {
    proxy_pass http://127.0.0.1:8080/server;
}
访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/serverupload


8.同4，但 proxy_pass带地址，则真实地址匹配地址会替换location匹配目录
location /api {
    proxy_pass http://127.0.0.1:8080/server;
}
访问地址： www.test.com/api/upload-->http://127.0.0.1:8080/server/upload


总结
1.proxy_pass代理地址端口后有目录(包括 / )，转发后地址：代理地址+访问URL目录部分去除location匹配目录 
2.proxy_pass代理地址端口后无任何，转发后地址：代理地址+访问URL目录部

```