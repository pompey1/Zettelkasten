202407221058
Status: #idea
Tags: [[Material 材质]]
# RenderTarget
RenderTarget 没有特殊说明一般指的是 TextureRenderTarget2D 资产，UE 中还提供了很多额外的 RenderTarget 类型，包括 CubeRenderTarget（之后考虑用这个模拟风力场）
# ContentExample 中的示例 RenderToTarget
主要演示了材质和 RenderTarget 之间的交互，可以通过材质往 RenderTarget 写内容，也可以通过 RenderTarget 给材质设置参数
## 1.1 Create Textures Useing Rendering Targets
核心节点 Draw Material To Render Target，优势是可以利用 UE 材质体系提供的强大渲染能力来渲染 RenderTarget
![[RenderTarget_1.png]]
## 1.2 Height Map Height
### 程序侧
**ConstructionScript** 
创建两个动态材质实例，一个用来渲染 RenderTarget，一个用来渲染实际的 Actor，按照这里的命名规则，用来渲染 RenderTarget 的材质球可以命名为 M_RT_XXX
**BeginPlay** 
动态创建一个 RenderTarget，将他设置为渲染 Actor 的材质的参数
**TakeDamage**
为渲染 RenderTarget 的材质设置参数
### TA侧
主材质球将 RenderTarget 连接到 WorldPositionOffset 节点上
## 1.3 Fluid Surface
第一时间激发的水柱是应用的位移贴图，而缓慢消失的波浪应用的是法线贴图，这样能最大优化性能

位移贴图准备了三张 RenderTarget，这里命名为位移 RenderTarget，法线贴图准备了一张 RenderTarget，这里命名为法线 RenderTarget

ComputeNormal 材质实例用来绘制法线 RenderTarget，HeightSim 材质用来绘制位移 RenderTarget

之所以需要三张位移 RenderTarget，是因为 HeightSim 材质需要之前的状态来计算当前的海浪状态，以此来模拟海浪的扩散
### 程序侧
**ConstructionScript** 
动态设置 StaticMesh，动态创建用来渲染 Actor 的材质实例
**BeginPlay** 
清理三个资产，并将法线 RenderTarget 设置为主材质参数
**Tick**
1. 判断玩家碰撞（按下不表）
2. 做了 FixedUpdate 处理，与帧率实现了分离
3. 创建一个 HeightSim 材质实例，将之前两张位移 RenderTaget 设置为他的参数，将这个材质绘制当前活跃的位移 RenderTarget 上，这里不是直接使用的 DrawMaterialToRenderTarget 节点，而是采用了 BeginDrawCanvasToRenderTarget->DrawMaterial->EndDrawCanvasToRenderTarget
![[RenderTarget_2.png]]
5. 创建一个 ComputeNormal 材质实例，将当前活跃的位移 RenderTarget （已经计算了海浪模拟的 RenderTarget）设置为这个材质的参数，将这个材质绘制到法线 RenderTarget 上，最后将当前活跃的位移 RendertTarget 设置为主材质的参数
### TA侧
**RenderTarget 计算材质**
有意识的是，绘制到 RenderTaget 的材质都只连接了 EmissiveColor 节点，这样可以避免光照计算的影响（以防万一 RenderTarget 在场景中？）

**主材质**
接收位移的 RenderTarget 连接到 WorldPositionOffset 节点上，接收法线的 RenderTarget，连接到 WorldNormal 节点上

# Capture Camera
在组件中添加 CaptureCamera，可以在他的 Detail 面板选择 RenderTarget

---
# References