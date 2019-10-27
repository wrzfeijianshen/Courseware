
debug := true
var := 100
ifeq ($(debug),true)
	var := 1000
else
	var := 0
endif

test1 :
ifeq ($(debug),true)
	@echo "debug : [$(debug)] == true"
	@echo "var : = [$(var)]"
else 
	@echo "debug : [$(debug)] != true"
	@echo "var : = [$(var)]"
endif

test2 :
    ifeq ($(debug),true1)
		@echo "debug : [$(debug)] == true"
    else 
		@echo "debug : [$(debug)] != true"
    endif
