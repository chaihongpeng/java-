# HDFS-API

## 部署

```xml
<dependency>
	<groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-client</artifactId>
    <version>3.3.3</version>
</dependency>
```

## 调用

```java
//配置访问地址
URI uri = new URI("hdfs://iZ2ze1k1nrm6duwvkwfqo6Z:8020");
//设置配置文件
Configuration configuration = new Configuration();
//访问用户
String user = "root";
//创建客户端
FileSystem fs = FileSystem.get(uri, configuration, user);
//发送一个创建目录的指令
Path path = new Path("/java/first-dir");
fs.mkdirs(path);
//关闭资源
fs.close();
```

