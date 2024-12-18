202412181748
Status: #idea
Tags: [[Assets 资产]]
# UE 加载
UE 内部各种花里胡哨的加载接口到最后调用的都是UObjectGlobals.h 中的全局方法 LoadPackageAsync，即使是同步加载，底层也是调用 LoadPackageAsync 以后通过 FlushAsyncLoading 阻塞主线程实现的

LoadPackageAsync 会根据引擎启动时设置的 AysncPackageLoader 类型（AysncPackageLoader 是一个接口）来执行对应的 LoadPackage 方法，默认情况下会调用 FAsyncLoadingThread::LoadPackage

FAsyncLoadingThread::LoadPackage 本身并不会直接加载，他只是把加载请求和 FAsyncPackageDesc 添加到对应的列表中，在 Engine 的 Tick 中会调用 ProcessAsyncLoading 处理加载请求

非 EDL 情况下，Tick 中会协程执行加载一段循环体，循环体内是实际的加载操作，当加载超时会跳出循环等待下一次 Tick 中继续执行



---
# References
https://zhuanlan.zhihu.com/p/634261508