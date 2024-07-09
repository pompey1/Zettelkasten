202407081637
Status: #idea
Tags: [[AI]]
# BehaviorTree 行为树 
传统行为树的节点类型
1. Sequence: UE 中的 Composite 节点
2. Selector: UE 中的 Composite 节点
3. Probability
4. Condition: UE 中用 Decorators 表示
5. Action: UE 中用 Task 表示 Actino 节点，因为这里复用了 GameplayTask 的相关逻辑
6. Filter: 

Composite 节点会根据子节点的执行情况 **向上** 返回

---
# References