---
name: delegate-flow
description: Use when delegating coding work to subagents, coordinating strong and weak model development, optimizing model cost, or handing implementation from a lead model to a worker model.
---

# Delegate Flow

## Core Principle

Use the strongest available model for high-impact decisions; reserve lower-cost or less capable models only for bounded, verifiable, reversible execution.

Strong model: judgment. Worker model: implementation. Strong model: acceptance review.

## When to Use

Use this skill when:

- A stronger lead agent needs to delegate implementation work to a subagent.
- Model cost matters, but maintainability still matters more.
- The task involves architectural, data structure, algorithmic, or module boundary decisions.
- The worker model is capable of coding but must not be responsible for product or architectural judgment.

Do not use for very simple single-step changes; if the coordination overhead of delegation exceeds the benefit, complete the task directly.

## Required Process

1. The strong model first understands project context, including relevant files, existing patterns, constraints, and recent changes.
2. The strong model clarifies requirements and success criteria.
3. The strong model judges whether the task scope is too large; if it involves multiple independent subsystems, split it with the user first.
4. The strong model proposes 2–3 viable options, explains the trade-offs, and provides a recommendation.
5. The user confirms the recommended direction; if the user disagrees, the strong model continues discussing options and does not write the design file.
6. The strong model writes the complete design into `docs/delegate-flow/specs/YYYY-MM-DD-<topic>-design.md`.
7. In the conversation, the strong model provides only a design summary, file path, and review request — it must not reproduce the full design inline.
8. The user reviews and approves the design; if the user disagrees, the strong model revises the design file first.
9. The strong model writes the complete short implementation plan or implementation handoff into `docs/delegate-flow/plans/YYYY-MM-DD-<topic>-handoff.md`.
10. The strong model confirms that the handoff specifies a worker model; if not, it first asks the user based on the current list of available models; if the list of available models cannot be obtained, the strong model must ask the user for candidate worker models — it must not fabricate any — and records the final choice in the handoff.
11. The strong model self-reviews the handoff, confirming that scope, verification commands, stop conditions, acceptance criteria, and worker model designation are complete and unambiguous.
12. In the conversation, the strong model provides only a handoff summary, file path, and review request — it must not reproduce the full handoff inline.
13. The user reviews the key implementation boundaries; if any boundary is unclear, the strong model revises the handoff file first.
14. The worker model implements only within the handoff boundaries.
15. The worker model reports status, changed files, verification results, failures, and residual risks.
16. The strong model independently reviews the actual changes, verification evidence, architectural fit, and scope control.
17. If rework is needed, the strong model sends bounded follow-up instructions and does not let the worker model redesign.

## User Review Gates

Discuss requirements first, then have the user review the design and implementation boundaries.

This skill does not require a lengthy formal plan every time, but three lightweight gates must be preserved:

1. **Direction confirmation**: before writing the design file, the user confirms the recommended option and key trade-offs.
2. **Design confirmation**: the user reviews the design file, confirming the goal, non-goals, option trade-offs, and key design constraints.
3. **Implementation boundary confirmation**: the user reviews the handoff file, confirming scope, verification commands, stop conditions, and acceptance criteria.

If the task is very small, the design and handoff can be very brief, but they must still be saved as separate files and confirmed by the user before delegating to the worker model.

The complete review object is the file. The conversation provides only a summary, path, and confirmation request; avoid duplicating full content in both chat and file.

Do not interpret "the user already agreed on the general direction" as "the user approved all implementation details." The implementation path affects maintainability, so the user must at minimum review the design file and the handoff file.

## Pre-Design Discussion

Before writing a design file, the strong model must complete requirements and option discussion:

- Understand project context before proposing design directions.
- Ask only one key question at a time; do not mix multiple decisions together.
- Clarify the goal, non-goals, constraints, and success criteria.
- Judge whether the task is too large; if it involves multiple independent subsystems, split it into smaller design tasks first.
- Propose 2–3 viable options and explain the trade-offs of each.
- Give a recommended option and explain why.
- Wait for the user to confirm the recommended direction before writing the design file.

## Persistence Rules

Conversation is for communication; files are for authorization and tracking. Write complete content to files; chat provides only summaries, file paths, and confirmations.

Every task entering delegated implementation must have two saved files:

- `docs/delegate-flow/specs/YYYY-MM-DD-<topic>-design.md`: stores the user-approved design.
- `docs/delegate-flow/plans/YYYY-MM-DD-<topic>-handoff.md`: stores the user-approved worker model handoff.

No design file: do not proceed to implementation. No handoff file: do not dispatch the worker model. No worker model designation: do not dispatch the worker model.

These two files record only the authorization basis: what the user approved and what the worker model is authorized to do. Worker model reports and strong model reviews may remain in the conversation; they are not required to be persisted under this skill.

## Strong Model Responsibilities

The strong model is responsible for:

- Exploring project context and existing patterns
- Requirements and non-goals
- Scope judgment and task decomposition
- Option comparison and recommendation
- Architecture and module boundaries
- Data structures and algorithms
- Public interfaces and persistence formats
- Risk assessment
- Test strategy
- Handoff self-review
- Confirming worker model designation in the handoff
- Saving and maintaining the design file and handoff file
- Final review and acceptance

The strong model must not delegate ambiguous design work to a less capable worker model.

## Worker Model Designation

The worker model must be determined before dispatch.

