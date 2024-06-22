202406190012
Status: #idea 
Tags: [[GAS]] [[动画]] [[ActionRPG]]
# ActionRPG 中的连招系统

1. 一个 Ability 对应一整套完整的连招
2. GA 的 EffectContainerMap 中配置 GameplayTag2EffectContainer （EffectContainer : TargetType2GEClass）
3.  GameplayTag 配置在 NotifyState 中，通过  BeginWeaponAttack 事件缓存到武器蓝图中，武器蓝图通过 ActorBeginOverlap 事件将 GameplayTagEvent 发送到碰撞的 Actor 上（TargetType **可能**就是在这里筛选的）
4.  Montage Section 切换的核心代码在主角蓝图的 JumpSectionForCombo 中，自动战斗模式下直接在 NotifyState 中调用，手动战斗模式下在主角蓝图的 DoMeleeAttack 中调用，通过判断 IsUsingMelee 判断是激活 Ability 还是调用 JumpSectionForCombo 触发连招，其实思路挺直白的
5.  可以通过 ASC 的接口判断是否存在对应 GameplayTag 的已被激活的 AbilitySpec，再通过 AbiltySpec 获取他所有的 Instances
6. MontageSetNextSection

---
# References