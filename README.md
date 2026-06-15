# Claude Node Projects — Workflow Kit

A set of Claude Code commands, skills, and rules that codify an opinionated, spec-driven development workflow for Node/TypeScript projects: write a spec, break it into small tasks, then implement each task with TDD, one verifiable step at a time.

## Main development flow

```
/spec   ──→  /plan  ──→  /build (repeated per task)
SPECIFY      TASKS       IMPLEMENT (TDD, one task at a time)
```

1. **`/spec`** — Define what you're building before writing code.
   - Asks clarifying questions (objective, users, features/acceptance criteria, tech stack, boundaries).
   - Produces `SPEC.md` covering: objective, commands, project structure, code style, testing strategy, boundaries.
   - Use for new projects/features or whenever requirements are unclear. Skip for trivial, unambiguous changes.

2. **`/plan`** — Turn the spec into an ordered, verifiable task list.
   - Enters plan mode (read-only) to map the codebase and dependencies between components.
   - Slices work vertically (one full path per task, not horizontal layers).
   - Writes `tasks/plan.md` (the plan, with checkpoints) and `tasks/todo.md` (the task list with acceptance criteria).
   - Presents the plan for review before any code is touched.

3. **`/build`** — Implement the plan, one task at a time.
   - For each task: write a failing test (RED) → implement minimal code (GREEN) → run full test suite → run build → commit → mark task complete → move on.
   - If a step fails, falls back to the debugging-and-error-recovery skill.

4. **`/test`** — TDD workflow, usable standalone for features or bug fixes.
   - New features: write failing tests → implement → refactor while green.
   - Bug fixes: **Prove-It pattern** — write a test that reproduces the bug, confirm it fails, fix it, confirm it passes, then run the full suite.

## Skills

Skills are loaded automatically by the commands above, or invoked directly when their description matches the task at hand.

| Skill | Purpose |
|---|---|
| `spec-driven-development` | The SPECIFY → PLAN → TASKS → IMPLEMENT gated workflow behind `/spec`. Each phase needs human validation before moving on. |
| `planning-and-task-breakdown` | Breaks a spec/requirement into small, independently verifiable tasks with acceptance criteria. Used by `/plan`. |
| `incremental-implementation` | Build in thin vertical slices: implement → test → verify → commit, repeat. Used for any multi-file change. |
| `test-driven-development` | RED/GREEN/REFACTOR cycle, plus the Prove-It pattern for bugs. Used by `/build` and `/test`. |
| `testing` | Conventions for this repo's tests: Jest, `*.test.ts`/`*.spec.ts` colocated in `src/`, per-package `jest.config.cjs`. |
| `new-module` | Steps for scaffolding a new app/package (`apps/` vs `packages/`, `package.json`, `tsconfig.json`, workspace linking). |
| `documentator` | Writes README files and ADRs — clear, concise, focused on usage and key decisions. Proposes changes before editing existing docs. |
| `explain-code` | Explains how code works using an analogy, an ASCII diagram, a step-by-step walkthrough, and a common gotcha. |

## Typical usage

```
/spec    # describe what you want to build, review SPEC.md
/plan    # review tasks/plan.md and tasks/todo.md
/build   # repeat until all tasks are done
```

For ad-hoc work outside this flow:
- `/test` for a specific feature or bug fix using TDD.
- Ask Claude to explain a part of the codebase — the `explain-code` skill kicks in automatically.
- Ask Claude to write/update a README or ADR — the `documentator` skill kicks in automatically.
