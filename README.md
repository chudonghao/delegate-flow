# delegate-flow

English | [中文](README.zh.md)

> [obra/superpowers: An agentic skills framework & software development methodology that works.](https://github.com/obra/superpowers)

A fine-tuned fork of the superpowers skill, designed to:
1. Adapt the workflow to personal preferences
2. Enforce a subagent workflow that separates strong models from weak models
3. Validate whether token costs can be reduced

## Separation of Strong and Weak Models

> Decision risk vs. execution certainty

> Strong models make irreversible decisions; weak models handle verifiable execution

Strong model handles high-impact decisions → Weak model handles well-scoped, verifiable, and rollback-safe implementation → Strong model reviews the result
