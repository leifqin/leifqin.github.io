# Maven之profile实现多环境配置动态切换

``` xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.1</version>
            <configuration>
                <source>${java.version}</source>
                <target>${java.version}</target>
                <encoding>${project.build.sourceEncoding}</encoding>
            </configuration>
        </plugin>
    </plugins>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <!-- 关闭过滤 -->
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <!-- 引入所有 匹配文件进行过滤 -->
            <includes>
                <include>application*</include>
                <include>bootstrap*</include>
                <include>banner*</include>
            </includes>
            <!-- 启用过滤 即该资源中的变量将会被过滤器中的值替换 -->
            <filtering>true</filtering>
        </resource>
    </resources>
</build>

<profiles>
    <profile>
        <id>dev</id>
        <properties>
            <!-- 环境标识，需要与配置文件的名称相对应 -->
            <profiles.active>dev</profiles.active>
            <logging.level>debug</logging.level>
        </properties>
        <activation>
            <!-- 默认环境 -->
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <profile>
        <id>test</id>
        <properties>
            <profiles.active>test</profiles.active>
            <logging.level>warn</logging.level>
        </properties>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <profiles.active>prod</profiles.active>
            <logging.level>warn</logging.level>
        </properties>
    </profile>
</profiles>
```

``` yaml

# 日志配置
logging:
  level:
    com.ruoyi: @logging.level@
    org.springframework: warn

# Spring配置
spring:
  profiles: 
    active: @profiles.active@
```