# Docker安装Nginx

``` bash
docker pull nginx
```

``` bash
sudo docker run --name nginx-test -p 8081:80 -d nginx
```

``` bash
mkdir -p /home/nginx/www /home/nginx/logs /home/nginx/conf
```

``` bash
sudo docker cp f77f78d2228d:/etc/nginx/nginx.conf /home/nginx/conf
```

``` bash
sudo docker run -d -p 8082:80 --name nginx-test-web -v /home/nginx/www:/usr/share/nginx/html -v /home/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /home/nginx/logs:/var/log/nginx nginx
```

``` bash
cd /home/nginx/www
touch index.html
```