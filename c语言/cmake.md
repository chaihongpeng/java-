# 常用函数和方法

## PROJECT

```cmake
PROJECT(PROJECT_NAME C CXX)
```

用来指定工程的名字和支持的语言

- MY_PROJECT指定工程项目的名称
- C CXX代表此项目支持C和C++语言
- 该指定隐式定义了CMAKE的变量
  - <PROJECT_NAME>_BINARY_DIR
  - <PROJECT_NAME>_SOURCE_DIR

## SET

```cmake
SET(SRC_LIST mian.cpp)
```

用来显示的指定变量

变量通过`${}`方式取值

## MESSAGE

```cmake
MESSAGE()
```

- `SEND_ERROR`产生错误,生成过程被跳过
- `SATUS`输出前缀为`--`的信息
- `FATAL_ERROR`立刻终止所有`cmake`过程

## ADD_EXECUTABLE

```cmake
ADD_EXECUTABLE(filename main.cpp)
```

生成可执行文件

- `filename`用来指定可执行文件的名称
- `SRC_LIST`

# 语法基本原则

```cmake
指令(参数1 参数2)
```

指定参数使用括号括起,参数之间使用空格或分号分开

指令是大小写无关的, 参数和变量是大小写相关

`SET(KEY,"VALUE")`参数可以选择加双引号,如果参数中有空格就必须要加双引号

`ADD_EXECUTAVLE(filename, main)`后面的main可以不加后缀,它会自动去寻找c和cpp文件,但不推荐这样写,因为可能同时存在main.cpp和main.c文件