# 函数

### 1、函数定义

	function func()
	end
	--以function开头、end结尾、必须加()

面向对象编程中经常会定义内部方法，在Lua中实现如下：

	class = {}	--一个对象

	function class.func1()
	end

	function class:func2()
	end

	-- 首先定义了一个名为class的table，然后在table上定义了两个函数成员。
	func1()和func2()使用了不同的定义方法，：自动传入一个名为self的变量，self同C++的this一样。

### 2、函数参数与返回值


### 3、可变参数

	function add(...)
		local s = 0
		for i, v in ipairs{...} do
			s = s + v
		end
		return s
	end

	print(add 1, 2, 3, 4, 5)-->15

### 4、闭包函数

闭包函数是指将一个函数写在另一个函数内部，这个内部函数可以访问外部函数中的局部变量。而内部函数一直在使用外部函数的局部变量，从而Lua一直维护外部函数局部变量的生命周期。
这个外部函数的局部变量，既不是局部变量，也不是全局变量，称之为外部局部变量或upvalue

	function newCounter()
		local i = 0
		return function()
			i = i + 1
			return i
		end
	end

	c1 = newCounter()
	
	print(c1())-->1
	print(c1())-->2
