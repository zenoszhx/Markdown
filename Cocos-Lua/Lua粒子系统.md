#粒子系统

###1.Cocos2d-Lua中的粒子系统

#####1.1Cocos2d-Lua粒子系统的基础关系
粒子中的基础关系如图所示：
![](http://7xqzxs.com1.z0.glb.clouddn.com/0824Coco2d-Lua%E7%B2%92%E5%AD%90%E7%B3%BB%E7%BB%9F.png)
cc.ParticleSystem是粒子的基类，定义了粒子系统的各种基本属性。

#####1.2cc.ParticleSystemQuad
cc.ParticleSystemQuad除了具有cc.ParticleSystem的所有属性外还有以下特点：

1. 粒子的大小可以是任意的浮点数
2. 系统可缩放
3. 粒子可旋转
4. 支持子区域
5. 支持手批量渲染

###2.料子系统批处理节点

###3.粒子属性

###4.粒子编辑器

###5.使用料子系统