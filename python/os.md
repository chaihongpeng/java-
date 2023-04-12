# OS模块

## 重命名文件
```python
os.rename(src="source.txt",dst="dst.txt")
```
  - src  原文件所在路径
  - dst  目标文件所在路径

## 删除文件

```python
# 被删除文件必须存在
os.remove(path="source.txt")
```

- path  文件所在路径

## 创建文件夹

```python
# 无法级联创建文件夹
# 创建的目录已经存在会报错
os.mkdir(path="dir_path")

# 可以级联创建文件夹
os.makedirs(name=r"dir_path\child_dir_path")
```

- path  创建文件夹的路径

## 删除空文件夹

```python
# 目标目录不存在会报错
# 目标文件夹内必须为空
 os.rmdir(path="dir_path")
```

- path  目标文件夹路径

## 级联删除文件夹

```python
shutil.rmtree(path="dir_path")
```

- path  目标文件夹路径

## 获取当前目录

```python
print(os.getcwd())
```

## 文件列表

```python
# 获取指定目录下的文件列表
os.listdir(path="dir_path")
# 
```

- path  指定目录

## os.path模块

```python
# 路径拼接
print(os.path.join(os.getcwd(), "source.txt"))
```



# 文件打开

```python
open(file="", mode="r", encoding=None, errors=None)
```

- file  参数用于表示要打开的文件, 意义是字符串或数字
- mode  文件模式
  - b  打开二进制文件
  - t  以文本文件模式打开文件
  - r  以只读模式打开文件
  - w  以只写模式打开文件, 不能读取文件.如果文件不存在, 则创建文件, 如果文件存在则覆盖文件内容
  - x  以独占模式打开文件, 如果文件不存在则创建并以写入模式打开; 如果文件已经存在, 则引发异常
  - a  以追加模式打开文件
  - \+ 表示更新模式,与r,w,a模式结合使用
  - `r,rt; w,wt; x,xt; a,at`          文本文件的对应操作
  - `rb; wb; xb; ab`                 二进制文件的对应操作
  - `r+; w+; a+; rb+; wb+; ab+`      以读写模式打开文件, r+文件不存在会抛出异常;w+可读写文件,文件不存在则创建文件;a+可读可以追加,文件不存在则创建文件字符串前加r可以防止反斜杠被转义 `r'C:\Users\Default\example.txt'`

# 文件关闭

```python
f_name="test.txt"
f=None
try:
  f = open(f_name)
  print("打开文件成功")
  content = f.read()
  print(content)
except FileNotFoundError as e:
  print("文件不存在")
except OSError as e:
  print("处理OSError异常")
finally:
  if f is not None:
    f.close()
    print("关闭文件成功")
```

```python
# with结束后,占用的资源就会被自动的释放掉
f_name="test.txt"
with open(f_name) as f:
  content = f.read()
  print(content)
```

# 文件读写

```python
f=open("test.txt","rt")
# 从文件中读取字符串
f.read(size=-1)
# 读取单行字符串,在读取到换行符或文本尾时返回单行字符串.如果已经到文本尾则返回一个空字符串
f.readLine(size=-1)
# 将所有数据读取进内存, 并放入到列表中, 每一行都是列表中的没有一个元素
f.readLines()

# 将字符串写入文件中, 并返回写入的字符数
f.write(str)
f.writeLines(lines)

f.flush
```

- size
  - 从文本中读取字符串, `size`限制读取的字符数, `size=-1`指对读取的字符数没有限制
  - 从文件中读取字节, `size`限制读取的字节数, `size=-1`则读取全部字节