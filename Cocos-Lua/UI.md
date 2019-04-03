# UI

align和valign参数可用的值：

* cc.TEXT_ALIGNMENT_LEFT 左对齐
* cc.TEXT_ALIGNMENT_CENTER 水平剧中对齐
* cc.TEXT_ALIGNMENT_RIGHT 左对齐
* cc.VERTICAL_TEXT_ALIGNMENT_TOP 垂直顶部对齐
* cc.VERTICAL_TEXT_ALIGNMENT_CENTER 垂直居中对齐
* cc.VERTICAL_TEXT_ALIGNMENT_BOTTOM 垂直底部对齐

### 1 文本标签

##### 1.1 TTF

字体库规范

创建方式：

	display.newTTFLabel(parames)

属性：

* text：显示文本
* font：字体名
* size：字体大小
* color：文字颜色
* align：水平对齐方式
* valign：垂直对齐方式
* dimensions：文字显示对象的尺寸（可选）。用cc.size创建
* x
* y

例：

	-- 左对齐，并且多行文字顶部对齐
	local label = display.newTTFLabel({
		text = "Hello World\n 您好世界",
		font = "Markr Felt",
		size = 64,
		color = cc.c3b(255, 0, 0),
		align = cc.TEXT_ALGNMENT_CENTER,
		valign = cc.VERTICAL_TEXT_ALIGNMENT_TOP,
		dimensions = cc.size(400, 200)
	})

	

##### 1.2 BMFont

适用于频繁更新的场合，需要使用图片来创建字体。

使用前需要添加.png和一个字体坐标文件.fnt。

通常用来显示英文、数字等数量有限的文本。

创建方式：

	display.newBMFontLabel(params)

参数：

* text
* font：字体的文件名
* align：
* maxLineWidth:最大行宽
* offsetX：图像的X的偏移量
* offsetY：图像Y的偏移量
* x
* y

例:

	local labelbmf = display.newBMFontLabel({
		text = "Hello",
		font = "helvetica-32.fnt"
	})

##### 1.3 UILabel

创建方式：

	cc.ui.UILabel.new({UILabelType = 2, text = "Hello, World", size = 64})
		:algn(display.CENTER, display.cx, display.cy)
		:addTo(self)

参数：和TTFLabel与BMFontLabel相同。

### 2 按钮

三个表现按钮仅是表现形式不一样，但内在的事件行为都继承自UIButton，可监听的事件如下：

* CLICKED，单击。UIButton:onButtonClicked(callback)监听
* PRESSED，按下。UIButton:onButtonPressed(callback)监听
* PELEASE，释放。UIButton:onButtonRelease(callback)监听
* STATE_CHANGED，状态改变。UIButton:onButtonStateChange(callback)监听

##### 2.1 UIPushButton

创建方式：

	cc.ui.UIPushButton.new(images, options)
	-- images：table类型。各个状态的图片。
	-- options：table类型。可选参数，scale9, flipX,flipY

例：

	function MainScene:ctor()
	
		local images = {
			normal = "button/Buttono1.png"
			pressed = "button/Buttono2.png"
			disabled = "button/Buttono3.png"
		}	

		cc.ui.UIPushButton.new(images, {scale9 = true})
			:setButtonSize(240, 60),
			:setButtonLabel("normal", cc.ui.UILabel.new({
				UILabelType = 2,
				text = "This is a PushButton",
				size = 18
			}))
			:setButtonLabel("pressed", cc.ui.UILabel.new({
				UILabelType = 2,
				text = "This is a PushButton",
				size = 18
			}))
			:setButtonLabel("disabled", cc.ui.UILabel.new({
				UILabelType = 2,
				text = "This is a PushButton",
				size = 18
			}))
			:onButtonClicked(function(event) 
				print("pushButton click")
			end)
			:align(display.LEFT_CENTER, display.left + 80, display.top - 80)
			:addTo(self)
	
	end

##### 2.2 UICheckBoxButton

UICheckBoxButton相当于一个开关，单击按钮在on与off之间切换。创建方式：

	cc.ui.UICheckBoxButton.new(images, options)
	-- 参数如同UIPushButton

