
## makefile 

makefile 是一个命令工具,可以自动化编译,一般命名为makefile或者Makefile.

现在咱针对c/c++语言进行对makefile的学习和认知

gcc 工作流程是编译c和c++的基础,有必要学习如何生成.o,可执行程序,以及静态库和动态库,以及相关的知识等.

### 准备环境
- ubuntu16.04 或其他linux系统
- vscode

https://blog.csdn.net/wrzfeijianshen/article/details/102657549

重点: 显示当前的tab 格式符号

“editor.insertSpaces”:false
“editor.detectIndentation”: false,
“editor.renderControlCharacters”: true,
“editor.renderWhitespace”: “all”,

远程修改文件用 remote-ssh

### gcc 工作流程
将.c编译生成可执行程序.

hello.c -->预处理阶段(gcc -E) 比如去掉注释,头文件展开,宏替换等 --> 汇编文件hello.s-->gcc -c 汇编器-->hello.o 二进制文件-->链接器 ld --> a.out

这样一步步的进行生成,这只有一个hello.c文件,通过生成.o文件,最终gcc 生成可执行程序

```
gcc -E hello.c -o hello.i
gcc -S hello.i -o hello.s
gcc -c hello.s -o hello.o
gcc hello.o -o hello
```

```
- 01/01_01 :
源文件 hello.c
#include "stdio.h"
#define VERSION "1.0"
int main()
{
	// 打印输出
	printf("hello %s ...\n",VERSION);
	return 0;
}
```

file 可执行程序名  查看当前程序是64位还是32等信息

ldd 可执行程序名,查看当前依赖哪些库名

```
root@fjs:~/code/fjs/makefile/01/01_1# file hello
hello: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=9f25f325f85140831606a9f6eab2f0bde3711e3b, not stripped
root@fjs:~/code/fjs/makefile/01/01_1# ldd hello
        linux-vdso.so.1 (0x00007ffeac3c6000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f3951941000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f3951f34000)
```

gcc 常用参数
gcc -c 只编译生成.o文件,称为目标文件
gcc -o 生成的目标文件的名字
gcc -I 指定头文件路径
gcc -L 指定库文件所在的路径
gcc -l 指定库的名字
gcc -g 调试信息 debug
gcc -M 查看当前依赖哪些头文件(系统头文件)
gcc -MM 查看当前依赖哪些本地文件,不包含系统头文件

```
hello.o依赖  hello.c 和stdio.h 等等

root@fjs:~/code/fjs/makefile/01/01_1# gcc -M hello.c
hello.o: hello.c /usr/include/stdc-predef.h /usr/include/stdio.h \
 /usr/include/x86_64-linux-gnu/bits/libc-header-start.h \
 /usr/include/features.h /usr/include/x86_64-linux-gnu/sys/cdefs.h \
 /usr/include/x86_64-linux-gnu/bits/wordsize.h \
 /usr/include/x86_64-linux-gnu/bits/long-double.h \
 /...
```

```
hello.c 仅仅依赖hello.c文件

root@fjs:~/code/fjs/makefile/01/01_1# gcc -MM hello.c
hello.o: hello.c
```

```
- 01/01_02
把宏名移动到.h头文件中,gcc hello.c 自动会生成a.out可执行程序.

root@fjs:~/code/fjs/makefile/01/01_2# gcc hello.c
root@fjs:~/code/fjs/makefile/01/01_2# ls
a.out  hello.c  hello.h
root@fjs:~/code/fjs/makefile/01/01_2# ./a.out 
hello 1.0 ...


hello.c 依赖 hello.c hello.h 这两个文件

root@fjs:~/code/fjs/makefile/01/01_2# gcc -MM hello.c
hello.o: hello.c hello.h
```

### 静态库和动态库

静态库 static library : 一些目标目标代码的集合,简单讲,就是一堆.o文件的集合,
在linux下一般是.a后缀名

静态库 一般供别人进行调用.
优点:函数库最终被打包到应用程序中,运行速度快.移植方便,不依赖其他文件
缺点:更新发布带来麻烦,耗费内存.

格式: 
- 前缀lib 
- 库名字:test
- 后缀:.a
如libtest.a

生成步骤:
- .c --> .o文件 格式: gcc -c xxx.c -o xxx.o
- 打包工具ar,把.o文件打包为.a文件, ar res(r更新,c创建,s建立索引)
    
    命令: ar rcs libxxx.a .o文件集合

静态库 使用: gcc hello.c -o hello.out  -L./ -I./ -ltest
- -L 指定链接的库目录
- -l 指定需要链接的静态库.去前缀和后缀
- -I 指定头文件所在的路径

