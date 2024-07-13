202407131844
Status: #idea
Tags: [[Animation 动画]] [[Material 材质]] [[Camera 相机]] [[ALS Advanced Locomotion System 高级运动系统]]
# AnimCurve 动画曲线
- AnimCurve 存储在骨架上，可以直接在动画序列上添加
- Type 表明此曲线是否用于 MorphTarget 或 Material
- Weight 曲线当前的值
- Auto 控制是否自动根据曲线设置 Weight 值，一般都是启用的，禁用一般是为了调试
- Bones 链接的骨骼数量
# 应用
动画曲线的应用十分广泛，他是动画驱动游戏的典型案例，主要更新方式有如下三种
1. Material Instance 官方示例就是运用了 AnimCurve 来实现对 MaterialInstance 的更新，这里只需要将曲线名称和 MI 参数名称保持相同，并且将 AnimCurve 的 Type 设置为作用于 Material 就可以实现数据绑定了（这种靠对名字的调用方式真是无力吐槽。。。）
2. Anim Blueprint 通过 GetCurrentValue 接口获取当前曲线值，同时如 ALS 所展示，AnimCurve 也可以用来实现对 Camera 的更新
3. Morph Target 中能够用来控制形变的动画曲线是在 fbx 文件中的 Morph Target 导入到 UE 以后自动生成的

# ALS 中的应用
将一堆 Pose 集合在一个 AnimSequence 资产中
1. 利用  [[Evaluate]] 节点，输入特定的时间在资产中选择特定的 Pose（其实不是很好的技巧，对于动画师来说可能好处理一点，但是对于程序来说，用一个 magic number 来获取对象实在不是一个好主意）
2. 也可以在附加动画中直接选择动画资产和对应的帧率选择对应的 Pose 作为附加动画的基础 Pose

---
# References
1. [虚幻引擎中的动画曲线 | 虚幻引擎 5.4 文档 | Epic Developer Community (epicgames.com)](https://dev.epicgames.com/documentation/zh-cn/unreal-engine/animation-curves-in-unreal-engine)
2. [【UE5】论动作游戏战斗系统的细节 技术向演示_单机游戏热门视频 (bilibili.com)](https://www.bilibili.com/video/BV1xb411D7PJ/?spm_id_from=333.999.top_right_bar_window_default_collection.content.click)