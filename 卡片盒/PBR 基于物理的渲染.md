202406272152
Status: #dictionary 
Tags: [[图形学]]
# 光照类别
入射光可以分为 Direct 和 Indrect 两种
出射光可以分为 Specular 和 Diffuse 两种
入射和出射互相组合可以组合四种不同的光照交互

- Direct Diffuse（最简单）
- Direct Specular（需要考虑到相机位置，稍微复杂点）
- Indirect Diffuse（计算量最大，需要引擎的全局光照方案支持，经常先离线渲染再储存到光照贴图中）
- Indirect Specular（也可以直接称之为 Reflection，和 Indirect Diffuse 一样复杂，计算方案包括：反射探针、平面反射 PlanarReflection、SSR、屏幕空间光线追踪等）
# Light Properties
- Direct Diffuse
- Indirect Diffuse
- Direct Specular
- Indirect Specular
- Shadows
- Ambient Occulusion
AO 也是材质编辑器中的引脚，一般在建模软件中烘焙出贴图使用，可以理解为 Indirect Diffuse 的乘数
# Surface Properties
## Base Color
现实中的材质 sRGB 值往往是在 20~240 之间，粗糙的表面最低值会高 50 左右

> [!Warning] Warning
> 注意，Base Color 只有前三个通道是有效的，alpha 通道的值 Base Color 会直接忽略

## Normal
永远不要手绘法线贴图，用高模烘焙低模流程程序化制作法线贴图
## Specular（非金属专用引脚）
- 非金属材质的 Specular 反射计算乘数一般在 0.04 左右
- 在非金属情况下，UE 中的 Specular 参数 0.5 映射的（俯视）反射计算乘数为 0.04，1.0 映射的（俯视）反射计算乘数为 0.08
- 0.08 的（俯视）反射计算乘数对大多数物体来说已经太高了
- 由于菲涅尔效应，平视视角 glancing angles 下的任何表面（无论金属还是非金属）的反射计算乘数都是 1，但是 PBR 的光照模型一般都会将菲涅尔因子 F 加入计算，所以开发者一般无需专门考虑
- **Specular Map** 大多数像素的值应是 0.5，其制作一般比较简单，直接将贴图乘以0.5即可，只有像岩石缝隙这种完全不应该反射的地方才需要使用更低的值来覆盖
## Roughness
- 白色是粗糙，而黑色是光滑
- **Roughness Map** 的制作没有技术上的约束，可以完全由美术选择，美术可以自己选择哪里需要光滑，哪里需要粗糙来制作符合故事背景或者符合现实的粗糙度贴图
## Metallic
- Metallic 参数为 0 时（完全非金属），BaseColor 代表漫反射计算的乘数，Specular 代表反射计算的乘数
- Metallic 参数为 1 时（完全金属），**BaseColor 代表 0 到 1 的反射计算乘数，而 Specular 节点直接禁用**，具体原因见参考文章[金属，塑料，傻傻分不清楚 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/21961722)
- **Metallic Map** 一般不会考虑 0 到 1 之间的中间值，不是 0 就是 1
- 所有金属的 BaseColor (前文解释过了) sRGB 值都在 180 以上
![[PBR 的材质属性.png]]

---
# References
[金属，塑料，傻傻分不清楚 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/21961722)

[由浅入深学习PBR的原理和实现 - 0向往0 - 博客园 (cnblogs.com)](https://www.cnblogs.com/timlly/p/10631718.html#229-%E7%8E%B0%E9%98%B6%E6%AE%B5%E7%9A%84bxdf2019%E5%B9%B4)