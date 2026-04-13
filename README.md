# SDD Practices

Spec-Driven Development best practices and Karpathy's LLM coding principles. A thinking framework, not a framework overhead.

## What's Inside

### SKILL.md — The Core

A practical synthesis for OpenClaw/Claude Code agents covering:

- **SDD Workflow**: Spec → Plan → Implement → Verify (4 phases)
- **Karpathy's 4 Principles**: Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution
- **Decision Tree**: When to apply full rigor vs. just do it
- **Anti-Patterns Table**: 8 common LLM mistakes and their corrections

### Expert Review Checklists

10 expert lenses for spec and code reviews:

| Expert | Lens | Core Question |
|---|---|---|
| Karl Wiegers | Requirements | "Is every requirement measurable and testable?" |
| Gojko Adzic | Specification by Example | "Can you provide Given/When/Then examples?" |
| Alistair Cockburn | Use Cases | "Who is the primary stakeholder?" |
| Martin Fowler | Architecture | "Does this violate single responsibility?" |
| Michael Nygard | Production Systems | "What happens when this fails?" |
| Sam Newman | Microservices | "How does this handle backward compatibility?" |
| Gregor Hohpe | Integration | "What's the message exchange pattern?" |
| Lisa Crispin | Agile Testing | "How would QA validate this?" |
| Janet Gregory | Collaborative Testing | "Did the whole team participate?" |
| Kelsey Hightower | Cloud Native | "How does this handle deployment?" |

## Philosophy

Inspired by [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876):

> "The models make wrong assumptions on your behalf and just run along with them without checking. They don't manage their confusion, don't seek clarifications, don't surface inconsistencies, don't present tradeoffs, don't push back when they should."

> "They really like to overcomplicate code and APIs, bloat abstractions, don't clean up dead code... implement a bloated construction over 1000 lines when 100 would do."

These guidelines bias toward **caution over speed**. For trivial tasks, use judgment — not every change needs the full rigor. The goal is reducing costly mistakes on non-trivial work, not slowing down simple tasks.

## Usage with OpenClaw

Copy or symlink `SKILL.md` to your workspace skills directory:

```bash
cp SKILL.md ~/.openclaw/workspace/skills/sdd-practices/SKILL.md
cp expert-checklists.md ~/.openclaw/workspace/skills/sdd-practices/references/expert-checklists.md
```

Or install as an OpenClaw skill:

```bash
openclaw plugins install sdd-practices
```

## Usage with Claude Code

Append to your `CLAUDE.md`:

```bash
cat SKILL.md >> CLAUDE.md
```

## License

MIT