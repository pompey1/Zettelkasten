202406191107
Status: #idea
Tags: [[内存管理]] [[GC]]
# GCObject

1. 存在意义：开门见山，GCObject 存在的意义是为了避免非 UObject 对象的 UObject 成员被错误回收
2. 我们都知道非 UObject 对象是不会被 UE 的 GC 系统识别和追踪的，所以他的 UObject 成员对象也无法被追踪到这一层被引用关系，这就会导致 UObject 被错误的回收
3. 继承自 GCObject 就可以避免上述问题，GCObject 持有一个静态的 UGCObjectReferencer 对象，他被添加到了 GC 系统的根节点，UGCObjectReferencer 手动设置了 ClassAddReferencedObjects 方法（UE 会调用这个方法来搜集需要被追踪的对象）该方法会转发到他内部持有的所有 GCObject 子类对象（每次构造一个 GCObject 对象时，静态的 UGCObjectReferencer 对象都会将其注册进 ReferencedObjects 中）的 AddReferencedObjects 方法
4.  本质上相当于 UGCObjectReferencer 来帮忙追踪 UObject 对象

---
# References