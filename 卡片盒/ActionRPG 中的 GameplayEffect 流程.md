202406212212
Status: #idea
Tags: [[GAS]] 
# GameplayEvent
GAS 原生提供了一套 GameplayEvent 的回调体系

## 绑定流程
ASC->AddGameplayEventTagContainerDelegate(......)
## 调用流程
![[GameplayEvent.png]]
# GameplayEventData
Payload 是 GameplayEventData 对象，他是 GameplayEvent 自带的数据，可以携带很多信息，比如碰撞角色等，目前 ActionRPG 中 GameplayEventData 的设置比较简单，只是把触发 OnComponentBeginOverlap 的 OtherActor 设置为了 Target，所以目前此项目只支持攻击一个目标

# GameplayEffect

GA 的 EffectContainerMap 中配置 GameplayEvent 的 Tag 和对应的 GameplayEffectContainer  （TargetType and TargetGameplayEffectClasses   ）

---
# References