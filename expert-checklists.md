# Expert Review Checklists

Detailed question templates from each expert lens for spec and code reviews.

## Karl Wiegers — Requirements Engineering

**Core question:** "Is every requirement measurable and testable?"

Checklist:
- Does each requirement have a unique identifier?
- Is every requirement verifiable (can you write a test for it)?
- Are acceptance criteria explicit and measurable?
- Is the priority and rationale stated?
- Are ambiguities flagged (words like "fast", "user-friendly", "robust")?
- Are business rules separated from functional requirements?
- Is the scope boundary clear (what's NOT included)?

Red flags:
- "Handle gracefully" → Replace with specific behavior
- "Should be fast" → Replace with measurable SLA
- "Etc." or "and so on" → Incomplete enumeration

## Gojko Adzic — Specification by Example

**Core question:** "Can you provide Given/When/Then examples?"

Checklist:
- Does each rule have at least one concrete example?
- Are examples distinguishable (not all happy path)?
- Do examples cover edge cases and boundary conditions?
- Can a developer implement from examples alone?
- Are examples validated with stakeholders?

Template:
```
Given [precondition]
When [action]
Then [expected outcome]
```

## Alistair Cockburn — Use Cases

**Core question:** "Who is the primary actor and what's their goal?"

Checklist:
- Is the primary actor identified?
- Is the goal stated from the actor's perspective?
- Are there extension/alternative scenarios?
- Is the minimum guarantee defined (what happens on failure)?
- Is the trigger clear (what initiates this use case)?

## Martin Fowler — Architecture & Design

**Core question:** "Does this violate single responsibility or introduce unnecessary coupling?"

Checklist:
- Does each module/class have one reason to change?
- Are dependencies pointing in the right direction?
- Is there unnecessary indirection?
- Could this be simpler without losing essential behavior?
- Are interfaces minimal and cohesive?
- Is the code testable in isolation?

Red flags:
- God objects / god classes
- Circular dependencies
- Passing data through layers that don't use it
- Premature generalization ("we might need this later")

## Michael Nygard — Production Systems

**Core question:** "What happens when this fails?"

Checklist:
- What are the failure modes for each component?
- Is there a timeout for every external call?
- Is there a circuit breaker or fallback?
- What's the blast radius of a failure?
- Can the system degrade gracefully?
- Are there monitoring and alerting for failure states?
- What's the rollback plan?

Template for failure analysis:
```
Component: [name]
Failure mode: [how it fails]
Detection: [how you know it failed]
Recovery: [automatic or manual steps]
Blast radius: [what else breaks]
```

## Sam Newman — Microservices & Boundaries

**Core question:** "How does this handle backward compatibility?"

Checklist:
- Are API contracts versioned?
- Can this be deployed independently?
- Is the bounded context clear?
- Are there anti-corruption layers at boundaries?
- Is data ownership clear (who owns what data)?
- Can old and new versions coexist during migration?

## Gregor Hohpe — Enterprise Integration

**Core question:** "What's the message exchange pattern?"

Checklist:
- Is the integration synchronous or asynchronous?
- What happens when the downstream system is slow or down?
- Are messages idempotent?
- Is there dead-letter handling?
- Are there ordering guarantees?
- Is the data format contract explicit (schema, version)?

## Lisa Crispin — Agile Testing

**Core question:** "How would QA validate this?"

Checklist:
- Are there unit tests for business logic?
- Are there integration tests for boundaries?
- Are there acceptance tests for user-facing behavior?
- Are edge cases and error paths tested?
- Is test data realistic?
- Can tests run independently and in parallel?

## Janet Gregory — Collaborative Testing

**Core question:** "Did the whole team participate in test design?"

Checklist:
- Were examples discussed with developers AND business?
- Are test scenarios derived from real user workflows?
- Is there exploratory testing beyond scripted tests?
- Are non-functional requirements (performance, security) tested?

## Kelsey Hightower — Cloud Native

**Core question:** "How does this handle cloud deployment?"

Checklist:
- Is configuration externalized (12-factor)?
- Can this scale horizontally?
- Are secrets managed properly?
- Is health checking implemented?
- Can this run in any environment (dev, staging, prod)?
- Is observability built in (logs, metrics, traces)?

## Using These Checklists

### For Spec Review
1. Pick 2-3 relevant experts based on the domain
2. Run through their checklist questions
3. Document gaps as spec issues with priority
4. Don't proceed to implementation until critical gaps are resolved

### For Code Review
1. Always include Fowler (architecture) and Nygard (production)
2. Add domain-specific experts as needed
3. Focus on: coupling, failure modes, testability, simplicity
4. Reject: "I improved X while I was at it" — only requested changes

### For Architecture Review
1. Fowler + Newman + Nygard (minimum)
2. Add Hohpe if integration is involved
3. Add Hightower if cloud/deployment is involved
4. Focus on: boundaries, failure isolation, independent deployability