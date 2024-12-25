202412181757
Status: #idea
Tags:  [[C++]]
# DoOnce
C++ 中有种 Trick 的写法，当你想执行一段程序生命周期中只执行一次的逻辑，但又需要考虑到前后的依赖关系以至于不能直接将这段逻辑放在合理的位置时，可以考虑这样写，如果希望在一个线程中执行一次的话可以把 static 换成 thread_local
```C++
static bool bOnce = false;  
if (!bOnce && GEventDrivenLoaderEnabled)  
{  
    bOnce = true;  
    FGCObject::StaticInit(); // otherwise this thing is created during async loading, but not associated with a package  
}
```
出自 UnrealEngine 源码 AysncLoading.cpp

---
# References