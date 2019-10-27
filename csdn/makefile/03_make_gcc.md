### 使用makefile编译c程序

下面就使用makefile进行编译一个c程序.
例子 : 03/01

```
.PHONY : all clean

all : main
	@echo "all out ok"

main : 
	@gcc -o main main.c
	@echo "main build ok"	

clean :
	@rm -rf main
```

```
可以发现,这是很简短的通过命令编译方式,直接一条命令编译了
比如咱们应该在终端上gcc -o main main.c,也可以,比如放到sh脚本也可以
自己写程序也可以有办法做到这样的编译程序.
为什么要通过make,make帮咱们把这样的脚本做了一套程序,咱们可以写不同的makefile
文件,来直接输入make,便可以得到想要的编译程序了.

可能要编译的文件,嵌套目录,不同目录下也有不同的.c文件,通过渐渐学习
咱们便拥有了一套完整的自动化编译的make文件,以后,直接放入makefile
来一条make,便可以得到咱们想要的库.a .so文件.

解释下: 下面这条makefile
咱们执行make,想要得到main可执行程序,此时,main便作为目标.
也就是咱们想要输出(生成)main目标
main :
    @gcc -o main main.c

clean 目标规则是想实现清除编译过程中生成的目标文件

root@fjs:~/code/fjs/makefile/03/01# ls
main.c  makefile
root@fjs:~/code/fjs/makefile/03/01# make
main build ok
all out ok
root@fjs:~/code/fjs/makefile/03/01# make
all out ok
root@fjs:~/code/fjs/makefile/03/01# ls
main  main.c  makefile
root@fjs:~/code/fjs/makefile/03/01# make clean 
root@fjs:~/code/fjs/makefile/03/01# ls
main.c  makefile
```

例子 : 03/02
```
.PHONY : all clean

all : main
	@echo "all out ok"

main : main.o test1.o
	@gcc -o main main.o test1.o
	@echo "main build ok"	

main.o : test1.h main.c 
	@gcc -o main.o -c main.c
	@echo "main.o build ok"	

test1.o : test1.h test1.c
	@gcc -o test1.o -c test1.c
	@echo "test1.o build ok"	

clean :
	@rm -rf main *.o
```

```
解释: 咱们编写makefile编译c程序时,一般是把所有的.c文件先生成.o.
再把所有的.o文件链接成可执行程序.

那么:
- 生成main 可执行程序, 的依赖便是 mian.o test1.o 两个.o文件
main : main.o test1.o
- 各自.o文件的依赖.h或者.c. 可以通过gcc -MM查看
test1.o : test1.h test1.c
main.o : test1.h main.c 
这样写的好处是,无论你想修改.h .c文件时,相关的依赖目标便会生成,
比如 你写成main.o : main.c ,当你修改了test1.h 文件时,便无法重新生成main.o了

root@fjs:~/code/fjs/makefile/03/02# make test1.o 
test1.o build ok
root@fjs:~/code/fjs/makefile/03/02# make main.o 
main.o build ok
root@fjs:~/code/fjs/makefile/03/02# ls
main.c  main.o  makefile  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/03/02# make main
main build ok
root@fjs:~/code/fjs/makefile/03/02# ls
main  main.c  main.o  makefile  test1.c  test1.h  test1.o
root@fjs:~/code/fjs/makefile/03/02# ./main 
test1 version :[1.0.1]
hello main run 

root@fjs:~/code/fjs/makefile/03/02# gcc -MM main.c
main.o: main.c test1.h
root@fjs:~/code/fjs/makefile/03/02# gcc -MM test1.c
test1.o: test1.c test1.h

```