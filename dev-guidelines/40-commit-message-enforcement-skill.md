# 40 — Commit Message Enforcement: Skill Spec

**Audience:** implementers/DevOps setting this up; also useful context for any dev using it.
**Problem this solves:** developers commit with no message or vague messages ("update", "wip", "fix"), making history unreadable and merge conflict resolution guesswork.
**Companion artifact:** the packaged `commit-msg-enforcer` skill (installable) and its pre-commit hook script, delivered alongside this document.

---

## 1. Convention Enforced

**Conventional Commits**, no exceptions:

```
type(scope): summary

[optional body — what changed and why]

[optional footer — Closes #42, BREAKING CHANGE: ...]
```

| Type | Use for |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `test` | Adding/correcting tests |
| `chore` | Tooling, dependencies, build config |
| `perf` | Performance improvement (must reference the measured bottleneck per `10`) |

**Scope** = the affected module/area (`feat(auth): add token refresh`).
**Summary** = imperative mood, no trailing period, under ~72 characters.

## 2. Enforcement Mechanism — Not a Suggestion

Asking politely hasn't worked (stated problem). Enforce at two points:

1. **Pre-commit hook** (local, immediate feedback) — rejects a commit whose message doesn't match the pattern, before it's even created.
2. **CI check on PR** (backstop, in case a hook was skipped/bypassed locally) — fails the PR if any commit in the branch doesn't conform.

Both are provided by the `commit-msg-enforcer` skill package: `assets/commit-msg-hook.sh` is the pre-commit hook logic; wire the same regex into CI as a backstop.

## 3. Explicit Failure Mode to Avoid

A message that matches the pattern but says nothing (`chore: updates`, `fix: fix bug`) **is still the problem this exists to fix** — format compliance is not the goal, information is. The hook checks format (fast, deterministic); it cannot judge content quality. For that:

- The `commit-msg-enforcer` skill, when asked to *draft* a message from staged changes, must describe the actual change and its reason — never a generic placeholder.
- Code review should reject a technically-conformant but content-empty message the same as it would reject any other quality issue.

## 4. What the Agent Does vs. Does Not Do

| Does | Does NOT do |
|---|---|
| Draft a Conventional Commits message from your staged diff, on request | Run `git commit` itself |
| Validate a message you wrote against the convention | Auto-fix and silently commit |
| Explain why a message was rejected | Bypass the hook |

Committing remains a deliberate, human-executed CLI action (`30`) — the agent's role is drafting/validating text, never executing the commit.

## 5. Installation

```bash
# from repo root
cp path/to/commit-msg-enforcer/assets/commit-msg-hook.sh .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
```

Or install the `commit-msg-enforcer` skill in Claude to have it draft conforming messages on request from your staged diff.
