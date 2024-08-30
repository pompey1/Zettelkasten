202408271342
Status: #idea
Tags: [[Wwise]]
# Wwise 251 性能优化
# L1 了解资源用量
### SoundBank 工作原理
SoundBank 是一个包含了
1. Audio Source
2. Event
3. Audio Structure 信息
4. 流播放预取数据
5. 流播放引用
的 **数据包**。一个平台和一种语言对应一组 SoundBank

### Auto-Defined SoundBank
Wwise 2022 引入了 Auto-Defined SoundBanks 来取代 User-Defined SoundBanks
1. 一个 Event 一个 SoundBank
2. SoundBank 只保存了 Event 和结构信息，没有保存资源，如果多个 Event 需要相同的资源，他只会在生成时候生成一次，并且加载时也只加载一次
3. 依赖 Unreal 来映射依赖并加载必要的音频。
所有没有包含在 User-Defined SoundBank 中的 Event 和所有 Auxiliary Bus 都会通过 User-Defined SoundBank 来打包成 SoundBank

> [!NOTE] 内存 or 帧率
> 基于 Auto-Defined 生成的 SoundBank 并非完美，他需要引擎实时的加载来播放声音，会降低帧率，所以需要手动分包来实现内存和帧率之间的平衡

### 流播放
直接从硬盘对音频进行流播放，可以减轻内存负荷。流播放会带来一些延迟，所以他并不适合需要精准记时的音效，但是对于环境音和 BGM 来说效果还是可以接受的。

直接在 Audio Source 的 General Settings 中勾上 Stream 选项，生成 SoundBank 时就会将 Media 资源存储到 WEM 文件中来实现游戏中的流播放

开启 Stream 以后，一般会勾上 Non-cachable 选项来最为彻底的节省内存

## 资源优化与资源监控
各种 Profiler 用法，详情见 251

# L2 转码与压缩
使用 Wwise 之前最好导入最高品质的音频，然后直接在 Wwise 工程中为每个文件设置转码设置 Conversion Setting
## Conversion Setting
有一下三种方法减小音频文件的大小
1. 降低采样率（Sample Rate）
2. 使用压缩音频文件格式
3. 减少声道数

## 采样率
对于采样率的选择是一个需要不断迭代的过程，最终播放的音频需要不产生明显的失真或放大固有的噪音
## 音频文件压缩
251 中，主要介绍了PCM、Vorbis、WEM Opus、ADPCM 四种音频文件格式
1. PCM 并非编解码器，他是未压缩的格式。使用很少的 CPU 资源，但会占用大量内存。适用于**短**的、**重复性**的音频和**高保真**声音。
2. Vorbis 编解码器，基于 Seek Table 完成音频文件的暂停、继续和循环。适用于一些**嘈杂**的，对音质要求不高的声。Vorbis 的**采样帧率**越小，时间戳之间的距离越短，声部精度也就越高，但同时内存占用也越高
3. WEM Opus 编解码器，音质略高于 Vorbis，但会占用更多 CPU 资源。适用于非循环音频源（如**对白**）或者长音频源（**环境声**），尤其适合长度超过 200 ms 的声音。在对文件大小要求特别严苛的情况下可以取代 PCM 和 Vorbix 搭配使用
4. ADAPM 编解码器，与 Vorbis 相比几乎不占用任何 CPU 资源，与 PCM 相比能释放更多的内存资源，是一种不错的折中方案，尤其是在移动平台上。适用于**环境声**和其他音效
251 中测试的 PCM 和 Vorbis 性能对照
![[Pasted image 20240828110320.png]]

## 声道数
在每个 Conversion Settings ShareSet 内，都可以选择将音频源转码为多少个声道，可用选项包括
1. As Input，不改变声道配置
2. Mono/Stereo，单/立体声道，将附加的声道混入选中声道
3. Mono Drop/Stereo Drop，单/立体声道弃用，启用除选定声道外所有其他声道

