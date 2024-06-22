202406200005
Status: #idea
Tags: [[动画]] [[蓝图]]
# AnimNotify 和 AnimNotifyState

AnimNotify 一般是立即发生的，提供了 Notify 的自定义接口，可以实现比较简单的逻辑，比如触发特效，触发音效等

AnimNotifyState 一般是持续的，提供了 NotifyBegin 和 NotifyEnd 的自定义接口，可以实现比较复杂的逻辑，或者说实现带有“状态”的逻辑

AnimNotify 可以在 Montage 面板的 Details 中设置参数，相当于在 Montage 面板中创建了一个实例，并且会被序列化保存在 UAnimMontage 资产中


---
# References