# Maven引入本地jar包

## 1. 上传到maven中心仓库

## 2. 搭建maven私服

## 3. 传统方式

## 4. pom文件scope的system属性
> **src/main/resources** 目录下新建目录 jar，将 DmJdbcDriver18.jar 放入 jar 目录
``` xml
</dependencies>
    <dependency>
        <groupId>dm.jdbc</groupId>
        <artifactId>DmDriver</artifactId>
        <version>1.8</version>
        <!-- scope system -->
        <scope>system</scope>
        <systemPath>${project.basedir}/src/main/resources/jar/DmJdbcDriver18.jar</systemPath>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.1.1.RELEASE</version>
            <configuration>
                <fork>true</fork> 
                <!-- includeSystemScope true-->
                <includeSystemScope>true</includeSystemScope>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>repackage</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
    <finalName>${project.artifactId}</finalName>
</build>
```
> 参考文档
[maven引入本地jar包的方法](https://juejin.cn/post/6844904135410581511)
