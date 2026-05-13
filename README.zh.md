# delegate-flow

> [obra/superpowers: An agentic skills framework & software development methodology that works.](https://github.com/obra/superpowers)

微调superpowers的skill，用于：
1. 调整为适合自己的工作流
2. 强制使用subagent工作流以实现强模型和弱模型分离
3. 验证能否减少token成本

## 强模型和弱模型分离

> 决策风险 vs 执行确定性

> 强模型做不可逆决策，弱模型做可验证执行

强模型负责高影响决策 -> 弱模型负责边界清晰、可验证、可回滚的实现 -> 强模型审核结果
