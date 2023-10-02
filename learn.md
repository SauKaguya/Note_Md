```bash
file 辨识文件类型

objdump 反汇编

wc(wordcount) -l显示行数

gcc hello.c -static --verbose gcc的日志

include <>和 include ""，前者直接到标准函数库中找文件，
后者会先在当前目录找，如果找不到，
则在之前已经使用include指令打开过的文件所在的文件夹内搜索，
如果已经有多个被include的文件，则按照它们被打开的相反顺序去搜索；
如果还是找不到，则在编译器设置的include路径内搜索；
还是找不到，在系统的INCLUDE环境变量内搜索。
```

**从c语言到二进制程序**

```sql
预处理 = 文本粘贴
预处理指令：gcc -E a.c
```

**编译的正确性**

```bash
.c执行中所有外部观测者可见的行为，必须在.s中保持一致
```

**比较两个文件是否完全相同**

```bash
文本文件：vimdiff file1 file2
非文本文件：diff file1 file2
很大的文件：md5sum file1 file2 (比较hash code)
```

**UNIX哲学**

```bash
1.每个小工具只做一件事，但做到极致
2.小工具采用文本进行输入输出，从而易于使用
3.通过小工具之间的组合来解决复杂问题
```
