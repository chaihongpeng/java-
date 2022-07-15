# HDFS的组成

## NameNode

- NameNode就是Master,它就是一个主管,管理者.
  - 管理HDFS的名称空间
  - 配置副本策略
  - 管理数据块(Block)映射信息
  - 处理客户端的读写请求

## DataNode

- DataNode就是Slave,NameNode下达命令,DataNode执行实际的操作
  - 存储实际的数据块
  - 执行数据块的读写操作

## Client

- Client就是客户端
  - 文件切分,上传文件时,Client将文件切分成一个一个的文件块
  - 与NameNode交互,获取文件的位置信息
  - 与DataNode交互,读取或写入数据
  - 提供一些命令管理HDFS,比如NameNode格式化

# HDFS的底层

## 文件块大小

- 存储文件的基本单元
  - 寻址时间为传输时间的1%为最佳状态