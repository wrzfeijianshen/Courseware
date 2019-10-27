
###
# 实现功能:编译.c文件和并删除编译生成的系列文件
###
# 声明伪目标
.PHONY : all clean

all : main
	@echo "all out ok"

main : main.o test1.o
	@gcc -o main main.o test1.o
	@echo "main build ok"	

# 不依赖 test1.h ,当修改了g_hello的值,下面这条就无法执行了
# main.o : main.c 
main.o : main.c
	@gcc -o main.o -c main.c
	@echo "main.o build ok"	

test1.o : test1.c
	@gcc -o test1.o -c test1.c
	@echo "test1.o build ok"	

main.c : test1.h 
	@echo "main.c build ok"	
 
test1.c: test1.h 
	@echo "test1.c build ok"	

clean :
	@rm -rf main *.o
