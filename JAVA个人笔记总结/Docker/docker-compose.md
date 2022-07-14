单机多容器的部署工具

#### Install

官网:https://docs.docker.com/compose/install/
获取docker-compose同时完成自动安装过程

```shell
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
```

#### Getting Started

1. 编写自己的`dockerFile`

2. 创建docker-compose.yml的配置文件，名字是固定的不可以更改
   ```yaml
   version: "3.9"
   services:
     web:
       # 使用/opt/soft目录下的dockerFile文件进行项目构建
       build: /opt/soft/
       # 指定本机端口映射到外部端口
       ports:
         - "80:8080"
       # 数据卷挂载, 将本机目的当前目录./挂载到/code目录
       volumes:
         - ".:/code"
       # 设置环境变量
       environment:
         JAVA_HOME: /opt/modules/java-11
     readis:
       image: "redis:apline"
   ```
3. 运行docker-compose命令
   ```shell
   #运行docker-compose脚本
   docker compose up
   ```
   
#### Compose ENV
在编写docker-compose.yml文件时, 可以使用`${JAVA_HOME}`来引用`.env`文件中的环境变量

```properties
JAVA_HOME=/opt/modules/java-11
```



