202407072347
Status: #idea
Tags: [[AI]]
# Navmesh
## 应用
Navmesh 在引擎中的 Gameplay 的应用需要两个 Actor: 1. Navmesh Bounds Volumn 2. Recast Navmesh .还有诸如 NavMesh Modifier 的 Actor 能为寻路条件提供更加灵活的调整，暂时按下不表
## Navmesh Bounds Volumn
1. 根据环境的几何信息烘焙出 Navigation Mesh （凸多边形集），之后寻路算法会以这个凸多边形为节点
2. 按 P 显示当前烘焙的寻路节点范围
## Recast Navmesh
1. 在大纲中的 Actor Detail 面板和 Project Setting 中都可以设置 NavMesh Agents 信息（对于寻路对象的描述，包括碰撞范围等），你可以设置多个 NavMehs Agents 并一对一烘焙对应的 NavMesh 节点

# 底层实现
涉及模块包括 AIModule、NavigationSystem、Navmesh (3rd)

---
# References