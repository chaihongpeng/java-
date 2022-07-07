# docker0

Docker服务启动后会自动创建`docker0`的网桥, 它在内核层连接了物理和虚拟的网卡, 这就将所有容器和本机主机都放在了同一个物理网络上. 

# docker network 网络命令

```shell
# 查看docker网桥列表
docker network ls
# 创建网桥
docker network create <new-network-name> \ 
	--network <birge|host|none|container>
# 连接网桥
docker network connect
# 删除所有不在用的网络
docker network prune
# 删除网络
docker network rm <network-name>
# 查看网桥信息
docker network inspect <network-name>
```

```shell
# 指定创建容器时指定网桥
docker run -itd \
	--network bridge # 指定使用的网桥模式
	--name node-01
	centos:centos7
```



# 常见的网桥模式

### bridge
为每一个容器分配, 设置IP等, 并将容器连接到一个`docker0`
虚拟网桥默认使用该模式

网桥模式下, 每个容器的`eth0`都和`docker0`网桥上有一个`veth`两两匹配, 容器和容器间可以通过`docker0`相互通讯, 也可以与物理网卡通讯

### host

容器将不在虚拟出自己的网卡, 配置自己的IP等, 而是使用宿主机的IP和端口

因为直接使用主机的IP, 所以容器不在需要端口映射, 容器会直接占用主机的端口

### none

容器有独立的Network namespace, 但是并没有对其进行独立的网络设置, 如分配veth pair和网桥连接, IP等

未配置任何网络设置, 只有一个本地的回环地址, 需要我们自己添加网卡和IP配置

### container

新创建的容器不会创建自己的网卡和配置自己的IP, 而是和指定的容器共享IP, 端口范围等

制定容器和另一个指定的容器共享IP和端口

### custom

