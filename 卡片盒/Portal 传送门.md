202412192343
Status: #idea
Tags: [[编程挑战]]
# Portal 传送门
# 项目设置
将项目设置中的反走样设置为 FXAA，而不是默认的 TAA
将项目设置中的 Motion Blur 去掉
这样能避免 RT 中的渲染效果差
摄像机近平面距离越小越好

# RT 分辨率
推荐创建动态材质，创建动态 RenderTarget，获取视口的 Size 来给动态生成的 RenderTarget 设置分辨率

并且推荐在 Tick 中创建 Check 函数，这样即使玩家切换了当前视口的分辨率，依然可以让 RT 保持和玩家自身的分辨率保持一致

# 相机设置
将 Aspect Ratio Axis Constraint 设置为 AspectRatio_MajorAxisFOV
假设你在看一幅画，无论画框的宽度和高度如何变化，画的较长边始终保持不变，画的较短边会根据画框的变化进行拉伸或压缩

# 射线检测坑点
Plane 无法用来碰撞检测和射线检测，因为它本身只有一个面，无法正常生成 PhysicalAsset


---
# References