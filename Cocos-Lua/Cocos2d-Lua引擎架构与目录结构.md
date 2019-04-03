#Cocos2d-Lua引擎架构与目录结构
###1.引擎架构
Cocos2d-Lua是由Cocos2d-x衍生出来的，他们之间的关系如下图所示：
![](http://7xqzxs.com1.z0.glb.clouddn.com/160808Cocos2d-Lua%E5%BC%95%E6%93%8E%E6%9E%B6%E6%9E%84.jpg)

1. 橙色区域的模块与具体平台相关，引擎渲染基于跨平台的OpenGL ES。
2. OpenGL ES之上是Cocos2d-x引擎架构，Cocos2d-x与ThirdPartLibrary对外暴露C++API
3. Lua Binding把C++ API映射为Lua API， 而Quick Framework基于Lua API二次封装并拓展了接口
4. 游戏最上层，可以同时使用Lua API和Quick框架
5. Coco2d-x内部又可以细分为多个模块
![](http://7xqzxs.com1.z0.glb.clouddn.com/160808Cocos2d-x%E6%A8%A1%E5%9D%97.jpg)

###2.引擎文件结构
##### 2.1 引擎根目录
![](http://7xqzxs.com1.z0.glb.clouddn.com/0821Quick-Cocos2d-Lua%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png)

1. build：编译Cocos2d-x库的各平工具
2. cocos：Cocos2d-x引擎C++代码
3. docs：Cocos2d-Lua的文档
4. extension：GUI、物理引擎等扩展模块
5. external：第三方库
6. licenses：引擎以及第三方库的说明文件
7. quick：Quick框架代码
8. tools：项目生成工具
9. player3.bate：启动Cocos2d-Lua模拟器的工具
10. setup_mac.sh：Mac下设置Cocos2d-Lua环境变量的脚本文件
11. setup_win.bate：Win下设置Cocos2d-Lua环境变量的脚本文件
12. unins0000.dat、unins.exe：Windwos下引擎卸载程序

##### 2.2 Quic框架
![](http://7xqzxs.com1.z0.glb.clouddn.com/0821Quick%E6%A1%86%E6%9E%B6%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png)

1. bin：Quick框架相关可执行脚本
2. cocos：Cocos2d-Lua binding接口
3. framework：Quick框架的Lua源码
4. lib：Quick框架的C++源码
5. player：模拟器的源码
6. samples：Quick框架测试案例
7. templetes：项目创建模板
8. welcome：项目启动欢迎模板

##### 2.3 framework目录结构
![](http://7xqzxs.com1.z0.glb.clouddn.com/0821Quick-Framework%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png)

1. cc:扩展Cocos2d-x Lua API，提供Quick基本模块（符合脚本风格的事件模块、组件架构）
2. cocos：Quick对Cocos2d-Lua模块的加强
3. deprected：废弃的接口
4. plantform：Quick平台相关的接口
5. anysdkConst.Lua：AnySDK中常量的定义
6. audio.lua：音乐、音效管理
7. cocos2dx.lua：用来导入Cocos2d-x Lua API
8. crypto.lua：加解密、数据编码库
9. debug.lua：提供调试接口
10. device.lua：提供设备相关属性的查询以及设备功能的访问
11. display.lua：与显示图像、场景相关的功能
12. filter.lua：滤镜功能
13. function.lua：提供一组常用函数，以及对Lua标准库的拓展
14. init.lua：quick框架的初始化
15. json.lua：JSON的编码与解码
16. luaj.lua：Lua与Java之间的交互接口
17. luaoc.lua：Lua与Objective-C之间的交互接口
18. network.lua：网络接口封装。检查WIFI、3G网络接口情况等
19. schduler.lua：全局计时器、计划任务，该模块在框架初始化时不会自动载入
20. shortcode.lua：一些经常使用的短代码，比如设置旋转角度。
21. transition.lua：为动作和驱动对象添加效果。
22. ui.lua：创建和管理用户界面

###3.项目文件结构
![](http://7xqzxs.com1.z0.glb.clouddn.com/0821Cocos2d-Lua%E9%A1%B9%E7%9B%AE%E6%96%87%E4%BB%B6%E7%BB%93%E6%9E%84.png)

1. frameworks：iOS、Android等平台的工程文件。
2. res：项目资源文件
3. src：项目源码存放文件夹
4. .project：Cocos Code IDE项目文件
5. config.json：项目信息配置文件
6. debug.log：最后一次log信息存档

![](http://7xqzxs.com1.z0.glb.clouddn.com/0821Cocos2d-Lua%E9%A1%B9%E7%9B%AE%E6%96%87%E4%BB%B6%E7%BB%93%E6%9E%84-src.png)

1. app：游戏界面及逻辑
 * app/MyApp.lua：游戏实例
 * app/scenes：游戏场景文件夹
 * app/scenes/MainScene：游戏的第一个场景
2. cocos：cocos2d-Lua/quick/cocos的复制，将随项目一起打包
3. config.lua：工程的配置文件，包括分辨率
4. main.lua：游戏入口