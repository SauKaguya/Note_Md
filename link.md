**预处理**

* 预处理命令

```bash
gcc -E hello.c -o hello.i
cpp
```

* 处理源文件中以“#”开头的预编译指令，包括：
  
  * 删除“#define”并展开所定义的宏
  
  * 处理所有预编译指令，如#ifdef，#endif等
  
  * 插入头文件到“#include”处，可以递归方式进行处理
  
  * 删除所有注释“//”和‘’/* */‘’
  
  * 添加行号和文件名标识，以便编译时编译器产生调试用的行号信息
  
  * 保留所有#pragma编译指令（编译器需要用）

* 经过预处理后，得到的预处理文件（如，hello.i），它还是一个可读的文本文件，但不包含任何宏定义

**编译**

* 编译过程就是将预处理后得到的预处理文件进行词法分析、语法分析、语义分析并优化，生成汇编代码文件

* 用来进行编译处理的程序称为编译程序（编译器，Compiler）

* 编译命令
  
  ```sql
  gcc -S hello.i -o hello.s
  gcc -S hello.c -o hello.s
  cc1
  ```

* 经过编译后，得到的汇编代码文件还是可读的文本文件，CPU无法理解它

> gcc命令实际上是具体程序（如cpp，cc1，as等）的包装命令，用户通过gcc命令来使用具体的预处理程序cpp、编译程序cc1和汇编程序as等

**汇编**

* 汇编代码文件（由汇编指令构成）成为汇编语言源程序

* 汇编程序（汇编器）用来将汇编语言源程序转换为机器指令序列（机器语言程序）

* 汇编指令和机器指令一一对应，前者是后者的符号表示，它们都属于机器级指令，所构成的程序成为机器级代码

* 汇编命令
  
  ```bash
  gcc -c hello.s -o hello.o
  gcc -c hello.c -o hello.o
  as hello.s -o hello.o
  ```

* 汇编结果是一个可重定位目标文件，其中包含的是不可读的二进制代码，必须用相应的工具软件来查看其内容

**链接**

* 预处理、编译和汇编三个阶段针对一个模块（一个*.c文件）进行处理，得到对应的一个可重定位目标文件（一个*.o文件）

* 链接过程将多个可重定位目标文件合并以生成可执行目标文件

* 链接命令
  
  ```sql
  gcc -static -o myproc main.o test.o
  ld -static -o myproc main.o test.o
  ```

**可执行文件的生成**

>  使用GCC编译器编译并链接生成可执行程序P:
> 
> gcc -O2 -g -o p main.c swap.c
> 
> -O2：二级优化
> 
> -g：生成调试信息

**可执行文件存储映像**

<img title="" src="file:///home/kaguya/Pictures/截图/截图%202023-09-26%2023-39-27.png" alt="" data-align="inline" width="287"><img title="" src="file:///home/kaguya/.config/marktext/images/2023-09-26-23-43-36-image.png" alt="" width="349">


