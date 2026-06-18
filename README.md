# delegate-flow

English | [中文](README.zh.md)

A workflow for software engineering:
1. Separating specification from implementation
2. Reducing decision risk through requirement/specification alignment, including requirement risk, engineering risk, correctness risk, and execution/handoff risk
3. Verifying whether token costs can be reduced

> We assume:
> 1. The main issue between humans and large models is requirement/specification alignment
> 2. Once the specification is clear, large models can produce correct code

> Inspired by [superpowers](https://github.com/obra/superpowers)

## Separation of Specification and Implementation

> Decision risk vs. execution certainty

> The specification stage makes irreversible decisions; the implementation stage completes the goal

Specification -> Implementation -> Review implementation result

## Skills

- `delegate-flow`: Communicate requirements, gradually reduce decision risk, add a handoff chapter, delegate implementation to a subagent, and review the implementation result.
- `delegate-flow-direct`: Communicate requirements, gradually reduce decision risk, do not add a handoff chapter, do not dispatch implementation to a subagent by default, implement, and review the implementation result.