例：

	function MainScene:ctor()

		local images = {
			off = "button/button1.png",
			off_pressed = "button/button2.png",
			off_disabled = "button/button3.png",
			on = "button/button4.png",
			on_pressed = "button/button5.png",
			on_disabled = "button/button6.png"
		}

		local function updateCheckBoxButtonLabel(checkbox)
			local state = ""
			if checkbox:isButtonSelected() then
				state = "on"
			else
				state = "off"
			end
			if not checkbox:isButtonEnabled() then
				state = state.."(disabled)"
			end
			checkbox:setButtonLabelString(string.format("state is %s", state))
		end

		local checkBoxButton = cc.ui.UICheckBoxButton.new(image)
			:setButtonLabel(cc.ui.UILabel.new({text = "", size = 22, color = cc.c3b(255, 96, 255)}))
			:setButtonLabelOffset(0, -40)
			:setButtonLabelAlignment(display.CENTER)
			:onButtonStateChange(function(event)
				updateCheckBoxButtonLabel(event.target) 
			end)
			:align(display.LEFT_CENTER, display.left + 40, display.top - 180) --设置锚点和位置
			：addTo(self)

		updateCheckBoxButtonLabel(checkBoxButton1)

	end

##### 2.3 UICheckBoxButtonGroup

UICheckBoxButtonGroup用来创建一组选项，一个时刻只有一项能被选中。创建方式：

	cc.ui.UICheckBoxButtonGroup.new(direction)
	--direction,integer类型，可选值：
	--display.LEFT_TO_RIGHT
	--display.RIGHT_TO_LEFT
	--display.TOP_TO_BOTTOM
	--display.BOTTOM_TO_TOP

	function MainScene:ctor()

		local images = {
			off = "button/button1.png",
			off_pressed = "button/button1.png",
			off_disabled = "button/button1.png",
			on = "button/button1.png",
			on_pressed = "button/button1.png",
			on_disabled = "button/button1.png",
		}

		local group = cc.ui.UICheckBoxButtonGroup.new(display.TOP_TO_BOTTOM)
			:addButton(cc.ui.UICheckBoxButton.new(images)
				:setButtonLabel(cc.ui.UILabel.new({text = "option 1", color = display.COLOR_WHITE}))
				:setButtonLabelOffset(20, 0)
				:align(display.LEFT_CENTER))
			:addButton(cc.ui.UICheckBoxButton.new(images)
				:setButtonLabel(cc.ui.UILabel.new({text = "option 2", color = display.COLOR_WHITE}))
				:setButtonLabelOffset(20, 0)
				:align(display.LEFT_CENTER))
			:addButton(cc.ui.UICheckBoxButton.new(images)
				:setButtonLabel(cc.ui.UILabel.new({text = "option 3", color = display.COLOR_WHITE}))
				:setButtonLabelOffset(20, 0)
				:align(display.LEFT_CENTER))
			:addButton(cc.ui.UICheckBoxButton.new(images)
				:setButtonLabel(cc.ui.UILabel.new({text = "option 4", color = display.COLOR_WHITE}))
				:setButtonLabelOffset(20, 0)
				:align(display.LEFT_CENTER))
			:setButtonLayoutMargin(10, 10, 10, 10)
			:onButtonSelectChanged(function(event)
				printf("-----------") 
			end)
			:align(display.LEFT_CENTER, diaplay.left + 40, display.top - 440)
			:addTo(self)
	end

### 3 输入控件

#####3.1 创建
	
	cc.ui.UIInput.new(options)
	--options可选参数如下：
	--image：输入框的图像，可以是图像名或是display.newScale9Sprite()创建的Sprite9Scale对象。
	--imagePressed：输入状态时显示的图像
	--imageDisabled：禁止输入状态显示的图像
	--listener：回调函数，监听输入事件
	--size：输入框的尺寸
	--x,y

#####3.2 获取文本

	local text = editbox:getText()

#####3.3 设置提示信息

	editbox:setPlaceHolder("请输入密码")

