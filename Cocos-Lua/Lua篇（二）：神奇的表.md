# 神奇的表

### 1、table构造式

（1）构造一个table的时候，如果不给key，则key默认从1开始。如要声明一个有关星期的table：
	
	t = {"Sunday", "Monday", "Tuesday"...}
	--这样一个table构造应该如下
	t = {}
	t[1] = "Sunday"
	t[2] == "Monday"

（2）使用混合的方式给table赋值，未设置key的项，key会自动从1开始编号。

（3）删除一个table时直接给他赋值为nil就可以了。


### 2.元表

##### 2.1 元表概念

metatable 是 table预定的一系列操作。比如把两个table相加，那么Lua就会先去检查table是否有metatable，然后检查metatable中是否有add方法。如果有就按照add中的方法来执行，如果没有则报错。

在Lua中的每一个值都有或者可以有一个元表。table和userdata可以拥有独立的元表。其他类型的值只能共享所属类型的元表，比如字符串就是使用的string的元表。

Lua丰创建table的时候是不会创建metatable的，需要使用setmetatable来设置元表。setmetatable的参数可以量任意table，包括要赋值的table本身。

例：实现一个table，能让它实现“+”运算

	Set = {}
	
	function Set.new(t)
		local set = {}
		for i, v in ipairs(t) do
			set[v] = true
		end
	end

	function Set.union(a. b)
		local res = Set.new()
		for k in ipairs(a) do
			res[k] = true
		end
        for k in ipairs(b) do
			res[k] = true
		end
		return res
	end

	function Set.tostring(st)
		locas s = "{"
		local sep = ""
		for e in pairs(set) do
			s = s .. sep .. e,
			sep = ","
		end
		return s.."}"
	end

	function Set.print(2)
		print(Set.tostring(s))
	end

	--定义metatable
	Set.mt = {}
	Set.mt.__add = Set.union

	s1 = Set.new{10, 20, 30, 50}
	s2 = Set.new{30, Set.mt}
	s3 = s1 + s2
	Set.print(s3)

##### 2.2 重要元方法

table的其他预定义元方法如图：

![](http://7xqzxs.com1.z0.glb.clouddn.com/Metamethods.png)

（1）_  _ index元方法
Lua中访问table的元素量先通过__index元方法来查找是否有这个函数，如果没有则返回nil。
通过改变 _  _index元方法，能够改变检索之后的结果。 _  _index的值可以直接是一个table，也可以是一个函数。若是table，则会以该table作为索引进行查询。若是函数，Lua则将table和缺少的域作为参数调用这个函数。


	--改变 _ _ index元方法，用函数来检索
	Window = {}
	Window.mt = {}
	Window.prototype = {x = 0, y = 0, width = 100, height = 100}
	Window.mt.__index = function(table, key)
		return Window.prototype[key]
	end
	
	function Window.new(t)
		setmetatable(t, Window.mt)
		return t
	end	

	-- 测试
	w = Window.new{x = 10, y = 20}
	print(w.height)
	--输出100

（2）__newindex元方法
 
_  _ newindex和 _ _ index的使用基本一样。区别在于 _ _ newindex的作用量用于table更新， _ _ index量用于table的查询操作。当table中不存在的索引赋值是，就会调用 _ _ newindex元方法。组合使用 _ _ index和 _ _ newIndex可以实现很多功能。

	Window = {}
	Window.mt = {}
	Window.prototype = {x = 0, y = 0, width = 100, height = 100}
	Window.mt.__newindex = function(table, key, value)
		print("update of element"..tostring(key)..tostring(value))
		rewset(table, key, value)
	end
	
	function Window.new(t)
		setmetatable(t, Window.mt)
		return t
	end	

	-- 测试
	w = Window.new{x = 10, y = 20}
	w.a = 10
	print(w.a)
	--输出10

（3） _ _ index在get表中未定义元素时触发，对应有rawget(table, key)来避免调用 _ _ index;  _ _ newindex在set表中未定义元素时触发，对应有rawset(table, key)来避免调用 _ _ newindex方法。