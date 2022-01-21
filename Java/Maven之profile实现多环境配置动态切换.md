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
            <!--打包该目录下的 application.yml -->
            <directory>src/main/resources</directory>
            <!-- 启用过滤 即该资源中的变量将会被过滤器中的值替换 -->
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

</build>

<profiles>
    <profile>
        <id>dev</id>
        <properties>
            <!-- 环境标识，需要与配置文件的名称相对应 -->
            <profiles.active>dev</profiles.active>
            <logging.level>debug</logging.level>
            <ruoyi.profile>${user.dir}/upload</ruoyi.profile>
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
            <ruoyi.profile>~/code/upload</ruoyi.profile>
        </properties>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <profiles.active>prod</profiles.active>
            <logging.level>warn</logging.level>
            <ruoyi.profile>>~/code/upload</ruoyi.profile>
        </properties>
    </profile>
</profiles>
```

``` yml
ruoyi:
  profile: @ruoyi.profile@ 

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