#####3.4 设置显示文本

	editbox：setText(“")

#####3.5 监听输入事件

#####3.6 密码输入

	editbox:setInputFlag(0)

例：

	local funciton onEdit(event, editbox)

		if event == "begin" then
			-- 开始输入，Player模拟器上没有这个事件
		elseif event == "changed" then
			-- 内容发生改变
		elseif event == "ended" then
			-- 输入结束
		elseif event == "return" then
			-- 从输入框返回，响应手机上的return按钮
		end

		local editbox = cc.ui.UIInput.new(
			image = "editbox.png",
			listener = onEdit,
			x = display.width/2,
			y = display.height/2,
			size = cc.size(200, 40)
		)

		self:addChild(editbox)
		editbox:setPlaceHolder("")

	end

### 4 进度条

#####4.1 创建

	cc.ui.UILoadingBar.new(option)
	-- option可选参数如下
	-- scale9,boolean类型，表示是否缩放
	-- capInsets，cc.rect类型，设置图片的绽放区域，避免缩放导致图片模糊失真。
	-- image，string类型，表示进度条的图片
	-- viewRect，cc.rect类型，表示显示区域
	-- percent，int类型，表示初始化进度
	-- direction，方向，默认从左到右。可选有：UILoadingBar.DIRECTION_LEFT_TO_RIGHT,UILoadingBar.DIRECTION_RIGHT_TO_LEFT

#####4.2 更新

	loadBar:setPercent(percent)

例：
	
	local loadBar = cc.ui.UILoadingBar.new({
		scale9 = true,
		capInsets = cc.rect(0, 0, 10, 10),
		image = "loading.png",
		viewRect = cc.rect(0, 0, 200, 32),
		percent = 30,
		direction = DIRCTION_RIGHT_TO_LEFT
	}) 
	:addTo(self)

### 5 滑动条

#####5.1 创建

	cc.ui.UISlider.new(direction, images, options)

	1.direction，number类型，表示滑动方向
		display.LETF_TO_RIGHT
		diaplay.TOP_TO_BOTTOM

	2.images，table类型，可用参数如下：
		bar string类型，滑动条图片路径
		button string类型，滑块图片路径
 
	3.options，table类型，可用参数如下：
		scale9 boolean，图片是否可缩放
		min number，最少的值是0
		max number，最大默认是100
		touchInButton boolean，是否只在触摸在滑动块上时才有效，默认为true。

#####5.2 修改滑动条大小

	UISlider:setSliderSize(barWidth, batHeight)

#####5.3 滑动块位置

	UISlider:setSliderValue(value)
	
	UISlider:getSliderValue()

#####5.4 事件监听

* PRESSED_EVENT，按下滑动块事件，监听方法如下：

	UISlider:onSliderPressed(function(event) end)
	
	event.x，event.y触摸点y坐标

* PELEASE_EVENT，释放滑动块事件，监听方法如下：

	UISlider:onSliderRelease(function(event) end)

	event.x, event.y，触摸点坐标

* STATE_CHANGED_EVENT，状态改变事件，监听方法如下：

	UISlider:onSliderStateChange(function(event) end)

	调用UISlider:setSliderEnabled(enabled)的时候触发这个消息

* VALUE_CHANGED_EVENT，值改变事件

	UISlider:onSliderValueChange(function(event) end)

	event.value中存放改变后的值

例：
	
	local images = {
		bar = "slider/SliderBar.png"
		button = "slider/SliderButton.png"
	}	

	local barHeight = 40
	local barWidth = 400
	local balueLabel = cc.ui.UILabel.new({text = "", size = 14, color = display.COLOR_BLACK})
		:align(display.LEFT_CENTER, display.left + barWidth + 60, dispaly.top - 60)
		:addTo(self)

	cc.ui.UISlider.new(display.LEFT_TO_RIGHT, images, {scale9 = true})
		:onSliderValueChanged(function(event) 
			valueLabel:setString(string.format("value = %0.2f", event.value))
			print(event.name)
		end)
		:onSliderStateChange(function(event)
			print(event.name)
		end)
		:onSliderPressed(function(event)
			print(event.name)
		end)
		:onSliderRelease(function(event)
			print(event.name)
		end)
		:setSliderSize(barWidth, batHeight)
		:setSliderValue(75)
		:align(display.LEFT_CENTER, display.left + 40, display.top - 80)
		:addTo(self)
		
### 6 滚动视图

#####6.1 创建

	cc.ui.UIScrollView.new(params)
	--可选参数如下：
	1.direction，可选：
		UIScrollView.DIRECTION_BOTH
		UIScrollView.DIRECTION_VERTICAL
		UIScrollView.DIRECTION_HORIZONTAL
	2.viewRect：表示列表控件的显示区域
	3.scrollbarImgH:string类型，表示水平方向的滚动条图片资源路径。
	4.scrollbarImgV:string类型，表示垂直方向的滚动条图片资源路径。
	5.bgColor：c4b类型，表示背景色，nil表示无背景色。
	6.bgStarColor：c4b类型，表示渐变背景开始色，nil表示无背景色。
	7.bgEndColor：c4b类型，表示渐变背景结束色，nil表示无背景色。
	8.bg，string类型，表示背景图片资源路径。
	9.bgScale9，boolean类型，表示背景图片是否可缩放。
	10.capInsets，rect类型，表示缩放区域。

#####6.2 回弹效果

	UIScrollView:setBounceable(false)
	-- 禁用回弹效果

#####6.3 事件监听

	UIScrollView:onScroll(function(event)
		print("ScrollListener:"..event.name)
	end)
	1.began阶段
	2.moved阶段
	3.ended阶段

例：

	local sp = display.newSprite("scroll/cloud.png")
	sp:pos(display.cx, display.cy)

	cc.ui.UIScrollView.new({
		direction = cc.ui.UIScrollView.DIRECTION_BOTH,
		viewRect = {x = 0, y = 0, width = 320, height = 480},
		scrollbarImgH = "scroll/barH.png",
		scrollbarImaV = "scroll/bar.png",
		bgColor = cc.c4b(255, 255, 255, 255)
	})
	:addScrollNode(sp)
	:setDirection(cc.ui.UIScrollView.DIRECTION_BOTH)
	:onScroll(function(event)
		print("TestUIScrollViewSene - scrollListener:"..event.name)
	end)
	:addTo(self)
	:pos(display.cx - 160, display.cy - 240)
	:setBounceable(false)


### 7 列表

#####7.1 创建

	cc.ui.UIListView.new(params)
	可用参数如下：
	1.direction，默认为垂直
		UIScrollView.DIRECTION_VERTICAL 垂直
		UIScrollView.DIRECTION_HORIZONTAL 水平
	2.alignment content对齐方式，默认为垂直居中
		UIListView.ALIGNMENT_LEFT
		UIListView.ALIGNMENT_RIGHT
		UIListView.ALIGNMENT_VCENTER
		UIListView.ALIGNMENT_TOP
		UIListView.ALIGNMENT_BOTTOM
		UIListView.ALIGNMENT_HCENTER
	3.viewRect，表示列表控件的显示区域
	4.scrollbarImgH，string类型，表示水平方向的滚动条图片资源路径。
	5.scrollbarImgV，string类型，表示水平方向的滚动条图片资源路径。
	6.bgColor，c4b类型，背景色。
	7.bgStartColor
	8.bgEndColor
	9.bg，string，表示背景图片资源
	10. bgScale9，表示背景图是否可缩放
	11. capInsets，rect类型，表示缩放区域
		
#####7.2 创建列表项

可以使用UIlistViewItem的new方法来创建一个表项，但这并不是推荐的方法。UIListView内部封装一个创建表项的成员函数，它针对当前表的属性在item上做了额外的初始化。

	local list = cc.ui.UIListView:new(params)
	local item = list:newItem(contentNode)
	--contentNode：可显示的节点类型
	--可不传contentNode，通过item:addContent(contentNode)来设置显示节点

UIListViewItem是为UIListView服务的，除了contentNode，它还有重要的功能即约束当前列表的宽高。它提供如下接口：
	
* UIListViewItem:setItemSize(w, h, bNoMargin)设置表项的宽高。
* UIListViewItem:setMargin(margin)设置表项之间的间隙。margin的默认为：{left = 0, right = 0, top = 0, bottom = 0}

例：

	在MainScene的ctor方法中加入如下代码：
	
	local function touchListener(event)
		local listView = event.listView
		if "clicked" == event.listView then
			if 3 == event.itemPos then
				listView:removeItem(event.item, true)
			end
		end
	end

	self.lv = cc.ui.UIListView.new{
		bgColor = cc.c4b(200, 200, 200, 200),
		-- bg = "list/sunset.png",
		bgScale9 = true,
		viewRect = cc.rect(40, 80, 120, 400),
		direction = cc.ui.UIScrollView.DIRECTION_VERTICAL,
		scrollbarImgV = "list/bar.png"	--滚动条图片
	}
		:onTouch(touchListener)
		:addTo(self)

	for i = 1, 15 do

		local item = self.lv:newItem()
		local const

		if 2 == i then

			content = cc.ui.UIPushButton.new("list/GreenButton.png", {scale9 = true})
				:setButtonSize(120, 40)
				:setButtonLabel(cc.ui.UILabel.new({text = "单击大小改变"}..i, size = 16, color = display.COLOR_BLUE}))
				:onButtonPressed(function(event)
					event.target:getButtonLabel():setColor(display.COLOR_RED)
				end)			
				:onButtonReleased(function(event) 
					event.target:getButtonLabel():setColor(display.COLOR_BLUE)
				end)
				:onButtonClicked(function(event)

					if not self.lv:isItemInViewRect(item) then
						print("TestUIListViewScene item not in view rect")
						return
					end 
					print("TestUIListViewScene buttonClick")
					local _, h = item:getItemSize()

					if 40 = h then
						item:setItemSize(120, 80)		
					else
						item:setItemSize(120, 40)
					end					
				end)

		elseif 3 == i then

			content = cc.ui.UILabel.new({
				text = "单击删除它"..i，
				size = 20,
				align = cc.ui.TEXT_ALIGN_GENTER,
				color = display.COLOR_BLACK
			})

			item:setBg("list/YellowBlock.png")

		elseif 4 == i then

			content = cc.ui.UILabel.new({
				text = "有背景图"..i,
				size = 20,
				align = cc.ui.TEXT_ALIGN_CENTER,
				color = display.COLOR_BLACK
			})

			item:setBg("list/YellowBlock.png")

		else

			content = cc.ui.UILabel.new({
				text = "item"..i,
				size = 20,
				align = cc.ui.TEXT_ALIGN_CENTER,
				color = display.COLOR_BLACK
			})

		end

		item:addContent(content)
		iten:setItemSize(120, 40)

		self.lv.addItem(item)

	end

	self.lv:reload()

