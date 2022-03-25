# Linux部署项目

## 安装 Nginx
```bash
#gcc安装，nginx源码编译需要
yum install gcc-c++

#PCRE pcre-devel 安装，nginx 的 http 模块使用 pcre 来解析正则表达式
yum install -y pcre pcre-devel

#zlib安装，nginx 使用zlib对http包的内容进行gzip
yum install -y zlib zlib-devel

#OpenSSL 安装，强大的安全套接字层密码库，nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http）
yum install -y openssl openssl-devel

#下载版本号可根据目前官网最新稳定版自行调整
cd /home/
wget -c https://nginx.org/download/nginx-1.18.0.tar.gz

#根目录使用ls命令可以看到下载的nginx压缩包，然后解压
tar -zxvf nginx-1.18.0.tar.gz

#解压后进入目录
cd nginx-1.18.0

#使用默认配置
./configure

#开启SSL模块配置
./configure --prefix=/usr/local/nginx  --with-http_stub_status_module --with-http_ssl_module

#编译安装
make
make install

#查找安装路径，默认都是这个路径
whereis nginx

#启动、停止nginx
cd /usr/local/nginx/sbin/
./nginx     #启动
./nginx -s stop  #停止，直接查找nginx进程id再使用kill命令强制杀掉进程
./nginx -s quit  #退出停止，等待nginx进程处理完任务再进行停止
./nginx -s reload  #重新加载配置文件，修改nginx.conf后使用该命令，新配置即可生效
./nginx -t #检查配置时候正确

```

``` bash
#设置开机启动
vi /etc/rc.local
#增加一行 /usr/local/nginx/sbin/nginx，增加后保存
#设置执行权限
cd /etc
chmod 755 rc.local
```

``` nginx
server {
	listen       80;
	listen       443 ssl;
	server_name  dev.jingyi.net;

	ssl_certificate /ssl/4681094_dev.jingyi.net.pem;
	ssl_certificate_key /ssl/4681094_dev.jingyi.net.key;
	ssl_session_timeout 5m;
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_prefer_server_ciphers on;

	location / {
		root   /home/zhst/zhst-ui;
		try_files $uri $uri/ /index.html;
		index  index.html index.htm;
	}

	location /zhst/ {
	alias   /home/zhst/zhst-ui/;
	try_files $uri $uri/ /zhst/index.html;
	index  index.html index.htm;
	}

	location /prod-api/{
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header REMOTE-HOST $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://localhost:8080/;
	}
}

```

## 安装 Docker 
```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

sudo systemctl enable docker
sudo systemctl start docker
```
## 安装 Redis
```bash
docker pull redis

docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-d redis redis-server --appendonly yes --requirepass greedisgood

firewall-cmd --zone=public --add-port=6379/tcp --permanent
firewall-cmd --reload

docker container update --restart=always redis
```
## 安装 Mysql
```bash
docker pull mysql:5.7

docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root  \
-e TZ=Asia/Shanghai \
-d mysql:5.7


firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload

docker container update --restart=always mysql
```
## 安装 JDK
```bash
1. 下载 jdk-8u271-linux-x64.tar.gz

2. mkdir /usr/java

3. 将 jdk-8u271-linux-x64.tar.gz 上传到 /usr/java

4. tar zxvf jdk-8u271-linux-x64.tar.gz

5. rm -f jdk-8u271-linux-x64.tar.gz

6. vi /etc/profile

7. done 后面插入(i)下面内容后保存退出(ESC :wq)
export JAVA_HOME=/usr/java/jdk1.8.0_271
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin

8. source /etc/profile

9. java -version
```
## 启动项目
```bash
1. 上传 ry.sh 和 ruoyi-admin.jar 到 /home/ruoyi 下

2. chmod u+x ry.sh

3. 修改 ry.sh
#########################################################
AppName=ruoyi-admin.jar
#########################################################

#使用unix换行符
vim ry.sh
:set ff=unix
:wq

4. 启动
./ry.sh start 启动 stop 停止 restart 重启 status 状态
```
> 参考资料
- [ry.sh](https://github.com/yangzongzhuan/RuoYi/blob/master/ry.sh)

