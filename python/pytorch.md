# Tensor

用于存数据, 可以存标量,向量,矩阵

## data

用于权重本身的值

## grad

gradient, 用于保存损失函数对权重的导数

## 创建矩阵

```python
# 创建一个没有初始化5行3列的张量
x= torch.empty(5,3)
print(x)
```

```python
# 创建一个初始化的张量
torch.rand(5,3)
```

```python
# 创建一个全零张量
torch.zeros(5,3,dtype=torch.long)
```

```python
# 直接通过数组创建张量

torch.tensor([[1,2],[3,4]])
```

```python
# 获取张量的尺寸
x.size()
```

## 操作举证

## autograd属性

torch.Tensor类是整个package中的核心类, 如果将属性.requires\_grad设置为True,他将追踪在这个类上的所有操作.
当代码要进行反向传播的时候, 我们直接调用.backward()就可以自动计算所有的梯度.在这个Tensor上的所有的梯度将被累加进属性.grad中

如果想终止一个Tensor在计算图中的回溯, 只需要执行.detach()就可以将该Tensor从计算图中撤下来, 在未来的回溯计算中也就不会再计算该Tensor了

也可以采用代码块的方式with torch.no\_grad()终止对计算图的回溯


torch.Function类,每个Tensor拥有一个.grad\_fn属性, 代表引用了哪个具体的Function创建了该Tensor

```python
x=torch.ones(2,2,requirs_grad=True)
```

pytorch中,方法末尾带有\_, 表示该方法可以原地改变Tensor的属性, requires\_grad如果没有主动设置,默认为False

```python
a=torch.rand(2,2)
a=a*3
print(a.requires_grad)
# print False
a.requires_grad_(True)
print(a.requires_grad)
# print True
```

## 反向传播

```python
out.backward()	
print(x.grad)
```
