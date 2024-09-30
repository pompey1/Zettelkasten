202407091805
Status: #idea
Tags: [[Assets 资产]]
# AssetRegistry 资产注册表
资产注册表缓存在 Intermediate 中，有一个 bin 文件和一个文件夹，引擎打开以后 Discovering Asset Data 的同时就会生成这个缓存，利用资产注册表，可以使用高级搜索语法筛选特定的资产，详见 [【虚幻引擎】内容浏览器高级搜索语法指南与制作自定义过滤器_虚幻 过滤文件-CSDN博客](https://blog.csdn.net/Utwelve/article/details/131099893)
## AssetRegistrySearchable
在序列化 UObject 的时候，标记为 AssetRegistrySearchable 的 Property **属性名**和**值**（通过 ExportTextureItem）会加入 TagsAndValues 保存在 AssetData 中，并跟随 AssetData 序列化，.uasset 文件头中的 Package Summary 保存了 TagsAndValues 相对于文件的偏移，方便引擎提前加载

---
# References
1. [【虚幻引擎】内容浏览器高级搜索语法指南与制作自定义过滤器_虚幻 过滤文件-CSDN博客](https://blog.csdn.net/Utwelve/article/details/131099893)
2. [UE 中利用反射为资产建立属性缓存 | 循迹研究室 (imzlp.com)](https://imzlp.com/posts/71162/)