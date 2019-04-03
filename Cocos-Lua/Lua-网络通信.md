#网络通信

Cocos2d-Lua一共提供了以下三个通信模块

* network，http协议的客户端解决方案
* SocketTCP，基于LuaSocket封装的tcp客户端解决方案
* WebSocket，WebSocket协议客户端解决方案

### 1 HTTP请求

Cocos2d-Lua的network模块封装了与网络相关的高层接口，可以方便地检测当前网络连接状态、网络类型、进行http网络请求。

##### 1.1 网络状态

1. network.isLocalWiFiAvailable()：本地WIFI网络是否可用，返回boolean类型。
2. network.isInternetConnectionAvailable()：用于检查互联连接是否可用，通常返回3G状态，具体情况和设备、操作系统有关。
3. network.isHostNameReachable()：用于检查是否可以解析指定的主机，返回boolean类型。
4. network.getInternetConnectionStatus()：返回互联网的状态值
	
	cc.kCCNetworkStatusNotReachable：无法访问互联网
	cc.kCCNetworkStatusReachableViaWiFi：通过WIFI
	cc.kCCNetworkStatusReachableViaWAN：通过3G网络

示例：

	print("WiFi status:"..tostring(network.isLocalWiFiAvailable()))

	pring("3G status:"..toString(network.isInternetConnectionAvailable()))

	pring("HomeName:"..tostring(network.isHostNameReachable("cn.cocos2d-x.org")))


	local netStatus = network.getInternetConnectionStatus()
	if netStatus == cc.kCCNetworkStatusNotReachable then

		print("kCCNetworkStatusNotReachable")

	elseif netStatus == cc.kCCNetworkStatusReachableViaWiFi then

		print("kCCNetworkStatusReachableViaWiFi")

	elseif netStatus == cc.kCCNetworkStatusReachableViaWAN then

		print("kCCNetworkStatusReachableViaWAN")

	else 

		print("Error")
	
	end

##### 1.2 HTTP请求

Quick中使用下面的接口向服务器发起HTTP请求。

	network.createHTTPRequest(callback, url, method)
	参数：
	1 callback：HTTP请求状态回调函数。一次请求会有多次状态回调。
	2 url：请求的网络地址
	3 method：GER和POST

示例：
           

GET：用于信息获取

	local function onRequestCallback(event)
	
		local request = event.request

		if event.name == "completed" then

			print(request:getResponseHeadersString())

			local code = request:getResponseStatusCode()
			if code ~= "200" then
				--请求结束，没有返回200响应代码
				print(code)
				return
			end

			--请求成功，显示内容
			print("response length"..request:getResponseDataLength())
			local response = request:getResponseString()
			print(response)

		elseif event.name == "progress" then

			-- 获取当前进度
			print("progress"..event.dltotal)

		else

			--请求失败，显示错误代码和错误信息
			print(event.name)
			print(request:getErrorCode(), request:getErrorMessage())
			return

		end

	end

	local request = network.createHTTPRequest(onRequestCallBack, "http://www.baidu.com", "GET")
	request:start()


POST:用于更新数据

	local request = network.createHTTPRequest(onRequestCallback, "http://127.0.0.1:1234/hello", "POST")
	request:addPOSTValue("name", "laoliu")
	request:start()

	-- 回调方法和GET一致
	-- 如果要传递自定义数据，使用以下接口
	request:setPOSTData("this is")

### 2 SocketTCP

 Cocos2d-Lua中集成了LuaSocket，但是使用略微复杂。SocketTCP后被Cocos2d-Lua所采纳，SocketTCP充分利用了Cocos2d-Lua中的事件机制。

##### 2.1基本用法

	--加载进内存
	SocketTCP = require("FrameWork.cc.net.SocketTCP")
	local socket = SocketTCP.new("127.0.0.1", 1234, false)
		1.参数1：服务器地址
		2.参数2：服务器端口号
		3.参数3：链接失败是否重连
	-- 创建连接实例后，向实例中添加状态处理接口，以响应网络状态变化
	socket:addEventListener(SockerTCP.EVENT_CONNECTED, onState)
	-- 向服务器发送请求包
	socket:connect()

##### 2.2完整示例

	SocketTCP = require("Framework.cc.net.SocketTCP")
	
	local socket = SocketTCP.new("172.0.0.1", 1234, false)

	local function onStatus(event)

		print("Socket status: %s", event.name)

		if event.name == SocketTCP.EVENT_CONNECTED then
			--向服务器发送数据，格式需要用Quick提供的ByteArray来转换
			socket:send(ByteArray.new():writeStirng("Hello server, i`m SocketTCP"):getPack())
		end

		if event.name == SocketTCP.EVENT_DATA then
			-- 接受到服务器发送的数据保存在event.data中
			print("Socket Receive data : %s", event.data)
		end

		if event.name == SocketTCP.EVENT_CLOSE then
			print("Socket Close")
		end

		if event.name == SocketTCP.EVENT_CLOSED then
			socket = nil
		end

	end

	socket:addEventListener(SocketTCP.EVENT_CONNECTED, onState)
	socket:addEventListener(SocketTCP.EVENT_CLOSE, onStatus)
	socket:addEventListener(SocketTCP.EVENT_CLOSED, onStatus)
	socket:addEventListener(SocketTCP.EVENT_CONNECT_FAILURE, onStatus)
	socket:addEventListener(SocketTCP.EVENT_DATA, onStatus)

	socker:connect()

	上述把所有的事件都绑定在onStatus回调函数中，内部通过判断event.name来区分各个事件，也可以分开实现每个事件的回调函
	