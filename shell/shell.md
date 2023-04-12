# 变量

## 变量类型

- 环境变量
  系统已经定义好, 默认存在的变量, 可以用于获取系统中的一些关键信息, 如: `$HOME`, `$PWD`, `$USER`
- 局部变量
  shell定义变量不需要声明变量的类型, 如: `var_name=value`
- shell变量

## 变量操作

读取变量, 使用shell变量需要在变量名前添加`$`, 如: `echo $var_name`

只读变量, 使用`readonly`将变量定义为只读, 如: `readonly var_name`; 只读变量在被修改时会报错

```shell
my_var="www.baidu.com"
readonly my_var
#抛出异常
my_var="www.google.com"
```

删除变量, 使用`unset`删除变量, 如: `unset var_name`; 被删除的变量不能再次使用. `unset`不能删除只读变量

## shell字符串

```shell
# 单引号内的任何字符都会原样输出, 单引号的中的变量是无效的
# 单引号字串中不能出现单独一个的单引号(对单引号使用转义符后也不行), 但可成对出现, 作为字符串拼接使用
my_str='this is a string'

# 双引号可以有变量
# 双引号可以出现转义字符
var_name="shell"
my_str="hello, $var_name"

# 获取字符串长度
my_str_len=${#my_str}
# 提取字符串
${my_str:1:4}
```

## shell数组

定义数组:

```shell
array_name=(value_0 value_1 ... value_n)
array_name[n]=value_n
```

读取数组:

```shell
# 一般读取格式
${array_name[n]}
# 获取所有数组元素
${array_name[@]}
# 获取数组的长度
${#array_name[@]}
${#array_name[*]}
# 获取单个元素的长度
${#array_name[n]}
```

# 条件判断

`[ condition ]` （注意condition前后都要有空格，否则报错）

常用的条件判断：
	整数的判断：
	=字符串比较
	-lt小于（less than）
	-le小于等于（less equal）
	-eq等于（equal）
	-gt大于（greater than）
	-ge大于等于（greater equal）
	-ne不等于（not equal）

	权限判断：
	-r 有读权限（read）
	-w写权限（write）
	-x有执行权限（execute）
	
	按照文件类型判断
	-f文件存在并且是一个常规文件（file）
	-e文件存在（existence）
	-d文件存在并且是一个目录（directory）
	
	多条判断
	&&当前命令执行成功，才执行后一条
	||上一条命令执行失败后，才执行下一条命令