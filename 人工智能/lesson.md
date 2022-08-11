- 数据集(data set): 一组西瓜
- 示例(instance)/样本(sample): 一个西瓜
- 属性(attribute)/特征(feature): 色泽, 声响; key
- 属性值(attribute value): 绿色, 清脆; value
- 属性空间(attribute space)/样本空间(sample space)/输入空间: 多属性形成一个坐标轴, 每一个西瓜在坐标轴上都有自己的坐标位置

> 由于空间中每一个点对应一个坐标向量, 因此我们把一个示例称为一个特征向量(feature vector)

- 一般的令$D=\{x_1,x_2,\cdots,x_m\}$表示有$m$个示例的数据集
  - $D$代表数据集$data\ set$
  - $x$代表示例
- 示例由$d$个属性描述, 每个示例$x_i=(x_{i1};x_{i2};\cdots;x_{i_d})$是$d$维样本空间$X$中的一个向量,$x\in X$
  - $d$代表维数(dimensionality)
  - $x_{i}$代表示例(instance/sample)
- 其中$x_{ij}$是$x_i$在第$j$个属性上的取值



- 学习(learning)/训练(training): 从数据中学得模型的过程
- 训练数据(training data): 训练过程中使用的数据
- 训练样本(training sample): 训练数据中的每一个样本成为训练样本
- 训练集合(training set): 训练样本组成的集合称为训练集
- 假设(hypothesis): 学得模型对应了关于数据的某种潜在规律
- 真相/真实(ground-truth): 这种潜在规律称为真相或真实
- 学习器(learner): 模型
- 预测(prediction)
- 标记(label): 事例的结果
- 样例(example): 拥有标记信息的示例
- 一般用$(x_i,y_i)$表示第$i$个样例, 其中$y_i\in Y$表示是示例$x_i$的标记,$Y$是所有标记的集合
- 标记空间(label space)/输出空间: 所有标记集合



- 分类(classification): 预测离散值的学习任务
  - 二分类(binary classification): 只涉及两个类别, 一般为"正类"和"负/反类"
  - 多分类(multi-class classification): 涉及多个类别
- 回归(regression): 预测连续值的学习任务



- 聚类(clustering): 对训练集进行分组
- 组(cluster): 每个组



- 监督学习(supervised learning)
- 无监督学习(unsupervised learning)



- 泛化(generalization): 学得模型适用于新样本的能力



## 假设空间

科学推理的两大基本手段

- 归纳(induction):从特殊到一般的泛化过程, 及从具体的事实归结出一般性规律
- 演绎(deduction): 从一般到特殊的"特化"过程, 及从基本原理推演出具体状况



- 归纳学习(inductive learning): 从样本中学习是一个归纳的过程, 因此亦称为归纳学习
    - 广义归纳学习: 从样例中学习
    - 狭义归纳学习: 从训练数据中学得概念(concept), 因此亦称为"概念学习或"概念形成"

​	我们把学习过程看作一个在所有假设(hypothesis)组成的空间中进行搜索的过程,  搜索目标是找到与训练集"匹配"(fit)的假设, 即能够将训练集中的瓜判断正确的假设

​	可能有多个假设与训练集一致, 即存在一个与训练集一直的"假设集合", 亦称为"版本空间"

## 线性模型

给定$d$个属性描述示例$x=(x_1;x_2;\cdots;x_i)$其中$x_i$是$x$在第$i$个属性上的取值

## 决策树

信息熵 $Ent(D)=-\sum\limits_{k=1}^{|Y|}p_k\log_2p_k$

## 神经网络

- 神经网络(neural network)

- 神经元(neuron): 神经网络中最基本的成分

- 阈值(threshold): 神经元的点位超出了阈值, 它就会被激活

M-P神经元模型

​	神经元接收到n个来自其他神经元的传递过来的输入信号, 这些输入信号将通过带权重的连接(connection)进行传递, 神经元接受到的总输入值将与神经元的阈值进行比较, 然后通过"激活函数"(activation function)处理以产生神经元的输入

感知机与多层网络

感知机(Perceptron)由两层神经元组成, 输入层接受外界的输入信号后传递给输出层, 输出层是M-P神经元, 亦称为"阈值逻辑单元"(threshold logic unit)

感知机能够容易的实现逻辑与或非运

$y=f(\sum_iw_ix_i-\theta_j)$

与 $y=f(x_1+x_2-2)$ 条件$w_1=w_2=1,\theta=2$

或 $y=f(x_1+x_2-0.5)$ 条件$w_1=w_2=1,\theta=0.5$

非 $y=f(-0.6x_1+0.5)$ 条件$w_1=-0.6,w_2=0,\theta=-0.5$







