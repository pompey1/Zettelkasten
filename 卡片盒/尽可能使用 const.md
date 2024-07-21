202407212355
Status: #idea
Tags: [[Effective C++]]
# 尽可能使用 const
对于指针来说左定值，右定向，迭代器的行为和指针类似
但是定向的迭代器没有专门的修饰符，一般都有专门的对象来实现
## Bitwise Const 和 Logical Const
常量的两种派别
Bitwise Const 意味着一个对象的任何一个 bit 都不能改变，但是对象内指针指向的对象改变并不会违反 Bitwise Const

Logical Const 意味着一个对象的部分数据可变，一般用 mutable 属性修饰可变的数据，这样即使在 const 成员函数中也能对他惊醒修改

## 重载技巧
如果一个 const 函数和一个 non-const 函数有很多重复的步骤，可以考虑在 non-const 函数中通过 staic_cast 和 const_cast 组合来调用 const 函数，从而使用避免代码重复

这里需要注意的是，一定要从 non-const 函数调用 const 函数，而不是反过来，在 non-const 中对数据进行任何修改都是合理的

---
# References