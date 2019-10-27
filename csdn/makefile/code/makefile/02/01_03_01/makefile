
###
# test1
###
# 定义变量
var := defTESTabc
var1 := $(var:def%abc=x%y)
var2 := $(var:%bc=%y)

all :
	@echo "var : [$(var)]"
	@echo "var1 : [$(var1)]"
	@echo "var2 : [$(var2)]"

	@echo "all out"


###
# test2
###
test2 : t1 t2 t3
	@echo "test2 build: "
	@echo "\$$@ : [$@]"
	@echo "\$$^ : [$^]"
	@echo "\$$< : [$<]"

t1 t2 t3 :
		@echo "build \$$@ : [$@]"

###
# test3
###

%x: %y
	@echo "\$$@ : [$@]"
	@echo "\$$^ : [$^]"

%y : 
	@echo "[$@]"

###
# test4
###
out := a.a b.a c.a

$(out) : %.a : %.b
	@echo "\$$@ : [$@]"
	@echo "\$$^ : [$^]"

%.b : 
	@echo "[$@]"

###
# test5
###

t5 := t5555
t6 :=
override t7 := t7777
test5:
	@echo "t5 : $(t5)"
	@echo "t6 : $(t6)"
	@echo "t7 : $(t7)"


