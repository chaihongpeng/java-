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
        // Text存储类是可以复用的
        final Text keyOut = new Text();
        final IntWritable valueOut = new IntWritable();
        
        @Override
        protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
            String line = value.toString();
            String[] words = line.split(" ");
            for (String word : words) {
                keyOut.set(word);
                valueOut.set(1);
                context.write(keyOut, valueOut);
            }
        }
    }
    ```
  - 编写reducer
    ```java
    package com.chp.kafka;
    
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Reducer;
    
    import java.io.IOException;
    
    public class ReducerLesson extends Reducer<Text, IntWritable, Text, IntWritable> {
        IntWritable valueOut = new IntWritable();
    
        @Override
        protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable value : values) {
                sum += value.get();
            }
            valueOut.set(sum);
            context.write(key, valueOut);
        }
    }
    ```
  - 编写driver
    ```java
    package com.chp.kafka;
    
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    
    import java.io.IOException;
    
    public class DriverLesson {
        public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
            //1. 获取job
            Configuration configuration = new Configuration();
            Job job = Job.getInstance(configuration);
            //2. 设置Jar路径
            job.setJarByClass(DriverLesson.class);
            //3. 关联mapper和reducer
            job.setMapperClass(MapperLesson.class);
            job.setReducerClass(ReducerLesson.class);
            //4. 设置map输出的k,v类型
            job.setMapOutputKeyClass(Text.class);
            job.setMapOutputValueClass(IntWritable.class);
            //5. 设置最终输出的k,v类型
            job.setOutputKeyClass(Text.class);
            job.setOutputValueClass(IntWritable.class);
            //6. 设置输入路径和输出路径
            FileInputFormat.setInputPaths(job, new Path("D:\\test\\test.txt"));
            FileOutputFormat.setOutputPath(job, new Path("D:\\test\\output.txt"));
            //7. 提交job,可以直接使用submit,waitForCompletion在提交后仍会获取处理的信息
            boolean result = job.waitForCompletion(true);
            System.exit(result ? 0 : 1);
        }
    }
    ```












