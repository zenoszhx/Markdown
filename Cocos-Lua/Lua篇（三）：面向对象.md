#面向对象

### 1.封装

了解metatable之后，Lua类的封装就变的容易。只要为table添加metatable，并设置 _ _index元方法。如下例中People的new方法。

	Prople = {age = 18}
	function People.new()
		local p = {}
		setmetatable(p, self)
		self.__index = self
		return p
	end

	function People:growUp()
		self.age = self.age + 1
		print(self.age)
	end

	--测试
	p1 = People.new()
	p1:growUp() --> 19
	p2 = People.new()
	p2:growUp() --> 19

从运行结果可以看出，两个对象拥有独立的age成员，而且所有有关People的方法都可以对外不可见，这样完全实现了面向对象中类的封装。

### 2.继承

依然使用上面People，展示Lua实现继承的方法。创建一个Pwo实例Man,再在Main上重写PeoPle同名方法。

	Man = People:new()
	function Man:growUp()
		self.age = self.age + 1
		print("man`s growUp"..self.age)
	end

	--测试
	man1 = Man.new()
	man1.growUp()    -->man`s growUp:19

结果说明Man成功重写了growUp方法，并能使用People定义的age成员。实现了继承的基本方法。

### 3.多态

Lua不支持函数多态，而指针的多态由于Lua动态类型的特性，本身就能支持。

	person = People.new()
	person:growUp()
	person = Man.new()
	person:growUp()

	----> People`s growUp:19
	----> Man`s growUp:19