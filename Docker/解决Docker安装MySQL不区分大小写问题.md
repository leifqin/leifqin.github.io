# Docker安装MySQL忽略大小写问题的问题

``` bash
docker run -p 3306:3306 --name mysql -v /home/mysql/log:/var/log/mysql -v /home/mysql/data:/var/lib/mysql -v /home/mysql/conf:/etc/mysql -e MYSQL_ROOT_PASSWORD=greedisgood  -e TZ=Asia/Shanghai -d mysql:5.7
```

查看当前mysql的大小写敏感配置

```
show global variables like '%lower_case%';
```
+------------------------+-------+
| Variable_name     | Value |
+------------------------+-------+
| lower_case_file_system | ON  |
| lower_case_table_names | 0   |
+------------------------+-------+

lower_case_file_system
表示当前系统文件是否大小写敏感，只读参数，无法修改。
ON 大小写不敏感 
OFF 大小写敏感 

lower_case_table_names
```
lower_case_table_names=0 表名存储为给定的大小和比较是区分大小写的
lower_case_table_names = 1 表名存储在磁盘是小写的，但是比较的时候是不区分大小写
lower_case_table_names=2 表名存储为给定的大小写但是比较的时候是小写的
unix,linux下lower_case_table_names默认值为 0 .Windows下默认值是 1 .Mac OS X下默认值是 2
```


在/home/mysql/conf目录下建立配置文件my.cnf
``` conf
[mysqld] 
lower_case_table_names=1
```

重启容器
``` bash
docker restart mysql
```
