202406212212
Status: #idea
Tags: [[GAS]] 
# GameplayEvent
GAS 原生提供了一套 GameplayEvent 的回调体系

## 绑定流程
ASC->AddGameplayEventTagContainerDelegate(......)
## 调用流程
![[GameplayEvent.png]]
# GameplayEffect

GA 的 EffectContainerMap 中配置 GameplayEvent 的 Tag 和对应的 GameplayEffectContainer  （TargetType and TargetGameplayEffectClasses   ）

---
# References