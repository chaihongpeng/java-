target: dependencies
    command

- dependcies: 文件依赖
- \<Tab\>command

```makfile
// 编译test.c文件脚本
test: test.c
    gcc test.c -o test
```

# 变量

```makefile
# 定义变量
VAR_1="all"
VAR_2="/tset"
# 使用变量
$(VAR_1): $(VAR_2)
# 变量赋值
```
