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