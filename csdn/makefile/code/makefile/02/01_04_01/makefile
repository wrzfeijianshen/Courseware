define func1 
    @echo "func1 : [$(0)]"
endef

define func2 
  @echo "func1 : [$(0)]"
  @echo "Param1 : [$(1)]"
  @echo "Param2 : [$(2)]"
endef

# 函数调用
test :
	$(call func1)  
	$(call func2,aaa)

#out
# func1 : [func1]
# func1 : [func2]
# Param1 : [aaa]
# Param2 : []