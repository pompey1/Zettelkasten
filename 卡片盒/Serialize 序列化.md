202407091658
Status: #idea
Tags: [[Assets 资产]]
# Serialize 序列化
## USaveGame
UE 原生提供的存档功能，支持单机游戏常见的多 slot 存档

USaveGame 本身只是一个空类，具体功能需要通过 UGameplayStatics 相关 API 来实现
## FFileHelper
UE 原生提供的文件功能，灵活度更高，可以自由选择保存路径，搭配 JSON 序列化的一些第三方库食用更佳
## << 操作符和 Serialize 方法
<< 操作符底层一般都是调用的 Serialize 方法，他们传入 FArchive 对象用来同时处理保存和加载，并没有像 C++ 一样通过不同的操作符区分保存和加载（可能是为了减少虚函数表占用的内存？）具体是保存还是加载根据 FArchive 的具体类型决定，在实战中一般不直接判断类型来区分，而是通过 IsSaving 或者 IsLoading 这些判断 Runtime 数据的接口来区分代码

---
# References
