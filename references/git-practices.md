# Git Practices

Git workflow conventions that complement SDD phases. The goal: history that explains *why*, not just *what*.

## Commits

### One Rule: Atomic Commits

One logical change per commit. A commit should:
- Pass all tests on its own
- Be revertable without breaking anything else
- Tell a complete story in isolation

```bash
# Bad: mixing concerns
git commit -m "fix login bug and refactor user model and update styles"

# Good: separate commits
git commit -m "fix(auth): handle null token in session validation"
git commit -m "refactor(user): extract address fields into ValueObject"
git commit -m "style(login): align form fields per design spec"
```

### Conventional Commits

Format: `type(scope): description`

| Type | When to use |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `test` | Adding or updating tests |
| `docs` | Documentation only |
| `style` | Formatting, whitespace (no logic change) |
| `chore` | Build process, dependency updates, tooling |
| `perf` | Performance improvement |
| `revert` | Reverts a previous commit |

**Scope** = the module, feature, or file affected. Keep it short: `auth`, `user`, `api`, `dashboard`.

**Description** = imperative mood, lowercase, no period. "add", not "added" or "adds".

```bash
feat(billing): add proration on mid-cycle plan change
fix(api): return 404 instead of 500 for missing resource
refactor(auth): extract token validation into middleware
test(user): add edge cases for empty email field
docs(api): document rate limiting headers
chore(deps): upgrade express to 4.18.2
```

### Commit Message Body (when needed)

Use the body to explain *why*, not *what* (the diff shows the what):

```
fix(payments): retry on transient Stripe errors

Stripe occasionally returns 503 during high load. Previously we surfaced
this as a payment failure to the user. Now we retry up to 3 times with
exponential backoff (1s, 2s, 4s) before failing.

Closes #142
```

Add body when:
- The change is non-obvious
- There's important context (why this approach, not others)
- Referencing an issue or PR

---

## Branching

### Strategy: Trunk-Based (recommended for small teams)

Single main branch (`main`). Short-lived feature branches, merged daily if possible.

```
main ──────────────────────────────────────────────►
       └── feat/login-2fa (max 1-2 days)
              └── merged, deleted
```

**Rules:**
- Branch from `main`, merge to `main`
- Branches live max 1-3 days (if longer, use feature flags)
- Delete branch after merge
- Never commit directly to `main` (use PRs or at minimum a quick review)

### Branch Naming

Format: `type/short-description`

```bash
feat/user-2fa
fix/null-token-crash
refactor/extract-payment-service
docs/api-rate-limits
chore/upgrade-dependencies
```

### When to Use Long-Lived Branches

Only for:
- `release/v1.2.0` — release candidates (short-lived, merge back to main)
- `hotfix/critical-security-patch` — emergency production fixes

Never: `dev`, `staging`, `my-feature` (vague), `temp` (always becomes permanent).

---

## Pull Requests

### Small PRs Win

Target: <400 lines changed. Reviewers stop reading past that.

If a PR is large, it's a sign the feature wasn't broken down enough. Go back to SDD Phase 2 (Plan) and split it.

### PR Description Template

```markdown
## What
[One sentence: what does this change do?]

## Why
[Why is this change needed? Link to issue/ticket if applicable]

## How
[Brief explanation of the approach. What alternatives were considered?]

## Checklist
- [ ] Tests added/updated
- [ ] No unrelated changes
- [ ] Self-reviewed the diff
```

### Review Focus (Reviewer)

- Does the code do what the PR description says?
- Are there missing tests?
- Are there unrelated changes? (flag them, don't silently accept)
- Is there a simpler approach? (suggest, don't block)
- Is there a failure mode not handled? (Nygard lens)

---

## SDD Integration

| SDD Phase | Git Action |
|---|---|
| Phase 1: Spec | Create branch `feat/feature-name` |
| Phase 2: Plan | Commit spec/plan doc: `docs(feature): spec for X` |
| Phase 3: Implement | Atomic commits as you go. One logical step = one commit |
| Phase 4: Verify | Tests pass → open PR → merge → delete branch |

### Commit as You Think

Don't batch everything into one massive commit at the end. Commit after each logical step:

```bash
# Working on auth feature
git commit -m "test(auth): add failing tests for 2FA flow"
git commit -m "feat(auth): implement TOTP generation"
git commit -m "feat(auth): add 2FA verification endpoint"
git commit -m "feat(auth): integrate 2FA check in login flow"
git commit -m "docs(auth): document 2FA setup in API reference"
```

This makes bisect, revert, and code review dramatically easier.

---

## Useful Commands

```bash
# Interactive rebase to clean up before PR (squash/reorder/edit)
git rebase -i HEAD~N

# Amend last commit (before push)
git commit --amend --no-edit

# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Stash current work, apply later
git stash && git stash pop

# Find which commit introduced a bug
git bisect start
git bisect bad HEAD
git bisect good <known-good-sha>

# See all changes in a branch vs main
git log main..HEAD --oneline

# Check what's staged before committing
git diff --staged
```

---

## Anti-Patterns

| Anti-pattern | Correct approach |
|---|---|
| "WIP" commits in main history | Squash or amend before merge |
| Huge commits "end of day" | Commit after each logical step |
| `fix`, `fix2`, `fix3` messages | Descriptive conventional commits |
| Long-lived branches (weeks) | Merge frequently, use feature flags |
| Force-pushing shared branches | Never force-push `main`; rebase only on private branches |
| Committing unrelated formatting changes | Separate commit or don't include |
| `.env` files committed | `.gitignore` + rotate credentials immediately |
| Merge commits cluttering history | Use `--rebase` merge strategy on PRs |
