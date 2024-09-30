202406242325
Status: #moc
Tags: [[Program 编程]]
# Blueprint 蓝图
# Tips & Tricks
特定的蓝图节点，包括 DestroyActor、SetLifeSpan 等支持**直接**传入 **TArray** 作为传入参数，无需 For Each Loop 遍历
Enum 可以直接使用 For Each 枚举名来遍历，无需使用 For Each Loop

在 PrintString 中加入前缀 **Warning:**  **Error:** 可以自动更改 log 颜色

**Shift + 左键** 可以高亮显示连线

Editor Preference - Blueprint Editor Settings - Allow Explicit Impure Node Disabling，勾选上述 Setting，右键蓝图节点，勾选 **Disable Compile**，可以直接跳过执行这个节点，还可以选择 Development Only 等选项

选择 Event , 在 Detail 面板上点击 **Call In Editor** 即可在编辑器下的 Detail 面板上点击按钮触发 Event 来预览相关逻辑的执行

**Shift + Delete** 可以只删除数据线不删除执行线

蓝图中的 Event 可以自动转换成 Delegate 使用，也可以通过 CreateEvent 方法将蓝图函数转换成 Delegate（EventDispatcher）

通过 Set Members in 结构名 可以隐藏不需要传入参数的节点

蓝图节点可以收藏，收藏以后的蓝图节点会出现在右侧的 Palette 中

选中一些节点 **Ctrl + 数字键** 快速添加书签，**Shift + 数字键** 快速切换书签

蓝图中可以使用 C#/spdlog 风格的 **Format Text** 节点连接字符串和参数

多人调试的时候可以故意设置延迟模拟网络连接不稳定的情况

---
# References
1. [理解蓝图技术架构 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/92268112)
2. [Tips&Tricks (pixelbear.cn)](https://bp.pixelbear.cn/tips)