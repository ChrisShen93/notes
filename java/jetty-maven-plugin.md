# jetty-maven-plugin

标签（空格分隔）： maven jetty

---

## 快速开始

将下面的内容添加进pom.xml中：
```xml
<plugin>
  <groupId>org.eclipse.jetty</groupId>
  <artifactId>jetty-maven-plugin</artifactId>
  <version>9.3.12.v20160915</version>
</plugin>
```

运行命令：
```sh
mvn jetty:run
```

待服务启动后，可以访问 http://localhost:8080/ 看到自己的工程项目
jetty会周期性地扫描工程中任何文件的改变并重新加载它们，这样用户可以立刻看到自己所做的改动。

jetty帮助信息：
```sh
mvn jetty:help

mvn jetty:help -Ddetail=true -Dgoal= <goal name>
```
