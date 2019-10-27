
target := test1 test2 test3 test4 test5 \
          test6 test7 test8 test9

all : $(target)
	@echo "all out"

# var_subst : [hxxxlxxxlxxxloxxx]
var_subst := $(subst aaa,xxx,haaalaaalaaaloaaa)
test1:
		@echo "var_subst : [$(var_subst)]"

#var_patsubst : [yyylloxxx]
var := bbblloaaa
var_patsubst := $(patsubst bbb%aaa,yyy%xxx,$(var))
var_patsubst2 := $(var:bbb%aaa=yyy%xxx)

test2:
	@echo "var_patsubst : [$(var_patsubst)]"
	@echo "var_patsubst2 : [$(var_patsubst2)]"


var_strip := a       c ccdd	 	dd cc
var_strip2 := 	 a    c 	ccdd	dd    	cc	 

var_strip3 := $(strip $(var_strip))
var_strip4 := $(strip $(var_strip2))

test3:
	@echo "var_strip : [$(var_strip)]"
	@echo "var_strip2 : [$(var_strip2)]"
	@echo "var_strip3 : [$(var_strip3)]"
	@echo "var_strip4 : [$(var_strip4)]"

# var_subst : [hxxxlxxxlxxxloxxx]
# var_patsubst : [yyylloxxx]
# var_patsubst2 : [yyylloxxx]
# var_strip : [a       c ccdd             dd cc]
# var_strip2 : [a    c    ccdd    dd      cc       ]
# var_strip3 : [a c ccdd dd cc]
# var_strip4 : [a c ccdd dd cc]

var_findstring := asdfgghjkl

var_findstring2 := $(findstring gg,$(var_findstring))
test4:
	@echo "var_findstring : [$(var_findstring)]"
	@echo "var_findstring2 : [$(var_findstring2)]"

# var_findstring : [asdfgghjkl]
# var_findstring2 : [gg]

var_filter := a.c b.o c.d d.o f.c e.h
# 这里只是匹配a.c.直接写a.c不就好了,除非找不到a.c
# 没有多大实在意义
var_filter1 := $(filter a.c,$(var_filter))
var_filter2 := $(filter %.c %.o,$(var_filter))

test5:
	@echo "var_filter1 : [$(var_filter1)]"
	@echo "var_filter2 : [$(var_filter2)]"

# var_filter1 : [a.c]
# var_filter2 : [a.c b.o d.o f.c]

var_filter_out := a.c d.d f.e o.o p.p c.s
var_filter_out1 := a.c o.o
var_filter_out2 := $(filter-out $(var_filter_out1),$(var_filter_out))

test5:
	@echo "var_filter_out : [$(var_filter_out)]"
	@echo "var_filter_out2 : [$(var_filter_out2)]"

# var_filter_out : [a.c d.d f.e o.o p.p c.s]
# var_filter_out2 : [d.d f.e p.p c.s]

var_sort := zzz ooo ccc ooo foo
var_sort1 := $(sort $(var_sort))
test6:
	@echo "var_sort1 : [$(var_sort1)]"

# var_sort1 : [ccc foo ooo zzz]

var_suffix := fggga.c eeec.c aae.o

var_suffix1 := $(suffix $(var_suffix))
var_basename := $(basename $(var_suffix))
test7:
	@echo "var_suffix1 : [$(var_suffix1)]"
	@echo "var_basename : [$(var_basename)]"

# var_suffix1 : [.c .c .o]
# var_basename : [fggga eeec aae]

var_addsuffix := aaa bbb ccc ddd
var_addsuffix1 := $(addsuffix .h,$(var_addsuffix))
var_addprefix :=  $(addprefix g_,$(var_addsuffix))
test8:
	@echo "var_addsuffix1 : [$(var_addsuffix1)]"
	@echo "var_addprefix : [$(var_addprefix)]"

# var_addsuffix1 : [aaa.h bbb.h ccc.h ddd.h]
# var_addprefix : [g_aaa g_bbb g_ccc g_ddd]

var_files := $(wildcard *.c)

define funcTest8 
	@touch a.c
	@touch b.c
	@echo "var_files : [$(var_files)] [$(wildcard *.c)]"
endef

test8:
	# rm -rf a.c b.c
	$(call funcTest8)
	@echo "var_files : [$(var_files)] [$(wildcard *.c)]"
# var_files : [a.c b.c]

test9:
	@ $(warning warning exit)
	@ $(error error exit)
	@echo "test9 exit"

