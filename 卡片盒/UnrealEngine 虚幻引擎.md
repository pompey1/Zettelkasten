202407071806
Status: #moc
Tags: [[游戏开发]]
# 踩坑日记
1. 源码版引擎很多实用的 Program 也需要编译，比如烘焙光照文件的 UnrealLightmass，打包用的 UnrealPak，安装 UnreaLink 插件所需要的 AutomationTool 和 Automation 文件夹下的一大堆项目
2. UE4 右键菜单出现闪烁、透明、黑屏等现象时，这是因为 Nvidia 驱动更新引发的 Bug，进入[After updating to NVIDIA Game Ready Driver 461.09 or newer, some desktop apps may flicker or stutter when resizing the window on some PC configurations | NVIDIA (custhelp.com)](https://nvidia.custhelp.com/app/answers/detail/a_id/5157/~/after-updating-to-nvidia-game-ready-driver-461.09-or-newer%2C-some-desktop-apps)下载并使用链接中的注册表文件重启
3. 文件加载时后缀是否带 C 需要留心，很多时候加载不出 UClass 都是因为后缀问题导致的
4. UE 中可以重载继承自 UObject 的 << 操作符，这个操作符很有误导性，并非和传统 C++ 一样<< 和 >> 代表相反的意思，UE 中的 << 既可以表示保存，也可以表示加载，具体根据传入的 FArchive 类型决定，引擎源码中常用 FArchive 的 IsLoading() 或者 IsSaving() 等接口来区分，<< 最终一般调用的都是 FArchive 的 Serialize() 方法

---
# References