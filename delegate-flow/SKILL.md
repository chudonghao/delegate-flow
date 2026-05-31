---
name: delegate-flow
description: Use this skill to communicate requirements with the user, gradually reduce decision risk, delegate implementation to a subagent, and review the implementation result.
---

# delegate-flow

Use this skill to communicate requirements with the user, gradually reduce decision risk, delegate implementation to a subagent, and review the implementation result.

**Core Goal**

Reduce decision risk and implementation cost.

**Hard Requirements**

Except for the "Subagent Implementation" step, do not modify project implementation code, generate project implementation, run implementation commands, or execute actual changes. This rule applies to all projects, no matter how simple they appear.

**Do Not Skip the Main Process Because the Requirement Is Simple**

Even if the requirement is simple, follow the process. Gate rules must be observed.

## Core Concepts

### Decision Risk

Decision risk comes from factors that are unconfirmed before implementation, will affect result quality during implementation, and will cause rework or maintenance cost if decided incorrectly.

Common decision risk factors include:

- **Requirement Risk**: Goals, non-goals, success criteria, business boundaries, and similar items are unclear, causing the implementation direction to be wrong.
- **Engineering Risk**: Naming style, organization, logic style, existing patterns, dependency boundaries, and similar items are unclear, causing the implementation to break project consistency and maintainability.
- **Correctness Risk**: Data structures, interface boundaries, module responsibilities, state transitions, error handling, and similar items are unclear, causing the code to appear complete while its behavior is unreliable.
- **Handoff Risk**: Implementation scope, stop conditions, reporting requirements, subagent model, and similar items are unclear, causing the subagent to expand scope, miss verification, or be unable to determine completion.

The implementation target file is used to converge these risks chapter by chapter: the Requirements chapter converges requirement risk, the Refinement chapter converges engineering risk and correctness risk, and the Handoff chapter converges handoff risk.

## Task List

You **must** create the following tasks and complete them in order:

1. **Explore Project Context**: Check relevant files, documentation, and recent commits
2. **Confirm Requirements**: Communicate requirements with the user (clarify goals and non-goals)
3. **Generate Implementation Target File**: Save the requirements to `docs/delegate-flow/target/YYYY-MM-DD-<topic>.md`
4. **Review Requirements**: Ask the user to review the Requirements chapter until the **user approves**
5. **Refine Implementation Target**: See the detailed rules below for this step
6. **Review Refinement**: Ask the user to review the complete Refinement chapter until the **user approves**
7. **Add Handoff Chapter**
8. **Self-Review Implementation Target File**: Quickly check in place for placeholders, contradictions, ambiguous wording, and business scope
9. **Subagent Implementation**: Dispatch the implementation target file to a subagent for implementation and wait for completion
10. **Review Implementation Result**: Review the implementation result and provide a brief report

### Basic Requirements

All task lists must be inserted at once.

### Subagent Implementation

**Hard Requirements**

- Use the model specified by the user for the subagent
- If no subagent model is specified, get the available model list from the platform and agree with the user
- If the platform cannot get the available model list
  - Do not fabricate a model list
  - Do not proceed with subsequent actions without authorization
  - Discuss the next action with the user

### Refine Implementation Target

Refining the implementation target is used to gradually converge engineering risk and correctness risk.

Refining the implementation target is a multi-round iterative process.

Insert the following subtasks all at once in each round:

1. **Propose Refinement**: Based on the requirements, context, and priority, propose 1 question or direction worth refining and give a recommendation
2. **Decide Refinement**: Communicate the refinement with the user until the user explicitly chooses: **approve refinement** or **reject refinement**
3. **Update Implementation Target File**:
   - If the user chooses **approve refinement**: write the confirmed refinement content into the implementation target file
4. **Decide Next Step**:
   - If the user chooses **approve refinement**: enter the next round of refinement
   - If the user chooses **reject refinement**: ask the user to choose whether to continue **proposing refinement** or **end refinement**

## Gate Rules

Steps in the task list that contain **user approves**, **approve refinement**, **reject refinement**, or **end refinement** are Gates. Do not proceed to the next step until the Gate has passed.

## Core Principles
- **Ask Only One Question at a Time**: Avoid causing information overload by asking multiple questions at once
- **Prefer Multiple Choice Questions**: When conditions allow, they are easier to answer than open-ended questions
- **Strictly Follow YAGNI**: Remove unnecessary content
- **Stay Flexible**: When content is logically inconsistent or questionable, backtrack and clarify in time

## Implementation Target File

The implementation target file converges risks chapter by chapter. The implementation target file constrains the implementation target and controls decision risk; it is not step-by-step implementation guidance.

The implementation target file is divided into three chapters: **Requirements**, **Refinement**, and **Handoff**.

The **Requirements** chapter is added and modified during the requirements stage, the **Refinement** chapter is added and modified during the refinement stage, and the **Handoff** chapter is added and modified during the handoff stage.

### Requirements Chapter Requirements

The Requirements chapter is a record of communication consensus. The Requirements chapter only records content confirmed with the user; do not add unconfirmed key designs, scope changes, or implementation requirements.

The Requirements chapter contains:
- **Background**: Project background and user requirements
- **Goals**: What problem this task should solve
- **Non-Goals**: What problem this task explicitly does not solve
- **Success Criteria**
- **Business Boundaries**

### Refinement Chapter Requirements

The Refinement chapter may have multiple subsections. Each subsection contains:
- **Explanation**: Refinement explanation
- **Constraints**: Restrictions and requirements produced by the refinement

### Handoff Chapter Requirements

The Handoff chapter includes:
- **Basic Instructions**: Necessary guidance for the subagent
- **Subagent Model**
- **Implementation Scope**: What belongs to this task and what must not be expanded
- **Stop Conditions**
- **Reporting Requirements**

### Counterexample

1. The document describes in detail which code to modify in which location of which file, turning the subagent into a command execution tool.
