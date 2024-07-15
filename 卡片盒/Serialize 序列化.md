202407091658
Status: #idea
Tags: [[Assets 资产]]
# Serialize 序列化
很多的通用数据格式都可以用来实现序列化，常见的比如 JSON、字节流（TArray< uint8 > ByteBuffer）、比特流（网络 IO）、XML，UE 中实现了基于前三者数据格式的序列化，这里以讨论字节流为主

# 用户接口
## USaveGame
UE 原生提供的存档功能，支持单机游戏常见的多 slot 存档，USaveGame 本身只是一个空类，具体功能需要通过继承并调用 UGameplayStatics 相关 API 来实现
## FFileHelper
UE 对文件操作的封装的帮助类，其中 API 实现的细节、封装的程度各不相同，有的是对 IFileManager 的一层封装（简单数据用这种即可），有的是通过 UnrealEdEngine 调用了控制台指令（FEexc 接口提供），然后通过调用 UPackage::Save 方法调用了 << 操作符，这里涉及到 uasset 的文件格式，更加复杂
## << 操作符
对于任何 Class 都可以通过重载 << 接口实现自定义的序列化逻辑
# 内部实现
一个 UObject 的序列化实际上分成两部分，数据类型转换和持久化操作，负责他们的 FArchive 的类型是不同的
## 访问者模式
<< 并没有像 C++ 一样通过不同的操作符隔离保存和加载代码，具体是保存还是加载根据 FArchive 的具体类型决定，在实战中一般不直接判断类型来区分，而是通过 IsSaving 或者 IsLoading 这些判断 Runtime 数据的接口来区分代码

<< 操作符中遍历调用成员对象重载的 << ，<< 会计算好数据的地址和来源，最终调用 FArchive->Serialize 方法

FArchiveState （UE5 以后，UE4.21 版本中全都耦合在 Archive 中）中有很多实用的工具方法，如 Tell、TotalSize、AtEnd 方法等
## 持久化 Persistent
如果涉及到在硬盘上的读写操作，需要通过 IFileManager 接口来创建一个FArchiveFileWriterGeneric 对象，参考 LinkerSave 中的成员对象 saver，最终调用 LinkerSave->Serialize 方法时调用了其 接口来创建一个 FArchiveFileWriterGeneric->Serialize 方法，其中逻辑包括了输入缓冲区的实现和具体操作系统 API 的调用

---
# References
[UE 序列化介绍及源码解析 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/617464719)
