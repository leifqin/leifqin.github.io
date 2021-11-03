**背景**：按照web发布安全要求，需要禁止JBoss的默认管理页面访问
**解决办法**：

#### JMX-CONSOLE请删除

```
$JBoss_home/server/default/deploy/jmx-console.war
```

#### WEB-CONSOLE删除

```
$JBoss_home/server/default/deploy/management/console-mgr.sar/web-console.war
```

#### STATUS禁止访问，请修改配置

```xml
$JBoss_home/server/default/deploy/ROOT.war/WEB-INF/web.xml
```

``` xml
<?xml version="1.0" encoding="ISO-8859-1"?>

<!DOCTYPE web-app
    PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
    "http://java.sun.com/dtd/web-app_2_3.dtd">

<web-app>
  <display-name>Welcome to JBoss</display-name>
  <description>
     Welcome to JBoss
  </description>
<!-- 注销以下模块
  <servlet>
    <servlet-name>Status Servlet</servlet-name>
    <servlet-class>org.jboss.web.tomcat.service.StatusServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>Status Servlet</servlet-name>
    <url-pattern>/status</url-pattern>
  </servlet-mapping>
-->
</web-app>
```