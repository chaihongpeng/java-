# yarn

- yarn是一个资源调度平台,负责为运算程序提供服务器运算资源,相当于一个分布式的操作系统,而MapReduce等于运行在yarn上的应用程序

## ResourceManager(RM)

- 管理整个集群的资源调度

## NodeManager(NM)

- 管理单台服务器的资源

## ApplicationMaster(AM)

- 为应用程序申请资源并分配给内部的任务

## Container

- 运行资源的程序