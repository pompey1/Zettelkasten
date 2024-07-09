202407071806
Status: #moc
Tags: [[游戏开发]]
# UnrealEngine 虚幻引擎

## 踩坑记录
1. 源码版引擎很多实用的 Program 也需要编译，比如烘焙光照文件的 UnrealLightmass，打包用的 UnrealPak，安装 UnreaLink 插件所需要的 AutomationTool 和 Automation 文件夹下的一大堆项目
2. UE4 右键菜单出现闪烁、透视、黑屏等现象时，这是因为 Nvidia 驱动更新引发的 Bug，进入[After updating to NVIDIA Game Ready Driver 461.09 or newer, some desktop apps may flicker or stutter when resizing the window on some PC configurations | NVIDIA (custhelp.com)](https://nvidia.custhelp.com/app/answers/detail/a_id/5157/~/after-updating-to-nvidia-game-ready-driver-461.09-or-newer%2C-some-desktop-apps)下载并使用链接中的注册表文件重启

---
# References