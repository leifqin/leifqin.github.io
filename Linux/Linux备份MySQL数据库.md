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

## 备份数据
> 把sys_oper_log表里面的数据全部备份到另外一张表sys_oper_log_bak，备份好后，再把sys_oper_log的数据删除掉，两条sql语句可以搞定
``` sql
CREATE TABLE `sys_oper_log_bak` (
  `oper_id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '日志主键',
  `title` varchar(50) DEFAULT '' COMMENT '模块标题',
  `business_type` int(2) DEFAULT '0' COMMENT '业务类型（0其它 1新增 2修改 3删除）',
  `method` varchar(100) DEFAULT '' COMMENT '方法名称',
  `request_method` varchar(10) DEFAULT '' COMMENT '请求方式',
  `operator_type` int(1) DEFAULT '0' COMMENT '操作类别（0其它 1后台用户 2手机端用户）',
  `oper_name` varchar(50) DEFAULT '' COMMENT '操作人员',
  `dept_name` varchar(50) DEFAULT '' COMMENT '部门名称',
  `oper_url` varchar(255) DEFAULT '' COMMENT '请求URL',
  `oper_ip` varchar(128) DEFAULT '' COMMENT '主机地址',
  `oper_location` varchar(255) DEFAULT '' COMMENT '操作地点',
  `oper_param` varchar(2000) DEFAULT '' COMMENT '请求参数',
  `json_result` varchar(2000) DEFAULT '' COMMENT '返回参数',
  `status` int(1) DEFAULT '0' COMMENT '操作状态（0正常 1异常）',
  `error_msg` varchar(2000) DEFAULT '' COMMENT '错误消息',
  `oper_time` datetime DEFAULT NULL COMMENT '操作时间',
  PRIMARY KEY (`oper_id`)
) ENGINE=InnoDB AUTO_INCREMENT=101 DEFAULT CHARSET=utf8 COMMENT='操作日志记录';

INSERT INTO sys_oper_log_bak (SELECT * FROM sys_oper_log WHERE oper_id > 100 AND oper_id <= 1000);

DELETE FROM sys_oper_log WHERE  oper_id > 100 AND oper_id <= 1000;
```