动态库(shared library )共享库
程序在运行时才会被载入.增量更新.以.so为后缀
优点:
- 节省内存
- 升级更新方便,替换库即可
缺点:
- 加载速度比静态库慢
- 移植性差

组成: libtest.so
- 前缀 lib
- 库名称 :test
- 后缀  .so

动态库的生成:
- 生成目标文件.o ,此时需要添加编译选项 -fPIC
(-fpic 创建与地址无关的编译程序,目的是为了能够在多个应用程序之间共享)
- 生成共享库 -shared
gcc -shared *.o列表

生成可执行程序:同静态库
使用: gcc hello.c -o hello.out  -L./ -I./ -ltest
- -L 指定链接的库目录
- -l 指定需要链接的静态库.去前缀和后缀
- -I 指定头文件所在的路径

执行时找不到该库: ldd file

如何让程序找到共享库:
- 把.so文件 拷贝到/lib 或者/usr/lib

例子1:生成静态库

正常编译文件步骤

```
第1步: 把每个.c文件生成.o文件
gcc -c test1.c -o test1.o
gcc -c main.c -o main.o
第2步: 生成可执行程序hello
gcc -o hello main.o test1.o

- 01/01_03: 例子,先正常编译,test1里面有void GetTest1Version() 函数,
咱们在main中调用GetTest1Version函数的示例

root@fjs:~/code/fjs/makefile/01/01_03# ls
main.c  test1.c  test1.h
root@fjs:~/code/fjs/makefile/01/01_03# gcc -c test1.c -o test1.o
root@fjs:~/code/fjs/makefile/01/01_03# ls
main.c  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_03# gcc -c main.c -o main.o
root@fjs:~/code/fjs/makefile/01/01_03# ls
main.c  main.o  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_03# gcc -o hello main.o test1.o
root@fjs:~/code/fjs/makefile/01/01_03# ls
hello  main.c  main.o  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_03# ./hello 
当前test1 版本号[1.1]
main run end 
```

上面的例子,生成.a文件

```
咱们把test1.h和test1.c 生成的.o文件 封装成静态库.然后供main函数进行调用

第1步: 把每个.c文件生成.o文件
gcc -c test1.c -o test1.o
gcc -c main.c -o main.o

第2步: 生成静态库libtest1.a
ar rcs libtest1.a test1.o

第3步: 生成可执行程序hello
gcc main.o -o hello  -L./ -I./ -ltest1
 
01_04 :操作如下

root@fjs:~/code/fjs/makefile/01/01_04# ls
main.c  test1.c  test1.h
root@fjs:~/code/fjs/makefile/01/01_04# gcc -c test1.c -o test1.o
root@fjs:~/code/fjs/makefile/01/01_04# gcc -c main.c -o main.o
root@fjs:~/code/fjs/makefile/01/01_04# ls
main.c  main.o  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_04# ar rcs libtest1.a test1.o
root@fjs:~/code/fjs/makefile/01/01_04# ls
libtest1.a  main.c  main.o  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_04# gcc main.o -o hello  -L./ -I./ -ltest1
root@fjs:~/code/fjs/makefile/01/01_04# ls
hello  libtest1.a  main.c  main.o  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_04# ./hello 
当前test1 版本号[1.1]
main run end 

```

例子2:生成动态库

```
同样是上面的三个文件
编译流程:
gcc -fpic -c *.c
gcc -shared *.o -o libxxx.so
gcc main.c -I./ -L./ -lxxxx -o hello

第1步: 编译出库.o
gcc -fpic -c test1.c -o test1.o
第2步: 生成.so文件
gcc -shared test1.o -o libtest1.so
第3步: 生成其他.o
gcc -c main.c -o main.o
第4步:生成可执行程序
gcc main.o -I./ -L./ -ltest1 -o hello

ldd hello

01/01_05

root@fjs:~/code/fjs/makefile/01/01_05# gcc -fpic -c test1.c -o test1.o
root@fjs:~/code/fjs/makefile/01/01_05# gcc -shared test1.o -o libtest1.so
root@fjs:~/code/fjs/makefile/01/01_05# gcc -c main.c -o main.o
root@fjs:~/code/fjs/makefile/01/01_05# ls
libtest1.so  main.c  main.o  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_05# gcc main.o -I./ -L./ -ltest1 -o hello
root@fjs:~/code/fjs/makefile/01/01_05# ls
hello  libtest1.so  main.c  main.o  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/01/01_05# ldd hello 
        linux-vdso.so.1 (0x00007ffd22791000)
        libtest1.so (0x00007fefde37b000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fefddf8a000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fefde77f000)
root@fjs:~/code/fjs/makefile/01/01_05# ./hello 
当前test1 版本号[1.2]
main run end 

```



