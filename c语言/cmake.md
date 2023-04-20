# 常用函数和方法

## PROJECT

```cmake
project(<my_project_name> <[C]> <[CXX]> <[JAVA]>)
PROJECT(MY_PROJECT_NAME C CXX)
```

用来指定工程的名字和支持的语言

- MY_PROJECT_NAME指定工程项目的名称
- C CXX代表此项目支持C和C++语言
- 该指定隐式定义了CMAKE的变量
  - <PROJECT_NAME>_BINARY_DIR
  - <PROJECT_NAME>_SOURCE_DIR

## SET

```cmake
set(SRC_LIST main.cpp a.cpp b.cpp)
```

用来显示的指定变量

变量通过`${}`方式取值

## MESSAGE

```cmake
message()
```

向终端输出自定义的信息

- `SEND_ERROR`产生错误,生成过程被跳过
- `STATUS`输出前缀为`--`的信息
- `FATAL_ERROR`立刻终止所有`cmake`过程

## ADD_EXECUTABLE

```cmake
add_excutable(<filename> <main.cpp>)
```

生成可执行文件

- `filename`用来指定可执行文件的名称
- `SRC_LIST`原文件读取的地方

## ADD_SUBDIRECTORY

```cmake
add_subdirectory(<src> <bin>)
```

添加关联目录

- `src`表示关联的目录位置
- `bin`生成的可执行文件全部添加到此目录下,`bin`可以自动创建

# 语法基本原则

```cmake
指令(参数1 参数2)
```

指定参数使用括号括起,参数之间使用空格或分号分开

指令是大小写无关的, 参数和变量是大小写相关

`SET(KEY "VALUE")`参数可以选择加双引号,如果参数中有空格就必须要加双引号

`ADD_EXECUTAVLE(filename main)`后面的main可以不加后缀,它会自动去寻找c和cpp文件,但不推荐这样写,因为可能同时存在main.cpp和main.c文件

## 变量使用

变量的使用`${}`的方式读取变量中的内容. `IF`控制语句中是直接使用变量名称

> windows如果使用MinGW, cmake命令需要增加参数
>
> ```sh
> cmake -G"MinGW Makefiles" .
> ```

```cmake
cmake_minimum_required(VERSION 3.21)

project(HELLO)
set(CMAKE_CXX_STANDARD 14)
set(SRC_LIST main.cpp)
MESSAGE(STATUS "this is binary dir" ${HELLO_BINARY_DIR})
MESSAGE(STATUS "this is source dir" ${HELLO_SOURCE_DIR})
add_executable(hello ${SRC_LIST})
```

```sh
cmake -G "MinGW Makefiles" -B ./build .
```

# 安装

```sh
make install DESTDIR=<path>
```

`path`指定安装的目录

`./configure -prefix=<paht>`

```cmake
project(HELLO)
add_subdirectory(src bin)
install(FILE README.md DESTINATION share/doc/cmake/)
install(PROGRAMS run.sh DESTINATION bin)
install(DIRECTORY doc/)
```

目录是否以`/`结尾差别很大, 如果不实用`/`结尾,则将整个目录放入到目标文件夹, 如果以`/`结尾则只安装文件夹中的内容
