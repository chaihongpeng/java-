# Hadoop安装

1. 下载解压

## Hadoop核心目录

- bin:

  - hdfs命令
  - yarn命令
  - mapred命令

- etc:

  - hdfs-site.xml存储相关配置
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <configuration>
        <!--nn web端访问地址-->
        <property>
            <name>dfs.nanmenode.http-address</name>
            <value>127.0.0.1:9870</value>
        </property>
        <!--2nn web端访问地址-->
        <property>
            <name>dfs.namendoe.secondary.http-address</name>
            <value>127.0.0.1:9868</value>
        </property>
    </configuration>
    
    ```

  - mapred-site.xml计算相关配置
    ```xml
    <?xml version="1.0"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <configuration>
        <!--指定mapreduce程序运行在yarn上-->
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
    </configuration>
    ```

  - yarn-site.xml资源调度相关配置
    ```xml
    <?xml version="1.0"?>
    <configuration>
        <!--指定MR走shuffle-->
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce-shuffle</value>
        </property>
        <!--指定Resource的地址-->
        <property>
            <name>yarn.resourcemanager.hostname</name>
            <value>localhost</value>
        </property>
        <!--环境变量继承-->
        <property>
            <name>yarn.nodemanger.env-whilelist</name>
            <value>
                JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME
            </value>
        </property>
    </configuration>
    
    ```

  - core-site.xml整体核心配置

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <configuration>
        <!--指定NameNode地址-->
        <property>
            <name>fs.defaultsFS</name>
            <!--注意在新的版本中此参数被更换为-->
            <name>fs.default.name</name>
            <value>hdfs://127.0.0.1:8020</value>
        </property>
        <!--指定hadoop数据存储目录-->
        <property>
            <name>hadoop.tmp.dir</name>
            <value>/myfile/hadoop-3.3.3/hdfs-data</value>
        </property>
        <!--网页登录静态页的默认名称-->
        <property>
            <name>hadoop.http.staticuser.user</name>
            <value>chp</value>
        </property>
    </configuration>
    ```

    - workers配置每个集群host

    ```
    127.0.0.1
    ```

    

- sbin:
  - start-dfs.sh启动存储集群命令
  - start-yarn.sh启动资源调度器
- share:
  - 学习资料

## 启动集群

- 第一次启动初始化
  ```shell
  hdfs namenode -format
  ```

- 启动集群
  ```shell
  sbin/start-dfs.sh
  ```

  

