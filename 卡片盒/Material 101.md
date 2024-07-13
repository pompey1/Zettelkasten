202406302309
Status: #idea
Tags: [[Material 材质]]
## Distortion Material
扰乱材质，可以做一些热空气扭曲之类的效果
## Flip Book
翻页节点，烘焙离线渲染结果到贴图上的经典应用
## Bump Offset & Parallex Occulsion
用来优化法线贴图表现的两种技术手段，他们在应用上都是通过一张 Height Map 来调整 uv 坐标，再将调整后的 uv 坐标传到采样器中

采用这两种节点的贴图视觉效果会跟随玩家的视角改变

Bump Offset 效果一般，但是性能很好，Parallex Occulsion 效果更好，但是性能消耗大（相当于在贴图上上光追）

---
# References