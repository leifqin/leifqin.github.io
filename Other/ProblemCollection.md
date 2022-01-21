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