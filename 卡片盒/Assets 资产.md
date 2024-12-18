202407091132
Status: #moc
Tags: [[UE C++]]
# Assets 资产
所有 uasset 后缀的文件对于 ue 来说都可以视为资产，umap 是“更高级”的 Primary Asset（主资产）
# Primary Asset 和 Secondary Asset
所有资产都可以分为 Primary Asset 和 Secondary Asset，相比较 Secondary Asset，默认的 Primary Asset 可以在右键注册表信息中看到 Primary Asset Type 和 Primary Asset Name 

AssetMananger 提供了很多接口专门用于 PrimaryAsset ，包括加载，获取 PrimaryAssetID 等

只要实现了 GetPrimaryAssetID() 虚方法的资产都是 PrimaryAsset，不需要限定为 Map

Primary Asset 的正常使用还需要 Asset Mananger 知道自己的路径，所以需要在 Project Setting -> Asset Mananger 的 Primary Asset Types To Scan 中配置

继承了实现 GetPrimaryAssetID() 的 Native Class 的蓝图类也是 PrimaryAsset，可以重写其中的一些方法且无需写一些 Editor 代码， Lyra 中同时使用了 DataAsset 和 蓝图类来配置数据

## Lyra 中的使用案例
![[Pasted image 20240915183159.png]]

# Tips & Tricks
在 Level 的 Asset Action 中选择 Capture Thumbnail 可以直接在 Level 中截图（原理类似 Scenen Capture Component）作为 Content Browner 的资产预览图标

Content Browser 的 Settings 中选择 Thumbnail Edit Mode 可以自由调整资产的展示角度

无需点击搜索栏，直接输入文件名就可以直接在当前目录下快速筛选文件

在资产编辑器的预览中，按住 **L** 移动鼠标可以拖动预览界面的光源

打开 Static Mesh 编辑器，在细节中可以重新设置导入设置，避免重新在 DCC 软件中编辑，包括 Transform 和 Pivot 点位置

**Ctrl + Tab** 可以快速切换不同的资产编辑界面

Editor Preference - Auto Reimport 勾选上 Monitor Content Directories，并配置相关映射，引擎可以自动导入修改后的文件，无需手动重新导入

**Ctrl + P** 快速筛选打开资产

---
# References
1. [资源管理：UASSET 资源加密方案 | 循迹研究室 (imzlp.com)](https://imzlp.com/posts/32412/)
2. [虚幻引擎资产管理总结 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/503069332)
3. [虚幻引擎资产管理 | 虚幻引擎 5.4 文档 | Epic Developer Community (epicgames.com)](https://dev.epicgames.com/documentation/zh-cn/unreal-engine/asset-management-in-unreal-engine)
4. [[中文直播]第33期 | UE4资产管理基础1 | Epic 大钊_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Mr4y1A7nZ/?spm_id_from=333.337.search-card.all.click&vd_source=0e2e99e3ece07b504251867302b25270)