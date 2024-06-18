202406190012
Status: #idea
Tags: [[GAS]] [[动画]] [[ActionRPG]]
# ActionRPG 中的连招系统

1. 一个 Ability 对应一整套完整的连招
2. GA 的 EffectContainerMap 中配置 GameplayTag2EffectContainer （EffectContainer : TargetType2GEClass）
3.  GameplayTag 配置在 NotifyState 中，通过  BeginWeaponAttack 事件缓存到武器蓝图中，武器蓝图通过 ActorBeginOverlap 事件将 GameplayTagEvent 发送到碰撞的 Actor 上（TargetType **可能**就是在这里筛选的）
4.  Montage Section 的核心代码在主角蓝图的 JumpSectionForCombo 中，明天耐心看完

---
# References