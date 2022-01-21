# Nginx常用配置

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