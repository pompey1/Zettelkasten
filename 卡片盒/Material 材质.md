202407011334
Status: #moc
Tags: [[渲染]]
## MaterialInstance 和 Material 的交互
在材质编辑器中加入 Scalar Parameter、Vector Parameter 或者 Texture Parameter 节点，都将在 MaterialInstance 提供对应的接口
## DynamicMaterialInstance
在 Runtime 可以根据任意一个 Material 创建一个 DynamicMaterialInstance，可以实时更改 Material 暴露出来的参数
# Tips & Tricks
作为参数的传入的 Texture 可以给他的 RGBA 通道分别取名

先连好线，再右键创建一个 reroute，右键 reroute 可以将其转换为一个连接好的 named reroute

---
# References