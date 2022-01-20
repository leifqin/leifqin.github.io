# Problem collection

## 1. Access denied for user 'root'@'192.168.1.148' (using password: YES)
``` mysql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'abc123!@#' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```