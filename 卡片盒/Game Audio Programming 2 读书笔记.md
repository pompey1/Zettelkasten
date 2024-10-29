202410241646
Status: #idea
Tags: [[Audio 音频]]
# Game Audio Programming 2 读书笔记

# Low-Level
### Audio Device Callback
当音频设备（或者声卡）的音频数据缓冲区需要新的数据时，会以**回调**的形式通知应用，应用会在回调中填入所需的音频数据。这些音频数据不管是采样率，还是数据流的格式（16-bit、24-bit 字节流）都不一定统一。回调的时间间隔取决于音频的播放时间，同时受到音频资源采样率和缓冲区大小的影响。

一旦音频数据缓冲区被填满以后，就会按需要对其中的数据做重新采样 Resampled、交错 Interleaved、去交错 Deinterleaved、抖动 Dithered 或 缩放 Scaled，以将数据转换成音频设备期望的格式。

一个典型的回调函数签名
```C++
void callback(float *buffer, int channels, int frames, void *cookie);
```

实际使用的 callback 函数签名并不会这么简单，他取决于平台和 API 的设计，不同的设计方案有 wasapi、advanced linux sound architecture、core audio apple 等



---
# References