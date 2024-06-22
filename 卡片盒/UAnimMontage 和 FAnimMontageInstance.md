202406202053
Status: #idea
Tags: [[动画]] [[资产管理]]

UAnimMontage 对象是 Content 中的资产，从编辑器将资产拖入蓝图编辑面板的过程本质上就是将UAnimMontage 资产序列化的过程，FAnimMontangeInstance 是 AnimInstance 在播放 Montage 时根据 UAnimMontage 对象创建的真正播放的 Montage 实例

FAnimNotifyEvent 实际存储在 UAnimSequenceBase 部分中
![[Pasted image 20240621234853.png]]

所以在以下这些继承自 UAnimSequenceBase 的资产类型中都可以保存动画通知（实际上运行时存储的是 FAnimNotifyEvent，是对 UAnimNotify 的一层封装）
![[Pasted image 20240621234953.png]]
# UAnimMontage 和 FAnimMontageInstance

---
# References