202407041545
Status: #idea
Tags: [[Animation 动画]]
# Anim Update 动画蓝图更新逻辑

> [!WARNING] Warning
> 本篇 Idea 中所有 this 指 SkeletalMeshComponent，没有特别说明的函数都是 SkeletalMeshComponent 的成员函数

只关注生命周期相关的函数，关于事件触发、曲线计算等逻辑，需要各种 bool 联合判断计算时机，暂时按下不表
# SkeletalMeshComponent 视角下的生命周期
# Step 0. RefreshBoneTransforms
SkinnedMeshComponent 每次 TickComponent 的时候都会调用 SkinnedMeshComponent->RefreshBoneTransforms

SkeletalMeshComponent 重载了 RefreshBoneTransforms 函数，其中连着先调用了 **TickAnimation** 后调用了 **DispatchParallelEvaluationTasks**

有些逻辑会先调用 TickAnimation 再调用 RefreshBoneTransforms，这里用 if 语句做了代码隔离，避免出现重复 TickAnimation
# Step 1. TickAnimation
1. 调用所有 AnimInstance 的 AnimInstance->**UpdateAnimation**
2. 获取每个 AnimInstance 在 GameThread 上的 Proxy
3. 依次调用 AnimInstance 的成员函数 PreUpdateAnimation、UpdateMontage、UpdateMontageSyncGroup、UpdateMontageEvaluationData、NativeUpdateAnimation（专门留出的空函数接口用来更新自定义数据）、NativeUpdateAnimation_WorkerThread、BlueprintUpdateAnimation，基本上都是转发给 Proxy 的同名方法
4. 如果设置了非并行 Evaluate， AnimInstance->UpdateAnimation 最后就会直接调用 **ParallelUpdateAnimation** 和 **PostUpdateAnimation** 而跳过后续的并行任务派发
# Step 2. DispatchParallelEvaluationTasks
如果设置了并行 Evaluate，SkeletalMeshComponent 会在这里创建两个 Task 并派发，先派遣了 FParallelAnimationEvaluationTask，然后设置FParallelAnimationCompletionTask 的开始条件为前者完成

由 SkeletalMeshComponent 来控制调用 **ParallelUpdateAnimation** 和 **PostUpdateAnimation** 的时机，而不是直接由 AnimInstance 顺序调用
## FParallelAnimationEvaluationTask
1. 转发到 this->ParallelAnimationEvaluation
2. 转发到 this->PerformAnimationProcessing
3. 调用所有 AnimInstance 的  AnimInstance->**ParallelUpdateAnimation**
4. 调用 this->EvaluateAnimation，根据 AnimInstance 计算结果计算纯动画数据并填充 BoneSpaceTransforms
5. 上述流程可以跑在不同的工作线程上
6. FParallelAnimationCompletionTask 的前置条件
## FParallelAnimationCompletionTask
1. 转发到 this->CompleteParallelAnimationEvaluation
2. 再转发到 this->PostAnimEvaluation(EvaluationContext)
3. 调用所有 AnimInstance 的 AnimInstance->**PostUpdateAnimation**
4. 不支持多线程并发，在 GameThread 上执行

# AnimInstance 视角下的生命周期
UpdateAnimation->ParallelUpdateAnimation->PostUpdateAnimation

---
# References
[Unreal 骨骼动画源码剖析 :: 简易现代魔法 — Zhirui Li's Blog](https://zhiruili.github.io/posts/unreal-skeletal-animation-source-code/)