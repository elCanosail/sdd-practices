---
name: sdd-practices
description: Spec-Driven Development best practices and Karpathy coding principles. Use when starting a new feature, reviewing a spec, writing code, or making changes to an existing codebase. Covers SDD workflow phases (spec → plan → implement → verify), Karpathy's 4 principles (think before coding, simplicity first, surgical changes, goal-driven execution), expert review checklists, and anti-patterns to avoid. Triggers: "spec-driven", "SDD", "spec review", "before coding", "code review", "Karpathy", "surgical", "simplicity first", "goal-driven", "spec panel".
---

# Spec-Driven Development Practices

A practical synthesis of SDD workflow and Karpathy's LLM coding principles. No framework overhead — just the thinking framework that prevents costly mistakes.

## SDD Workflow

Every non-trivial feature goes through these phases. Trivial tasks (typos, one-liners, obvious fixes) skip directly to implement.

### Phase 1: Spec → Think Before Coding

Write a brief spec answering:
- What problem does this solve?
- What are the acceptance criteria? (measurable, testable)
- What assumptions am I making?
- What are the tradeoffs?

**Stop if:** You can't state the acceptance criteria. Ask for clarification.

**Expert review checklist** (pick relevant experts):
- **Wiegers**: Does every requirement have measurable acceptance criteria?
- **Adzic**: Can you provide Given/When/Then examples?
- **Fowler**: Does this violate single responsibility or introduce unnecessary coupling?
- **Nygard**: What happens when this fails? What are the failure modes?
- **Hightower**: How does this deploy? What's the rollback plan?

### Phase 2: Plan → Simplicity First

State the implementation plan with verification checkpoints:
1. [Step] → verify: [check]
2. [Step] → verify: [check]

**The simplicity test:** Would a senior engineer say this is overcomplicated? If yes, simplify.

**Anti-patterns to reject:**
- Features beyond what was asked
- Abstractions for single-use code
- "Flexibility" or "configurability" that wasn't requested
- Error handling for impossible scenarios
- If 200 lines could be 50, rewrite it

### Phase 3: Implement → Surgical Changes

Write code. Touch only what you must.

**Rules:**
- Don't "improve" adjacent code, comments, or formatting
- Don't refactor things that aren't broken
- Match existing style, even if you'd do it differently
- Remove imports/variables/functions YOUR changes made unused
- Don't remove pre-existing dead code unless asked
- Every changed line must trace directly to the user's request

### Phase 4: Verify → Goal-Driven Execution

Run the verification checkpoints from Phase 2. Loop until all pass.

**Transform imperative to verifiable:**
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

## Decision Tree

```
Is this a task (build/fix/change something)?
  NO — it's a problem (something is wrong, unclear, or strategic)
    → Use CPS skill first (complex-problem-solving)
    → Return here once the solution is defined
  YES →
    Is the task trivial (typo, one-liner)?
      YES → Just do it. Skip phases.
      NO →
        Is the spec clear with measurable acceptance criteria?
          YES → Phase 2 (Plan)
          NO → Phase 1 (Spec) — ask questions, don't assume
```

**CPS → SDD handoff:** CPS Phases 0-5 define *what* to build and *why*. SDD Phases 1-4 define *how* to build it.
The output of CPS Phase 5 (Rumelt's strategy kernel) becomes the input for SDD Phase 1 (Spec).

For full CPS methodology, see the `complex-problem-solving` skill.
For creative iteration (when you have a rough idea and need to feel your way to clarity), see the `taste-loop` skill.

**The natural flow:** CPS defines the problem → Taste Loop discovers the solution shape → SDD builds it.

## Anti-Patterns

| Anti-pattern | Correct approach |
|---|---|
| Picking an interpretation silently | State assumptions explicitly, present alternatives |
| Hiding confusion | Name what's unclear, ask for clarification |
| Overcomplicating a simple task | Minimum code that solves the problem |
| Touching adjacent code "while you're at it" | Only touch what the request requires |
| Vague success criteria ("make it work") | Define measurable, testable outcomes |
| Adding "nice to have" features | Only what was requested, nothing speculative |
| Refactoring unrelated code | Mention it, don't change it |
| Building abstractions for future use | Build abstractions when you have 3+ use cases |

## When to Use Each Expert Lens

- **Requirements/spec review** → Wiegers + Adzic (measurability, examples)
- **Architecture/design review** → Fowler + Newman (coupling, boundaries, compatibility)
- **Production readiness** → Nygard + Hightower (failure modes, deployment)
- **Testing strategy** → Crispin + Gregory (coverage, edge cases, team participation)

For detailed expert profiles and question templates, see [references/expert-checklists.md](references/expert-checklists.md).

## Git Integration

Each SDD phase maps to a Git action:

| SDD Phase | Git Action |
|---|---|
| Phase 1: Spec | Create branch `feat/feature-name` |
| Phase 2: Plan | Commit spec doc: `docs(feature): spec for X` |
| Phase 3: Implement | Atomic commits per logical step |
| Phase 4: Verify | Tests pass → PR → merge → delete branch |

**Commit format:** `type(scope): description` (Conventional Commits)

For full Git workflow, branching strategy, PR templates, and anti-patterns, see [references/git-practices.md](references/git-practices.md).

## Integration with AGENTS.md

This skill reinforces the Coding Principles section in AGENTS.md. When both are loaded:
- AGENTS.md provides the standing rules
- This skill provides the SDD workflow and expert lenses
- In case of conflict, AGENTS.md rules take precedence (they're project-specific)