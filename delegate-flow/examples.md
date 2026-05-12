# Delegate Flow Examples

## Requirements Clarification Questions

Before writing a design file, the strong model asks only one key question at a time:

```text
Before writing the design file, I need to confirm one key point:

[One specific question]

Options:
A. [Option A]
B. [Option B]
C. [Option C]

Which do you prefer? If none of these fits, you can also describe your thinking directly.
```

## Option Comparison and Direction Confirmation

Once requirements and context are reasonably clear, the strong model presents options first — it does not write the design file directly:

```text
Based on the current requirements and project context, I see 3 viable options:

Recommended: A. [Option name]
- Approach: [One sentence]
- Advantage: [One sentence]
- Cost: [One sentence]
- Why recommended: [One sentence]

Alternative: B. [Option name]
- Approach: [One sentence]
- Advantage: [One sentence]
- Cost: [One sentence]

Alternative: C. [Option name]
- Approach: [One sentence]
- Advantage: [One sentence]
- Cost: [One sentence]

My recommendation is A. Once you confirm this direction, I will write the design file.
```

## Splitting an Oversized Task

If the requirements span multiple independent subsystems, split first — do not write a large design directly:

```text
This requirement contains multiple parts that can be designed and implemented independently:

1. [Subtask A]
2. [Subtask B]
3. [Subtask C]

Suggested starting point: [Recommended first subtask]

Reasons:
- [Reason 1]
- [Reason 2]

Once you confirm, I will write the design file for this subtask first.
```

## Design File Template

Save to `docs/delegate-flow/specs/YYYY-MM-DD-<topic>-design.md`:

```markdown
# [Task Name] Design

## Requirements and Success Criteria

[The problem the user needs to solve, and the success criteria by which completion can be judged.]

## Non-Goals

- [What is explicitly out of scope.]

## Project Context

- [Relevant files, existing patterns, or constraints.]
- [Recent changes or known risks that affect the design.]

## Design Approach

- [The core approach.]
- [Key data structure, algorithm, module boundary, or interface decisions.]

## Key Trade-offs

- [Why this approach was chosen.]
- [Why other approaches were not chosen.]
```

## Handoff File Template

Save to `docs/delegate-flow/plans/YYYY-MM-DD-<topic>-handoff.md`:

```markdown
# [Task Name] Implementation Handoff

## Authorization Basis

- Design file: `docs/delegate-flow/specs/YYYY-MM-DD-<topic>-design.md`

## Model Designation

- Worker model: [Model name]

## Objective

[Describe the intended outcome in one sentence.]

## Non-Goals

- [What must not be modified.]
- [Which features or refactors are out of scope.]

## Scope

In scope:
- [Path or module]

Out of scope:
- [Path or module]

## Design Constraints

- [Architecture, API, data structures, or algorithms already decided by the strong model.]
- Follow existing local patterns.
- Do not introduce new dependencies unless explicitly listed here.

## Implementation Steps

1. [Specific step]
2. [Specific step]
3. [Specific step]

## Verification Commands

- [Exact command]

## Acceptance Criteria

- [Observable behavior]
- [Test or lint expectations]

## Stop and Report Conditions

- The plan conflicts with existing code.
- Judgment is required about public APIs, data models, permission rules, migrations, or security decisions.
- Tests fail for reasons outside the assigned scope.
- Large-scale refactoring or new abstractions are required.

## Worker Model Final Report Format

- Status: DONE / DONE_WITH_CONCERNS / NEEDS_CONTEXT / BLOCKED
- Changed files:
- Key implementation:
- Verification run:
- Failed or skipped verification:
- Risks or follow-up items:
```

## Design File Review Request

After writing the design file, send only a summary and path in chat:

```text
I have written the design file:
`docs/delegate-flow/specs/YYYY-MM-DD-<topic>-design.md`

Summary:
- Objective: [One sentence]
- Approach: [One sentence]
- Key trade-offs: [One sentence]
- Non-goals: [One sentence]
- Primary risks: [One sentence]

Please review the file. Once confirmed, I will write the implementation handoff.
```

## Handoff Review Request

After writing the handoff file, send only a summary and path in chat:

