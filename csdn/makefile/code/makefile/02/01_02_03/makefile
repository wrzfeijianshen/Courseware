all : 

test1 : 
	@echo "test1 out"
test1 : 
	@echo "test1_1 out"

test2 : 
	@echo "test2 out"

test2 : test3
	@echo "test2_2 out"

test3:
	@echo "test3 out"

test4 : test5
test4 : test6
# 等价于
# test4 : test6 test5
	@echo "test4_4 out"

test5 :
	@echo "test5 out"

test6 :
	@echo "test6 out"



