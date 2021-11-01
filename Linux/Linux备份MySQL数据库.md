# Linux备份MySQL数据库

## 创建 backup.sh 文件
```bash
cd /home

mkdir backup

touch backup.sh

vim backup.sh

docker exec mymysql mysqldump -hdev.jingyi.net -uroot -p123456 sjzt > /home/backup/sjzt_$(date +%Y%m%d).sql

:wq

# 添加可执行权限
chmod 777 backup.sh 或者 chmod +x backup.sh

# 执行sh文件
./backup.sh
sh backup.sh
bash backup.sh
source backup.sh

```
> 注意事项

crontab 定时执行的时候 `dump` 出来的文件大小始终是 0KB，后来发现去掉 `-it` 就可以了，按照文档解释 `-t` 是分配一个伪终端,但是 `crontab` 执行的时候实际是不需要的

## 定时任务导出数据库
```bash
# 安装crond
yum -y install vixie-cron
yum -y install crontabs

# 启动
service crond start

crontab -e

00 23 * * * /home/backup.sh

:wq

service crond restart

```
## 导入数据库
```bash
docker cp /home/backup/sjzt.sql mymysql:/home/

docker exec -it mymysql  /bin/bash

# 方法一
mysql -uroot -p123456

mysql> show databases;

mysql> use sjzt;

mysql> source /home/sjzt.sql;

# 方法二
mysql -uroot -p123456 sjzt < /home/sjzt.sql;
```

