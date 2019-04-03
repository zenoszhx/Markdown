# Lua调度器、定时器


###1 全局调度器

Cocos2d-Lua框架下默认不加载全局调度器，需要手动加载：

	local scheduler = require(cc.PACKAGE_NAME..".shceduler")

全局调度器模块提供了如下三种调度器：

1. 全局高度器：schedulerUpdateGlobal(listener)
2. 全局自定义调度器:schedelerGlobal(listener, interval)
3. 全局延时调度器:performWithDelayGlobal(listener, time)

前两个调度器的生命周期需要手动管理，全局延时调度器会在回在回调后自动销毁，但也不是所有情况下都完全可靠，引擎提供了一个注销调度器的接口：scheduler.unscheduleGlobal(handle)

#####1.1 全局帧调试器

每一帧都会触发的高度器。主要用在碰撞检测等每一帧都要计算的地方。
全局帧调度器不依赖于任何场景，因此可以在整个游戏场景里实现较为精确的全局计时。例：

	local scheduler = require(cc.PACKAGE_NAME..".scheduler")

	local function onInterval(dt)
		print("update")
	end

	scheduler.schedulerUpdateGlobal(onInterval)

#####1.2 全局自定义调度器

全局帧调试器是全局自定义调度器的特例，自定义调度器可以指定调度时间。例:

	local scheduler = require(cc.PACKAGE_NAME..".scheduler")

	local function onInterval(dt)
		print("custom")
	end

	scheduler.scheduleGlobal(onInterval)

#####1.3 全局延时调度器

在游戏中的某些场合，只想实现一个单次的延时调用。scheduler.performWithDelayGlobal()会在等待指定的时间后执行一次回调函数，然后自动取消该schdeler。例:

	local scheduler = require(cc.PACKAGE_NAME..".scheduler")

	local function onInterval(dt)
		print("once")
	end

	scheduler.performWithDelayGlobal(onInterval, 0.5)

###2.节点调度

调度器是Node提供的方法之一。Node的调度器只能在Node中使用，Node负责管理调度器的生命周期，当Node销毁时会自动注销节点名下的所有调度器。

大部分情况， 我们使用节点调度器，这样我们能把精灵集中在游戏逻辑实现，而不是调度器的生命周期管理。

节点调度器提供了三种调试器:

* 节点帧调度器。（归类到节点帧事件，参考事件分发机制）
* 节点自定义调度器
* 节点延时调度器 

#####2.1节点自定义调度器

自定义时间间隔必须大于两帧的间隔时间。例:

	self:scheduler(function() print("schedule")end, 1.0)

#####2.2节点延时调度器

等待指定时间后执行一次回调函数。例：

	self:performWithDelay(function()print("performWithDelay"), 1.0)