# L3 声部（Voice）管理
Event 触发的所有音频资源都会变成所谓的声部，每个声部都需要一些计算，我们可以只保留最重要的一些计算来节省资源。

> [!NOTE] Voice 和 Sound Voice
> 注意不要混淆声部（Voice）和用于对白的语音（Sound Voice）
## 了解虚声部 Virtual Voice
有些声音可能会因为各种原因无法被听到，我们需要将这些无法听到的声音发送到虚声部，从而避免默认情况下所有声部都要执行的一系列处理，包括
1. 对音频文件解码
2. 对声音重新采样（应用 Pitch Shift）
3. 应用效果器和滤波器
4. 执行音量计算
当声部变为虚声部时，Wwise 将跳过除了第四步音量计算意外的所有其他步骤
### Max Voice Instances 和 Volume Threshold
Project -> Platform Settings 中可以设置项目全局的最远声音距离和声音阈值
### Virtual Voice Behavior
在 Property Editor 中转到 Advanced Setting 选项卡，包括四个选项
1. Continue to Play: 适用于 BGM 等需要常驻的音频
2. Kill Voice: 适用于脚步声等较短的一次性声音
3. Send to virtual voice: 略
4. kill if finite else virtual: 适用于工程中大部分 Audio Structure
### 设置音频播放优先级
Wwise 内置了优先级系统方便管理声部在实声部和虚声部之间的转换，可以直接设置 Priority 和 Offset priority by （优先级的距离衰减）
### 声部限值
详见 Wwise 251
## 细节层次
可以直接通过 RTPC 曲线来控制 Switch Group 的变换，详见 Wwise 251
## 验证 Virtual Voice Behavior 设置
详见 Wwise 251，最大声部实例数一般不会低到 10，Sample 中的 50 是个不错的数字
# L4 效果器 Effect
利用效果器在 SoundBank 内存不变的情况下可以增加音乐的变化，详情见 Wwise 251
# L5 平台管理
详情见 Wwise 251
# L6 SoundBank 粒度
## 弃用 SoundBank 中的音频
可以在 SoundBank Editor 中的 Game Syncs 选项卡中去除不需要的 switch，此举也可以降低 Soundbank 内存
## 媒体重定位 Media Relocation
Wwise 官方案例采用了 Region SoundBank 的分包方案，只有到达某一个区域的 Trigger 才会触发这个区域的 SoundBank 加载。支持媒体重定位的编码器可以保证在离开某个区域以后正在播放的 Event 能够继续播放完，并且可以快速卸载 SoundBank，Vorbis、ADPCM、Opus 和 PCM 都支持媒体重定位
## SoundBank 优化
一般 SoundBank 分包在开发后期才会开始
# L7 运行时管理

## 运行时性能分析选项
Profiler 可以选择 Profiler Only 选项来提高运行时性能，专注于性能测试
## 监控内存用量
1. Media：SoundBank 加载的压缩和未压缩媒体
2. Processing：用于解压、应用效果器、混音和 DSP 处理的音频缓冲区
3. Processing（Plug-in）：Convolution Reverb、RoomVerb 或 Time Stretch 等插件的内存分配
4. Integration：特定于集成的内存分配（比如回调或捕获音频输出）
剩余详情见 https://www.audiokinetic.com/library/edge/?source=Help&id=memory 页面
## 设置 Memory Allocation Size Limit
如果对游戏内存预算有限，可以设置一个 Memory Allocation Size Limit。实际开发中，一般会用 Playback Limit 来限制声景和内存，这只是迫不得已的保底措施。
## 匮乏
在耗尽资源预算时通常会收到匮乏错误，这里错误消息用于提示用户降低资源用量或分配更多资源。常用的匮乏错误包括以下两种
1. Voice Starvation：没有及时处理音频缓冲区，此时，音频可能会出现毛刺噪声或异常
2. Source Starvation：播放带宽太低，无法满足播放需求

---
# References