202406190053
Status: #idea
Tags: [[蓝图]]
# Event 和 Function

1. 虽然在 C++ 层面上，Event 和 Function 本质上都是一个成员 UFunction，但是在蓝图层面上，他们是不同的两个概念
2. Event 一般由外部调用，对于所有蓝图全局可见
3. Function 一般由内部调用，对于其他蓝图不可见
4. EventDispatcher 就是给蓝图用的简化版（多播）委托，可以在任意处声明，也可以在任意处使用

---
# References