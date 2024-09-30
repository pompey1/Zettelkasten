202406202053
Status: #idea
Tags: [[Animation 动画]] 
# AnimMontage
## UAnimMontage 和 FAnimMontageInstance
UAnimMontage 对象是 Content 中的资产，FAnimMontangeInstance 是 AnimInstance 在播放 Montage 时根据 UAnimMontage 对象创建的真正播放的 Montage 实例
## AnimNotify 和 AnimNotifyState
AnimNotify 一般是立即发生的，提供了 Notify 的自定义接口，可以实现比较简单的逻辑，比如触发特效，触发音效等

AnimNotifyState 一般是持续的，提供了 NotifyBegin 和 NotifyEnd 的自定义接口，可以实现比较复杂的逻辑，或者说实现带有“状态”的逻辑

**AnimNotify 可以在 Montage 面板的 Details 中设置参数**，相当于在 Montage 面板中创建了一个实例，这个实例被封装成了一个 FAnimNotifyEvent 对象，可以被序列化保存在 UAnimMontage 的资产文件中

这里需要注意的是 FAnimNotifyEvent 数组实际存储在 UAnimSequenceBase 部分中
![[AnimMontage 动画蒙太奇_3.png]]

所以在以下这些继承自 UAnimSequenceBase 的资产类型中都可以保存动画通知
![[AnimMontage 动画蒙太奇_4.png]]

# 关于无法被 Unlua 覆盖的解释
AnimNotify(State) 是在编辑器阶段为动画资产（动画片段、蒙太奇等继承自 UAnimSequenceBase 的动画资产）添加 FAnimNotifyEvent 时 New 的对象，由该 Event 持有
![[AnimMontage 动画蒙太奇_1.png]]

AnimNotify(State) 的触发在 FParallelAnimationCompletionTask 中，所以是跑在 GameThread 上的，详见 UAnimInstance::TriggerAnimNotifies 方法

![[AnimMontage 动画蒙太奇_2.png]]
Unlua 需要 UObject 在加载地图创建以后的流程中才会实现绑定，自然无法覆盖 AnimNotify 中的方法

---
# References