## Document

单机多容器的部署工具
docker-compose概念组成

- 一个文件 docker-compose.yml
- 两个要素
  - 服务(service): 服务就是docker中的容器
  - 工程(project): 多个服务组成的一个完整的业务单元

## Install

官网:https://docs.docker.com/compose/install/
获取docker-compose同时完成自动安装过程

```shell
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
```

## Getting Started

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

```dotenv
JAVA_HOME=/opt/modules/java-11
```

#### Command-line

```sh
# 打印docker-compose日志
# -f 追踪日志输出, 实时更新日志
# -t 展示时间戳
# --tail all 从每个容器的日志末尾开始显示的行数
# --until 在时间戳之前显示日志
docker compose logs

# 重新构建, 启动和附加到容器
# --detach -d 后台运行
docker compose up

# 构建或重建服务
docker compose build

# 停止容器并且移除由up命令创建的容器, 网络, 数据卷和镜像
docker compose down 
```

#### docker-compose.yml

```yaml
verson: 3 #docker compose的版本

services: # 服务列表
  webapp: #指定服务名
    build: . #使用指定路径的Dockerfile文件进行构建
    
    build:
      # 指定Dockerfile文件所在目录
      context: .
      # 指定Dockerfile文件的别名
      dockerfile: webapp.Dockerfile
      # args定义Dockerfile的构建参数
      # ARG HADOOP_DIR
      # RUN echo $HADOOP_DIR
      args:
        # 写法一
        HADOOP_DIR: /opt/moduls/hadoop-3.3.3
        # 写法二
        - HADOOP_DIR=/opt/moduls/hadoop-3.3.3
      ssh:
        - def
      
```

## Compose file

默认读取顺序`compose.yaml`,`compose.yml`,`docker-compose.yaml`,`docker-compose.yml`

### 环境Profiles

```yaml
services:
  foo:
    image: foo
  bar:
    image: bar
    profiles:
      - test
  baz:
    image: baz
    depends_on:
      - bar
    profiles:
      - test
  zot:
    image: zot
    depends_on:
      - bar
    profiles:
      - debug
```

### 顶级命名

```yaml
services:
  foo:
    image: busybox
    environment:
      - COMPOSE_PROJECT_NAME
    command: echo "I'm running ${COMPOSE_PROJECT_NAME}"
```

### 顶级元素

```yaml
# 顶级元素
services:
	# 顶级命名
  frontend:
    image: awesome/webapp
    networks:
      - front-tier
      - back-tier
# 顶级元素
volumes:
	# 顶级命名
  db-data:
    driver: flocker
networks:
  front-tier: {}
  back-tier: {}
```

## Docker

### container_name

指定自定义容器的名称

### image

指定启动容器的镜像

### command

覆盖容器镜像的默认命令(Dockerfile CMD)

### environment

定义容器中的环境变量

# Compose命令

```sh
# 用法
docker compose [options] [command] [args...]
```

## Options

```sh
docker compose -f,--file FILE #指定一个compose文件,默认使用docker-compose.yml
```

## Commands

```sh
# 启动[service]服务, 并以[command]命令运行
docker compose run [options] [service] [command] [args...]
```

```sh
# 启动或重启所有定义的服务
docker compose up [options] [service...]
# options
-d --detach # 后台运行程序
```

```sh
# 停止容器并且移除所有通过up创建的容器,镜像,网络,数据卷
docker compose down [options]
-v --volumes # 移除compose声明的数据卷和容器的匿名数据卷
```

