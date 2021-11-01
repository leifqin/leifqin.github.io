# Docker安装Nginx.md

## Docker 启动 Nginx
``` bash
docker run -p 80:80 -p 443:443 --name nginx \
--restart=always \
--privileged=true \
-v /mydata/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /mydata/nginx/html:/etc/nginx/html \
-v /mydata/nginx/log:/var/log/nginx \
-v /mydata/nginx/cert:/etc/nginx/cert \
-d nginx 
```

## 配置 SSL 证书
``` nginx
server { 
    listen 80; 
    listen 443 ssl; 
    server_name  qinlei.xyz;

    ssl_certificate /etc/nginx/cert/baidu.com_chain.crt;
    ssl_certificate_key /etc/nginx/cert/baidu.com_key.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;

    client_max_body_size 1024m;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /etc/nginx/html; #页面存放目录
        index  index.html index.htm; #默认页面
    }
}
```