# DockerFile

#### FROM

指定基础镜像, 当前新镜像基于那个镜像, 指定一个已经存在的镜像作模板, 第一条必须是from

```dockerfile
FROM centos:centos7
```

#### MAINTAINER

指定作者的名字和邮箱

```dockerfile
MAINTAINER chp
```

#### RUN

容器构建时需要的命令

命令格式有两种

- shell格式: RUN <命令行> 等同于终端操作shell命令
- exec格式: RUN ["yum", "-y", "install", "vim"]

```dockerfile
# 在镜像中安装一个vim
RUN yum -y install vim
RUN ["yum", "-y", "install", "vim"]
```

#### EXPOSE

当前容器对外暴露端口

```dockerfile
# 主机端口:容器端口
EXPOSE 8080:8080
```

#### WORKDIR

指定默认登录的工作目录

```dockerfile
# 指定每次进入容器的默认目录
WORKDIR /opt/modules/hadoop
```

#### ENV

用来在构建镜像过程中设置环境变量

```dockerfile
ENV JAVA_HOME /opt/modules/java
```

#### ARG

指定定义一个变量, 用户可以在构建时使用

```dockerfile
ARG custom_arg=custom_value
```

#### VOLUME

容器卷

```dockerfile
# 容器路径, 为了Dockerfile的可移植性, Dockerfile不支持指定宿主机的路径
VOLUME /opt/modules/hadoop/node-01/data
```

#### ADD

将宿主机的文件拷贝到容器且会自动处理URL和压缩包

#### COPY

向容器中拷贝文件, 但是不带有解压和处理URL功能

#### CMD

指定容器启动后要做的事情

支持run和exec

容器中可以有多个`CMD`命令, 但是只最后一个会生效, `CMD`会被`docker run `之后的参数替换

```dockerfile
CMD nginx -c /etc/nginx/nginx.conf
```

#### ENTRYPOINT

类似于`CMD`, 也是在容器运行时执行, 但是他不会被run后面的命令覆盖, 而`CMD`编程`ENTYPONT`的参数进行传递

命令格式变为 `ENTRYPOINT + CMD`

```dockerfile
# 当执行docker run -itd <image-name>是相当于容器中默认执行了nginx -c /etc/nginx.conf
# 如果用户在执行run方法时, 指定了指定参数run -itd <image-name> /etc/new-nginx.conf 则相当于将CMD部分的内容替换为自身指定的内容
CMD ["/etc/nginx/nginx.conf"]
ENTRYPOINT ["nginx", "-c"]
```

