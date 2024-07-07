202407072338
Status: #moc
Tags: [[虚幻引擎]]
# AI
虚幻中的游戏 AI 设计思路将完整的 AI 系统分为了三个部分 Sense、Think、Act
## Sense
目前 UE 对于 Sense 总体有两套方案
1. AIPerception & AIPerceptionStimuli
2. EQS 

Sense 的本质是当前 AI 状态的快照 Snapshot ，所以包括 NavMesh 生成的信息也包括在 Sense 模块中
## Think
行为树的 Composite （Sequence、Selector、Decorator）节点
## Act
行为树的 Task 节点

---
# References
[Chasing the Player - Introduction to AI with Blueprints (epicgames.com)](https://dev.epicgames.com/community/learning/courses/67R/unreal-engine-introduction-to-ai-with-blueprints/K1X/chasing-the-player)