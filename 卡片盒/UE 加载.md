202412181748
Status: #idea
Tags: [[Assets 资产]]
# UE 加载
# 用户层接口
（1）同步加载接口，例子太多了，略过
（2）异步加载接口，FStreamableMananger 的 RequestAsyncLoad、UAssetMananager 的 LoadPrimaryAsset 等
# 数据驱动
UE 内部各种花里胡哨的加载接口到最后调用的都是 UObjectGlobals.h 中的全局方法 LoadPackageAsync，即使是同步加载，也是调用 LoadPackageAsync 以后调用 FlushAsyncLoading 实现的

LoadPackageAsync 会根据引擎启动时设置的 AysncPackageLoader 类型（AysncPackageLoader 是一个接口）来执行对应的 LoadPackage 方法，**默认情况**下会调用FAsyncLoadingThread::LoadPackage

FAsyncLoadingThread::LoadPackage 本身并不会直接加载，他只是把加载请求和 FAsyncPackageDesc 添加到对应的列表中
# 分帧加载
## TickAsyncThread
TickAsyncThread 是主线程和 Async 线程共同的分帧加载起点，他有两种触发方式
（1）从主线程的 Tick 触发
在 EditorEngine 和 Level 的 Tick 中都会调用全局方法 ProcessAsyncLoading（所以不管是 Editor 和 Game 都能正常使用加载），然后会转发到 AsyncPackageLoader 子类重载的 ProcessLoading 中处理加载请求（默认情况下会调用 FAsyncLoadingThread::ProcessLoading）ProcessLoading 是对 TickAsyncLoading 添加性能测试打桩的封装

TickAsyncLoading 最终会执行到 TickAsyncThread

> [!NOTE] 同步加载还是异步加载？
> FlushLoading 之所以阻塞主线程是因为他会在加载完成前在主线程循环执行 TickAsyncLoading，所以虽然调用了 LoadPackageAsync，但同步加载的本质仍然是同步加载，只不过入队加载请求的过程和异步加载相同，所以复用了 LoadPackageAsync 的接口，本质上应该是 EPIC 自己的命名有问题，可能是因为架构设计之初没有考虑到同步加载的情况

（2）从 Async 线程的 Run 触发
在 Run 函数中也会触发 TickAsyncThread，编辑器下不会开启 Async 线程，需要在 ini 文件中加入配置

```
[/Script/Engine.StreamingSettings]
s.AsyncLoadingThreadEnabled=True
s.EventDrivenLoaderEnabled=True
```
同时该配置只有 Cook 以后才会生效，PIE 下也是无效的，所以在编辑器下只能在主线程分帧加载实现异步加载，无法启动多线程加载（EDL 也是针对 Cook 以后的资源的，所以编辑器下也无效）
## AysncPackageThread
TickAsyncThread 会依次执行 (1) CreateAsyncPackagesFromQueue 和 (2) ProcessAsyncLoading

(1) CreateAsyncPackagesFromQueue 会根据之前在 LoadPackageAysnc 中塞入列表中的加载请求和 FAsyncPackageDesc 创建或者找到已经创建的 FAsyncPackage 

(2) ProcessAsyncLoading 在没开启 EDL 的情况下，会分帧遍历所有创建好的 FAsyncPackage，对每个 FAsyncPackage 调用 TickAsyncPackage，也就是说实际的加载逻辑分散在每个 FAsyncPackage 中了。值得注意的是，ProcessAsyncLoading 中也会执行 CreateAsyncPackagesFromQueue，确保列表中不再有任何加载需求（这里根据注释可知，EPIC 内部仍然有争议是否在已经超时的情况下继续处理列表）
## FAsyncPackage
FAsyncPackage 是对 FLinkerLoad 的封装，包括一些加载过程中的中间数据

TickAsyncPackage 中会分帧执行一系列逻辑，包括 CreateLinker （创建 FLinkerLoader 用来反序列化 uasset 文件）、LoadImports（加载依赖的 Package）CreateImports（反序列化 Import 表，获取 Import 表中的 UPackage）等
## FLinkerLoad
具体的反序列化逻辑，略

---
# References
https://zhuanlan.zhihu.com/p/634261508