```python
import numpy as np
```

# 构建一个数组

````python
array = np.array([1, 2, 3, 4])
````



# 数组+1

```python
array + 1
```

- 数组+1,所有元素都加1

# 数组*1

```pytho
array * 2
```

- 数组乘2,所有元素都乘2

# 数组相乘

```python
array * array
```

- 相同坐标位置的元素做乘法
- 形状对应不上的数组会报错

# 数组的形状

```python
array.shape
np.shape(array)
```

- 用于描述数组的形状
- 返回值是一个元组,用于描述数组的维度和每个维度的元素数

# 数组的元素类型

```python
array.dtype
```

- 用于描述数组内的类型
- 对于numpy数组,数组内的元素类型必须是一致的
- 返回值是一个dtype对象

# 数组元素所占字节数

```python
array.itemsize
```

- array里面的每个元素所占的字节数

# 数组中一共有多少个元素

```python
array.size
np.size(array)
```

- 用于查看数组中一共有多少个元素

# 查看数组整体占内存大小

```python
array.nbytes
```

- 实际array对象的大小要比这个数据大,因为对象要额外保存数组的形状数据类型等信息

# 查看数组的维度

```python
array.ndim
```

# 定义

- 数组的形状和类型一旦确定,就不允许随意修改数组的类型
- 如`array.fill(7.3),最终数组中的元素都会替换为7而不是7.3,因为在创建时已经定义为int32类型了`

# numpy array的数据类型

```python
np.array([],dtype=int)
```

- 创建数组是强行指定元素的类型
- numpy可以指定的类型
  - python中默认已有的类型
    - bool
    - int
    - float
    - complex
    - str
  - np中提供的数据类型
    - np.float32
    - np.float64
    - np.int32
    - np.int64
  - 通过字符串指定类型:"float","int32"
  - 最通用类型:object
  - numpy的特殊类型
    - inf: 无穷大,numpy中array/0,1/0=inf,-1/0=-inf
    - nan:不合法的类型,0/0=nan

# 转化数组类型

```python
np.asarray(array,dtype="float")
np.array(array,dtype="float")
array.astype(float)
```

- array()方法会产生一个新的数组
- asarray()会判断原来的数组是否与现有的数组定义发生过改变,如果没有改变则直接返回原来的数组,否则创建一个新的
- astype()方法不会去改变原来的数组,也是产生一个新的数组

# 生成特殊类型的数组

```python
np.zeros((2,3))
```

- 生成指定类型的全零数组
- 参数为元组,用于确定数组的形状

```python
# 生成一个全1的数组
np.ones((2,3))
# 生成一个从1到10的数组,间隔为2
np.arange(1,10,2)
# 生成等间距数组,从0开头,10结尾,一共指定5个数
np.linspace(0,10,5)
```

# 数组的索引

```python
# 获取第0个元素
array[0]
# 获取最后一个元素
array[-1]
# 获取从第1个元素到最后一个元素
array[1:]
```

# 元素迭代

```python
# 迭代array数组,获取维度内的元素
for item in array:
    print(item)
# 打印array数组的所有元素值value,和对应的坐标idx
for idx, value in np.ndenumerate(array):
    print(idx, value)
```

# 数组的操作

- 求和

```python
# 数组元素的求和
array.sum()
# 在某一个维度上求和,axis=0就是同行相加,求每行的和
array.sum(axis=0)
```

- 求每行最大值的位置

```python
# 求某一维度的最大值
array.argmax(axis=1)
```



- 求每行最小值的位置

```python
# 求某一维度的最小值
array.argmin(axis=1)
```

- 求平均值

```python
# 求某一维度的平均值
array.mean(axis=1)
# 求全局的平均值
array.mean()
```

- 额外添加维度

```python
array[None,:,:]
```

- 转置

```python
array.T
```

- 数组的拼接

```python
# 将数组a和b拼接
np.concatenate([a,b])
```











