# MapReduce

#### MapReduce优缺点
  - 优点:
    1. 易于编程: 用户只关心业务逻辑. 实现框架的接口
    2. 良好的扩展性: 可动态增加服务, 解决资源不够问题
    3. 高容错性: 任何一台机器挂掉了,可以将任务转移到其他节点
    4. 适合海量数据计算(TB/PB): 几千台服务器同时计算,
  - 缺点
    1. 不擅长实时计算. Mysql
    2. 不擅长流式计算. SparkStreaming Flink
    3. 不擅长DAG有项无环图计算 Spark

#### MapReduce进程
  1. MrAppMaster: 负责整个程序的过程调度及状态协调
  2. MapTask: 负责Map阶段的整个数据处理流程
  3. ReduceTask: 负责Reduce阶段数据处理流程

#### MapReduce编程规范
  - 用户程序编写分为三部分: Mapper, Reducer, Driver
    1. Mapper阶段
       - 继承自己的Mapper
       - Mapper输入是KV对的形式(<K,V>的类型可以自定义)
         - K是偏移量
         - V对应一行的内容
       - Mapper的对应逻辑在重写map()方法中实现
       - Mapper的输出数据是<K,V>对的形式(<K,V>的类型可以自定义)
       - map()方法(MapTask进程)对每一个<K,V>调用一次
    2. Reducer阶段
       - 用户自定义的Reducer要继承自己的父类
       - Reducer输入的类型对应Mapper的输出数据,也是<K,V>
       - Reducer的业务逻辑卸载reduce()方法里面
       - (ReduceTask进程)对每一组相同K的<K,V>组调用一次reduce()方法
    3. Driver阶段
       - 相当于Yarn的客户端,用于提交我们的整个程序到Yarn集群中,提交的是封装的MapReducer程序相关运行参数的Job对象
#### 编写程序
  - 搭建环境
    1. 引入依赖
    ```xml
    <!--客户端的version一定要和集群的version保持一致-->
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-client</artifactId>
        <version>3.3.3</version>
    </dependency>
    ```
  - 编写mapper
    ```java
    //引入依赖;注意org.apache.hadoop.mapred.Mapper对应的是hadoop1版本的接口,请勿导错包
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.Text;

    //其中KEYIN, VALUEIN, KEYOUT, VALUEOUT分别对应偏移量,行内容,输出Key,输出Value    
    public class MapperLesson extends Mapper<LongWritable, Text, Text, IntWritable> {
        
    }

    ```












