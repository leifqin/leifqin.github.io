# Linux 常用命令

> 参考 [Linux命令大全](https://www.linuxcool.com/)

## firewall-cmd 命令
``` bash
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload
```

## nc 命令
``` bash
nc -vz 192.168.1.147 3306
nc -vz -w 5 192.168.1.147 3306
```

## crontab 命令
``` bash
crontab -l
crontab -e
crontab -r
```

## ps 命令
``` bash
ps -ef | grep java
ps -aux --sort=-%cpu | head -n 10
ps -aux --sort=-rss | head -n 10
```

## kill 命令
``` bash
kill -9 PID
```

## nohup 命令
``` bash
# 输出错误信息到日志文件
nohup java -jar test.jar >/dev/null2>log & 
# 不输出信息
nohup java -jar test.jar >/dev/null2>&1 & 
```

## curl 命令
``` bash
curl sipv4.com
curl https://github.com
```