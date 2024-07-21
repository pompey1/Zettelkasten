202407190108
Status: #idea
Tags: [[Effective C++]]
# 用 const、enum、inline 代替宏
## const
define 意味着编译器无法看到其名称，他也无法存储在符号表中，所以报错时会难以理解
## inline
使用内联函数可以避免 define 可能遇到的异常情况（纯粹的文本复制黏贴容易导致调用顺序问题）
## enum hack
enum hack 的行为与 const 和 inline 相比更接近于宏
1. 绝对不会导致额外的内存分配（对于一些“聪明”的编译器来说，即使是 const 变量也不会分配内存）
2. 对他取地址不合法
3. enum hack 是模板元编程常用的基础技术

---
# References