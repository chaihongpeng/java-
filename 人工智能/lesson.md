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
  - $d$代表维数
  - $x_{i}$代表示例(instance/sample)
- 其中$x_{ij}$是$x_i$在第$j$个属性上的取值