- The handoff must state the final worker model to be used.
- If the handoff does not specify a worker model, the strong model must first ask the user based on the platform's current list of available models — it must not guess.
- If the current environment does not expose a list of available models, the strong model must ask the user for candidate worker models — it must not fabricate a model list.
- If the model specified by the user is not available, the strong model must say so in the conversation and have the user re-select from available models.
- The model actually used when dispatching the subagent must match the worker model recorded in the handoff.
- If the current platform does not support specifying the worker model in the handoff, the strong model must stop and ask the user — it must not switch to a different model.
- The model selection process remains in the conversation; the handoff records only the final worker model.

The handoff only needs to record:

```text
Worker model:
```

## Handoff Self-Review

Before sending the short implementation plan or handoff to the user for review, the strong model checks:

- Are there any vague instructions such as "handle appropriately," "add more tests," or "optimize"?
- Are in-scope and out-of-scope files or modules clearly defined?
- Are the design constraints sufficient so the worker model does not need to redesign?
- Is the worker model clearly designated?
- Are verification commands specific and do they state expected results?
- Do the stop conditions cover architecture, data model, permissions, security, migrations, and large-scale refactoring?
- Can the acceptance criteria be objectively evaluated?
- Should multi-task work be split into independent smaller tasks rather than handed to the same worker model at once?

## Worker Model Boundaries

The worker model may:

- Implement the already-determined plan
- Add focused tests required by the handoff
- Follow existing local code patterns
- Fix lint, type, and formatting issues introduced by this change
- Report blockers clearly

The worker model must report using one of the following statuses:

- `DONE`: task complete, verification has been run, ready for strong model review.
- `DONE_WITH_CONCERNS`: task complete, but concerns or residual risks exist; the strong model must read the concerns before deciding on next steps.
- `NEEDS_CONTEXT`: required context is missing; the strong model must provide it before redispatching.
- `BLOCKED`: the task cannot proceed; the strong model determines whether to add context, escalate to a stronger model, break into smaller tasks, or return to user confirmation.

The worker model must stop and report — not decide unilaterally — when:

- The plan conflicts with existing code
- Judgment is required about public APIs, data models, migrations, security rules, or permission models
- Tests fail for reasons outside the assigned scope
- Implementation requires large-scale refactoring
- The worker model wants to introduce new architecture, new dependencies, or new abstractions

## Implementation Handoff

Before dispatching the worker model, provide:

```text
Objective:
Non-goals:
User-approved design:
Worker model:
In-scope files or modules:
Out-of-scope files or modules:
Design constraints:
Implementation steps:
Verification commands:
Acceptance criteria:
Stop and report conditions:
Final report format:
```

The handoff can be brief, but every boundary must be unambiguous. Unless the user explicitly requests skipping, the strong model should let the user review this handoff before dispatching the worker model.

## Handling Worker Model Statuses

After the strong model receives the worker model's report:

- `DONE`: proceed to independent review.
- `DONE_WITH_CONCERNS`: first determine whether the concerns affect correctness, scope, or maintainability; if they do, address the concerns before reviewing.
- `NEEDS_CONTEXT`: provide the missing context and redispatch the same task.
- `BLOCKED`: do not force the same model to retry unchanged; based on the cause, choose to add context, escalate to a stronger model, break into smaller tasks, or ask the user to reconfirm direction.

## Review Checklist

After the worker model reports, the strong model checks:

- Does the implementation conform to the approved design?
- Did the worker model stay within the requested scope?
- Did the strong model look at the actual diff, rather than just trusting the worker model's summary?
- Does the verification output provide evidence supporting the completion claim?
- Are the data structures, algorithms, and public interfaces still maintainable?
- Were the requested verification commands actually run?
- Do the tests verify behavior rather than only implementation details?
- Are there unresolved migration, permission, security, compatibility, or edge-case risks?
- Is rework suitable for continued delegation, or should the strong model take over?

## Common Mistakes

| Mistake | Correction |
| --- | --- |
| "The worker model can fill in the details." | If details affect architecture, the strong model must decide first. |
| "I already know what to do; just write the design." | Requirements and option discussion must be completed, and the user must confirm the recommended direction, before writing the design file. |
| "Giving one option is fastest." | The strong model's value lies in trade-offs; it should generally provide 2–3 options with reasoning. |
| "Review will catch all problems." | Handoffs prevent drift first; review is then cheaper. |
| "Coding is low-risk work." | Core state, algorithms, permissions, and migrations are design work even when expressed as code. |
| "A vague prompt saves time." | Vague delegation transfers decision-making to the weakest link in the pipeline. |
| "The user approved the design, so dispatch directly." | Design approval is not implementation boundary approval; at minimum show the user the short plan or handoff. |
| "It was confirmed in chat; no need to save." | Design and handoff files must be saved before entering delegated implementation; files are the authorization record. |
| "It's in the file; paste it fully in chat too." | Complete content goes in files only; chat gets only the summary, path, and review request. |
| "No worker model specified, but dispatch anyway." | Do not dispatch without a worker model designation; ask the user first and record it in the handoff. |
| "The worker model said it's done." | The strong model must independently inspect the actual changes and verification evidence. |
| "The worker model is stuck; try again." | Without adding context, switching models, or breaking up the task, retrying usually just repeats the failure. |

## Additional Reference

For reusable prompts and handoff templates, see [examples.md](examples.md).
