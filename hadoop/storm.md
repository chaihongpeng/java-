- 有向无环图
  - 计算DAG的拓扑关系
    - 1 --> 4 代表 4的入度+1, 4是1的邻接点
    - 首先将边与边的关系确定, 建立好入度表和邻接表
    - 从入度为0的点开始删除
    - 判断有环无环的方法, 对入度数组遍历, 如果有的点入度不为0, 则表明有环

#### Storm的组件

- Tuple: 主要数据结构, 有序元素列表. 支持所有数据类型. 通常被建模为一组逗号分隔的值, 并传递到storm集群中
- Stream: 流是元组的无序序列
- Spouts: 流的源, Storm从原始源Kafka, Twitter, Kestrel等队列中输入数据. ISpout是spouts的核心接口, 一些特定的接口实现IRichSpout，BaseRichSpout，KafkaSpout等. 也可以自定义Spouts读取数据源
- Bolts: 逻辑处理单元, Spouts将数据传递到Bolts. Bolts可以执行过滤, 聚合, 加入, 与数据源和数据库交互的操作. Bolts接受数据并发送给一个或多个Bolts. IBot还bots的核心接口