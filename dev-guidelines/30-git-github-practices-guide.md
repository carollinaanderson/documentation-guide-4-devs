# 30 — Git/GitHub Practices Guide

**Audience:** junior developers. No prior Git experience assumed.
**Does not cover:** commit message format/enforcement (see `40`), file naming (see `20`).

---

## 1. Repo Sizing (DevOps Perspective)

| Approach | When to use | Trade-off |
|---|---|---|
| **Single-responsibility repo** (one service = one repo) | Default choice. Each deployable unit gets its own repo. | Clean CI/CD per service, independent versioning; more repos to track |
| **Monorepo** | Multiple tightly-coupled services that always deploy together, or shared libraries used by many services | Simpler cross-service refactors; requires more sophisticated CI tooling to avoid rebuilding everything on every change |

**Rule of thumb:** if two things are deployed independently and rarely change together, they're separate repos.

## 2. Fork vs. Branch

| | Fork | Branch |
|---|---|---|
| **What it is** | Your own copy of the entire repo, under your account | A parallel line of work inside the same repo |
| **Use when** | Contributing to a repo you don't have write access to (open source, external partners) | Working within the team's own repo |
| **Workflow** | Fork → clone your fork → branch → commit → push to your fork → open PR back to the original repo | Clone once → branch → commit → push → open PR within the same repo |

**Team default: branching, not forking** — everyone has write access to team repos.

## 3. Branching Model

**Decision: trunk-based development.**

- `main` is always deployable.
- Short-lived feature branches: `feat/short-description`, `fix/short-description`.
- Branches live days, not weeks — merge fast, behind a feature flag if the feature isn't ready for users yet.
- No long-lived `develop`/`release` branches — they drift from `main` and cause exactly the merge chaos this guide exists to prevent.

```bash
git checkout main
git pull
git checkout -b feat/add-user-export
# ...work, commit...
git push -u origin feat/add-user-export
# open PR
```

## 4. What a PR Is, and the Review Bar

A **Pull Request (PR)** proposes merging your branch into `main`. It is the traceable approval record required by SOX for any financially-relevant change (see `10`).

**Minimum bar before merge:**
- At least one human reviewer approves (never self-merge on regulated/production systems).
- CI passes (formatting, lint, tests — see `10`).
- Commit messages conform to the standard in `40`.
- Reviewer is not the author, for SOX-relevant systems (segregation of duties).

## 5. What an Issue Is

An **Issue** tracks a bug, task, or feature request — independent of any specific branch. Reference issues from PRs (`Closes #42`) so merging the PR auto-closes the tracked work and both stay linked in history.

## 6. Secrets: Token Rotation, `.env`, `.gitignore`

**Token rotation procedure:**
1. Generate the new token in the provider's dashboard (GitHub, cloud provider, etc.).
2. Update it in the secrets manager / CI secret store — never in a file that gets committed.
3. Redeploy/restart services that read the secret.
4. Revoke the old token.
5. If rotation is due to *suspected exposure* (e.g. a token was accidentally committed): revoke immediately, first — rotate before anything else, even before removing it from git history.

**`.env` — local secrets, never committed:**
```
# .env  (gitignored — real values live only here, locally)
DATABASE_URL=postgres://localhost/dev
API_KEY=your-local-dev-key
```
```
# .env.example  (committed — shows required keys, no real values)
DATABASE_URL=
API_KEY=
```

**`.gitignore` — minimum baseline:**
```
.env
.env.*
!.env.example
node_modules/
__pycache__/
*.pyc
target/
.DS_Store
*.log
```

## 7. Git/GitHub CLI Cheat Sheet

**Why CLI, not an agent:** committing is a deliberate, human-reviewed action — not something to automate away. Use an agent to *draft* a commit message (see `40`), but run the commit yourself.

| Task | Command |
|---|---|
| Clone a repo | `git clone <url>` |
| Create + switch to a branch | `git checkout -b feat/my-feature` |
| See what changed | `git status` / `git diff` |
| Stage changes | `git add <file>` / `git add .` |
| Commit | `git commit -m "type(scope): summary"` |
| Push a new branch | `git push -u origin feat/my-feature` |
| Push subsequent commits | `git push` |
| Update local `main` | `git checkout main && git pull` |
| Bring `main` into your branch | `git checkout feat/my-feature && git rebase main` (preferred over merge for a clean history) |
| See commit history | `git log --oneline --graph` |
| Open a PR from CLI | `gh pr create` |
| List your PRs | `gh pr list` |
| Check out someone's PR locally | `gh pr checkout <number>` |
| Merge vs rebase | Use `rebase` to update your branch from `main`; use the PR's "squash and merge" button to land into `main` — avoid manual `git merge` on shared branches |

## 8. The Classic Merge Chaos — Why It Happens

Usually one of: (1) long-lived branches drifting far from `main`, (2) no PR review requiring a rebase before merge, (3) commits with no message making conflict resolution guesswork. Trunk-based development (short branches) + mandatory PR review + enforced commit messages (`40`) address all three directly.
