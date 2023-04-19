```shell
gcc -Wall hello.c -o hello
```

- `-o`: 指定输出文件的名称
- `-Wall`: warning all打印警告信息

```shell
# 双杠参数,就是guu发明的
gcc --version
```

>windows命令优先寻找当前目录,然后在在环境变量中寻找
>
>linux命令则不寻找当前目录,想要执行当前目录的可执行文件要在可执行文件前加上`./`

# 多文件编译

```sh
gcc -Wall main.c hello.c -o new_output_hello 
```

在`gcc`指令后指定多个文件,在编译时会将所有指定的文件都进行编译

```c
#include "hello.h"
#include <stdio.h>
// 两者的区别
//"hello.h" 上引号会先从当前目录寻找,如果找不到,就会去系统头文件目录去寻找
//<stdio.h>不会在当前目录去寻找, 而是直接去系统头文件目录去寻找
//系统头文件一般放在/usr/include或/usr/local/include目录下
```

# GCC选项

## -v

```sh
gcc -v -Wall hello.c
```

verbose compilation 冗长,打开后gcc会输出更详细的信息

# C语言编译过程

`.c` –> `.exe`

1. `-E`预处理: 把`.h`和`.c`文件展开, 形成一个文件. 宏定义就直接替换, 头文件和库文件就直接打开形成一个文件`.i`
2. `-S`编译: `.i`文件生成一个汇编代码文件`.S`
3. `-c`汇编: `.S`文件生成`.o`文件-->obj
4. `-o`链接: `.o`链接为`.exe`,`linux`下链接为`.elf`

预处理c语言的编译期叫做gcc, cpp的编译期叫做g++

预处理阶段, 宏定义做替换处理

编译将代码继续转化为汇编语言

# Makfile

1. 创建文档. 取名`Makefile`

2. #是注释

3. 显示规则
   ```
   目标文件: 依赖文件
   [TAB]指令
   ```

   ```makefile
   hello:hello.o
   	gcc hello.o -o hello
   hello.o:hello.S
   	gcc -c hello.S -o hello.o
   hello.S:hello.i
   	gcc -S hello.i -o hello.S
   hello.i:hello.c
   	gcc -E hello.c -o hello.i
   .PHONY:
   clear:
   	rm -rf hello.o hello.S hello.i
   ```

4. 第一个目标文件是最终目标, 编写makefile文件脚本书序与执行顺序是相反的

5. 伪目标, 不需要生成目标文件, 只是单纯要执行一件事情
   ```makefile
   .PHONY:
   clear:
   	rm -rf hello.o hello.S hello.i
   ```

   以上代码用于清除中间过程产生的临时文件

   # Makfile[2]变量

   ## 变量

   ```
   # 替换
   变量 = 值
   # 追加
   变量 += 值
   # 恒等于(常量定义)
   变量 := 值
   ```

   ## 变量的使用

   ```makefile
   CC:=gcc
   OBJ=circle.o
   OBJ+=cube.o
   OBJ+=main.o
   
   main:$(OBJ)
   	$(CC) $(OBJ) -o test
   circle.o:circle.c
   	$(CC) -c circle.c -o circle.o
   cube.o:cube.c
   	$(CC) -c cube.c -o cube.o
   main.o:main.c
   	$(CC) -c main.c -o main.o
   
   clean:
   	rm -rf $(OBJ) main
   ```

   

# Makefile[3]隐含规则

```makefile
# 任意的.c文件和任意的.o文件
%.c
%.o
# 所有的.c文件和所有的.o文件
*.c
*.o

```

```makefile
CC:=gcc
OBJ=circle.o
OBJ+=cube.o
OBJ+=main.o

main:$(OBJ)
	$(CC) $(OBJ) -o test
%.o:%.c
	$(CC) -c %.c -o %.o

clean:
	rm -rf $(OBJ) main
```

# Makefile[4]通配符

```makefile
# 所有的目标文件
$@
# 所有的依赖文件
$^
# 所有依赖文件的第一个文件
$<
# 所有时间戳比目标依赖文件晚的依赖文件,并以空格分开
$?
```

```makefile
CC:=gcc
OBJ=circle.o
OBJ+=cube.o main.o
RMRF=rm -rf

main:$(OBJ)
	$(CC) $^ -o $@
%.o:%.c
	$(CC) -c $^ -o $@

.PHONY:
clear:
	$(RMRF) $(OBJ)
clear-all:
	$(RMRF) $(OBJ) main
```

# Makefile[5]函数

