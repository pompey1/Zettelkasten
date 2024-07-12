202407041545
Status: #idea
Tags: [[Animation 动画]]
## FParallelAnimationEvaluationTask
1. 转发到 Comp->ParallelAnimationEvaluation
2. 支持多线程并发
3. FParallelAnimationCompletionTask 的前置条件
### AnimInstance->ParallelUpdateAnimation
动画蓝图实例更新动画状态
### EvaluateAnimation
在动画蓝图实例更新完成以后计算纯动画数据（Pure animations）并填充 BoneSpaceTransforms
## FParallelAnimationCompletionTask
1. 转发到 Comp->CompleteParallelAnimationEvaluation
2. 不支持多线程并发，在 GameThread 上执行

---
# References
[Unreal 骨骼动画源码剖析 :: 简易现代魔法 — Zhirui Li's Blog](https://zhiruili.github.io/posts/unreal-skeletal-animation-source-code/)