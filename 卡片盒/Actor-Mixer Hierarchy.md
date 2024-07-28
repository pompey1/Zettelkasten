202407271622
Status: #idea
Tags:  [[Wwise]]
# Actor-Mixer Hierarchy
Actor-Mixer Hierarchy 下有很多对象可供调遣
## Sound SFX ![[Pasted image 20240727162607.png]]
最常用的 Actor-Mixer 对象，用于播放一段音频

SFX 对象并不直接代表音频文件，它代表音频文件播放时将要通过的一个**通道**，通道有很多控制手段来操控真正的音频文件，这些音频文件存放在**音轨**里面，音轨又被送入通道

Wwise 的优势在于可以在最后把声音整合到游戏中时再决定如何压缩文件大小，所以导入音频文件时直接导入无损的 wav (Wave) 格式也没有问题

Sound SFX 对象的名称颜色代表这个文件是否经历过**转码**的过程，这个过程通常发生在生成 SoundBank（声音包）的时候，蓝色代表没有转码过，<span style = "color:silver;">白色</span>代表已经转码过，<span style = "color:blue">蓝色</span>代表没有转码过，<span style = "color:red">红色</span>代表还没有导入音频文件
## Sound Voice ![[Pasted image 20240727162628.png]]
Sound SFX  + 本地化功能
## 批处理
直接导入一整个文件夹的音频文件可以在导入时自动创建一个虚拟文件夹，其本身没有任何作用，只是方便分类，但是你可以将其类型改变成 Random Container，这样就能随机播放其中的音频，取消勾选相应的 Sound SFX 对象可以避免它加载到内存中

---
# References