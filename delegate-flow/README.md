# Delegate Flow

`delegate-flow` is a small Cursor skill source project for codifying the model delegation workflow.

It expresses one rule:

> The strong model owns high-impact decisions. The worker model owns bounded, verifiable, reversible implementation. The strong model reviews the result.

## Files

- `SKILL.zh.md` - Chinese source draft, for easy maintenance and iteration.
- `SKILL.md` - English formal version, for discovery and loading by Cursor and agents.
- `examples.zh.md` - Chinese prompt template source draft.
- `examples.md` - English prompt template formal version.

## Intended Use

Use this skill when you want a stronger model (e.g. GPT-5.5) to own requirements, architecture, data structures, algorithms, implementation handoff, and review, while a lower-cost or less capable model (e.g. Sonnet 4.6) handles bounded coding tasks.

This project uses a clean skill source directory structure that can later be copied to a personal or project-level skill location.

## Recommended Maintenance

Chinese files as source drafts:

```text
SKILL.zh.md
README.zh.md
examples.zh.md
```

English files as formal loaded versions:

```text
SKILL.md
README.md
examples.md
```

Recommended workflow for maintaining a bilingual version:

1. The strong model writes or modifies the Chinese source draft.
2. The worker model translates the Chinese source draft into the English formal version.
3. The strong model reviews the English version to verify no constraints were lost, no tone was weakened, and no boundaries were changed.

## Relationship to Requirements and Design Process

This skill preserves a general software engineering principle: discuss requirements first, have the user review the design, then proceed to implementation.

It does not require a lengthy formal plan every time. For most delegation tasks, the recommended lightweight flow is:

```text
Context exploration -> Requirements discussion -> Option comparison -> Direction confirmation -> Write design file -> Summary + path, request user review -> Write handoff file -> Summary + path, request user review -> Worker model implements -> Strong model independently reviews
```

The key here is not document length, but ensuring the worker model does not quietly take on design judgment during implementation. The strong model must first understand project context, clarify the goal, non-goals, constraints, and success criteria, propose candidate options, and explain trade-offs. Only after the user confirms the recommended direction does the strong model write the design file.

To avoid wasting tokens, complete content goes into files; chat provides only the summary, file path, and confirmation request. The user reviews the file — not a copy pasted inline in chat.

After the worker model finishes, the strong model must not simply trust the reported summary; it must independently inspect the actual changes and verification evidence before deciding to accept, request rework, escalate models, or return to user confirmation.

## Review File Structure

Before entering delegated implementation, two authorization files must be saved:

```text
docs/delegate-flow/
├── specs/
│   └── YYYY-MM-DD-<topic>-design.md
└── plans/
    └── YYYY-MM-DD-<topic>-handoff.md
```

- `specs/` stores the user-approved design — i.e., "what the user approved."
- `plans/` stores the worker model implementation handoff — i.e., "what the worker model is authorized to do."

No design file: do not proceed to implementation. No handoff file: do not dispatch the worker model. No worker model designation: do not dispatch the worker model.

## Install as a Personal Cursor Skill

Copy the formal version files to the personal Cursor skills directory:

```text
~/.cursor/skills/delegate-flow/
```

The installed directory must contain at least:

```text
delegate-flow/
├── SKILL.md
└── examples.md
```

You may also copy the `*.zh.md` files if you want to keep the bilingual maintenance drafts.

## Typical Workflow

1. Load `delegate-flow` when delegating a coding task to a subagent.
2. The strong model first understands project context, then clarifies requirements and success criteria.
3. The strong model judges whether the task is too large; if so, split it first.
4. The strong model proposes 2–3 options and a recommended direction.
5. The user confirms the recommended direction.
6. The strong model saves `docs/delegate-flow/specs/YYYY-MM-DD-<topic>-design.md`.
7. The strong model provides a design summary and path in chat and requests the user to review the file.
8. The strong model saves `docs/delegate-flow/plans/YYYY-MM-DD-<topic>-handoff.md` and self-reviews it first.
9. If the handoff does not specify a worker model, the strong model asks the user based on the current list of available models; if the list of available models cannot be obtained, the strong model must ask the user for candidate worker models — it must not fabricate any — and records the final choice in the handoff.
10. The strong model provides a handoff summary and path in chat and requests the user to review the file.
11. The user confirms the key implementation boundaries.
12. When the strong model dispatches the subagent, the model actually used must match the worker model recorded in the handoff.
13. If the current platform does not support specifying the worker model in the handoff, the strong model must stop and ask the user — it must not switch to a different model.
14. The worker model implements with clear boundaries and reports using a defined status.
15. The strong model independently reviews the actual changes and verification evidence before accepting.

## Recommended Model Division of Responsibility

The strong model owns:

- Requirements
- Architecture
- Data structures
- Algorithms
- Public interfaces
- Implementation handoff
- Final review

The worker model owns:

- Focused code changes
- Focused tests
- Type, lint, and formatting fixes related to this task
- Implementation reporting

Once the task requires new product or architectural judgment, the worker model must stop and report.