```text
I have written the implementation handoff:
`docs/delegate-flow/plans/YYYY-MM-DD-<topic>-handoff.md`

Summary:
- Authorized design: [design file path]
- Worker model: [Model name]
- In scope: [One sentence]
- Out of scope: [One sentence]
- Verification commands: [One sentence]
- Stop conditions: [One sentence]

Please review the file. Once confirmed, I will dispatch the worker model for implementation.
```

## Worker Model Dispatch Prompt

Use this when dispatching the worker model after the user has confirmed the design and implementation boundaries:

```text
You are the worker model responsible for bounded implementation tasks.

The sub-agent model used when dispatching this task must match the "Worker model" specified below.

Objective:
[Describe the intended outcome in one sentence.]

Non-goals:
- [What must not be modified.]
- [Which features or refactors are out of scope.]

Worker model:
[Model name]

User-approved design:
- [Design summary]
- [Key trade-offs]

In-scope files or modules:
- [Path or module]

Out-of-scope files or modules:
- [Path or module]

Design constraints:
- [Architecture, API, data structures, or algorithms already decided by the strong model.]
- Follow existing local patterns.
- Do not introduce new dependencies unless explicitly listed here.

Implementation steps:
1. [Specific step]
2. [Specific step]
3. [Specific step]

Verification commands:
- [Exact command]

Acceptance criteria:
- [Observable behavior]
- [Test or lint expectations]

Stop and report when:
- The plan conflicts with existing code.
- Judgment is required about public APIs, data models, permission rules, migrations, or security decisions.
- Tests fail for reasons outside the assigned scope.
- Large-scale refactoring or new abstractions are required.

Final report format:
- Status: DONE / DONE_WITH_CONCERNS / NEEDS_CONTEXT / BLOCKED
- Changed files:
- Key implementation:
- Verification run:
- Failed or skipped verification:
- Risks or follow-up items:
```

## Worker Model Constraint Prompt

Add this to the worker model task when implementation must strictly limit scope:

```text
Adhere strictly to the handoff. Do not expand scope, do not introduce new architecture, do not rewrite unrelated code, do not make product decisions.

If the plan is incomplete or conflicts with the codebase, stop and report the blocker — do not guess.

Your role is to implement, not to redesign.
```

## Rework Prompt

Use when the worker model's first result needs correction:

```text
Continue from your previous work, but address only the following review findings:

Findings:
1. [Specific issue]
2. [Specific issue]

Keep the original scope unchanged. Do not refactor unrelated code. If fixing these issues requires a design change, stop and explain why.

Report only:
- What changed compared to the previous version
- Verification run
- Remaining risks
```

## Strong Model Review Checklist

Use after the worker model reports completion:

```text
Review focus:
- What is the worker model status? DONE / DONE_WITH_CONCERNS / NEEDS_CONTEXT / BLOCKED
- Does the implementation conform to the approved design?
- Did the worker model stay within the requested scope?
- Did you look at the actual diff, rather than just trusting the worker model's summary?
- Does the verification output provide evidence supporting the completion claim?
- Are public interfaces, data structures, and algorithms maintainable?
- Were the requested verification commands run and did they pass?
- Are the tests behavior-focused and meaningful?
- Are there unhandled edge cases, migration, permission, security, or compatibility risks?
- Is rework suitable for continued delegation, or should the strong model take over?
```

## Worker Model Status Handling

Use after receiving the worker model's report:

```text
Status handling:
- DONE: proceed to strong model independent review.
- DONE_WITH_CONCERNS: read the concerns first; if they affect correctness, scope, or maintainability, address them before reviewing.
- NEEDS_CONTEXT: provide the missing context and redispatch the same task.
- BLOCKED: do not retry unchanged; choose to add context, escalate to a stronger model, break into smaller tasks, or return to user confirmation.
```

## Model Division of Responsibility Example

```text
Strong model: GPT-5.5
Worker model: Sonnet 4.6

Strong model owns:
- Requirements
- Architecture
- Data structures
- Algorithms
- Handoff
- Review

Worker model owns:
- Bounded implementation
- Focused tests
- Local lint or type fixes
- Implementation reporting
```
