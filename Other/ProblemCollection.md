# Problem collection

## 1. Access denied for user 'root'@'192.168.1.148' (using password: YES)
``` sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'abc123!@#' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

## 2. Failed to decode downloaded font
``` xml
<resources>
    <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
        <excludes>
             <exclude>static/**/*.woff</exclude>
             <exclude>static/**/*.woff2</exclude>
             <exclude>static/**/*.ttf</exclude>
        </excludes>
     </resource>
     <resource>
         <directory>src/main/resources</directory>
         <filtering>false</filtering>
         <includes>
              <include>static/**/*.woff</include>
              <include>static/**/*.woff2</include>
              <include>static/**/*.ttf</include>
         </includes>
     </resource>
</resources>
```

## 3. Ubuntu 安装 MySQL 问题
```
------------------------------------------------------------------------------
/etc/mysql/debian.cnf ->user   password

mysql -udebian-sys-maint -pUsIgysQBZbL6X4qW
mysql> 
show databases;
use mysql;
update user set authentication_string=PASSWORD("123456") where user='root';
update user set plugin="mysql_native_password";
flush privileges;
exit;

set global validate_password_policy=0;

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

FLUSH PRIVILEGES;
----------------------------------------------------------------------------

/etc/mysql/mysql.conf.d/mysqld.cnf

# By default we only accept connections from localhost 
#bind-address   = 127.0.0.1 
bind-address   = 0.0.0.0

[mysqld] 
lower_case_table_names=1

sql-mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

------------------------------------------------------------------------------
```

## 4. 终端访问GitHub
```
打开ClashX
设置为系统代理
出站模式选择（直连）
节点选择 US - 美国西雅图
curl https://github.com
git push
```