上例展示了4种不同item用法，应用了UIListView的大部分功能。

（1） 列表1的contentNode是一个文本标签，显示当前item的编号。

（2） 列表2的contentNode是一个按钮

* self.lv:isItemInViewRect(item)列表视图可滚动查看这个接口判断item是否在可视区域内。
* item:getItemSize()有两个返回值，item的高和宽

（3） 列表3和列表1的区别仅仅在于文字，单击可以删除。实现该功能需要监听ListView的事件。
	
	self.lv:onTouch(function(event) end)
	enent.name是事件类型，当它为clicked时，event还据有下面域：
	1.item被单击的列表项实体
	2.itemPos被单击在表中的位置
	3.listView 表实例
	4.point包含触摸点x,y的table

	listView:removeItem(event.item, true)
	第一个参数是被移除的项，第二个参数是是否使用移除动画。

（4） 列表4使用新的接口item:setBg(bgName)设置背景图片。


### 8 分页

#####8.1 视图

	cc.ui.UIPageView(params)
	可选参数如下：
	1.column，number类型，表示每一页的列数。
	2.row，表示每一页的行数
	3.columnSpace，表示列之间的间隙
	4.rowSpace，表示行之间的间隙
	5.viewRect，表示页面控件的显示区域
	6.padding，table，表示页面四周的间隙 
		left
		rignt
		top
		bottom
	7.bCirc，表示页面是否循环

#####8.2 创建项

同ListView一样，推荐使用UIPageView的成员函数来创建分页项。

	local page = cc.ui.UIPageViewItem.new();
	local item = page.newItem

#####8.3 监听
	
	UIPageView:onTouch(function(event) 
		dump(event, "TestUIPageViewScene--evnet")
	end)

例：

	local funciton touchListener(event)
		dump(event, "TestUIPageViewScene - event")
	end

	self.pv = cc.ui.UIPageView.new({
		viewRect = cc.rect(80, 240, 480, 480),
		column = 3,
		row = 3,
		padding = {left = 20, right = 20, top = 20, bottom = 20},
		columnSpace = 10,
		rowSpace = 10,
		bCirc = true
	})
		:onTouch(touchListener)
		:addTo(self)

	for i = 1, 18 do

		local item = self.pv.newItem()
		local content = cc.LayerColor:create(
			cc.c4b(
				math.random(250),
				math.random(250),
				math.random(250),
				250
			)
		)		
		
		content:setContentSize = (240, 140)
		content:setTouchEnabled(false)
		item:addChild(content)
		self.pv:addItem(item)
	end

	self.pv:reload()