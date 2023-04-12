```python
# 重命名
os.rename(<src:源文件>,<dst:目标文件>)
# 删除文件
os.remove(<path:删除文件>)
# 创建文件夹, 不可以级联创建文件夹
os.mkdir(<path:文件夹>)
# 删除空文件夹
os.rmdir(<path:文件夹>)
# 创建文件夹, 可以级联创建目录
os.mkdirs(<path>)
# 级联删除文件夹
shutil.rmtree(<path>)
```
