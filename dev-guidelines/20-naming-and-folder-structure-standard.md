# 20 — Naming & Folder Structure Standard

**Audience:** full dev team.
**Does not cover:** commit message format (see `40`), Git workflow (see `30`).

---

## 1. Documentation File Naming

**Decision: numeral-prefixed, kebab-case, gaps of 10.**

```
00-readme.md
10-engineering-and-compliance-guidelines.md
20-naming-and-folder-structure-standard.md
30-git-github-practices-guide.md
40-commit-message-enforcement-skill.md
90-prompt-enhancement-framework.md
```

**Why numerals:** enforces a stable reading order in any file browser or repo view, independent of alphabetical sort. Steps of 10 leave room to insert `15-...` later without renumbering the whole set.

**Why kebab-case for docs:** case-insensitive filesystems (macOS/Windows) treat `README.md` and `readme.md` as the same file but not consistently across tools; kebab-case avoids case-collision bugs, is URL-safe for any doc site, and diffs cleanly in git history.

## 2. Code File Naming — Differentiated From Docs

**Decision: code never takes a numeral prefix, and casing follows each language's own idiom — not one global rule.**

| Language | File naming | Symbol naming |
|---|---|---|
| Python | `snake_case.py` | `snake_case` functions/vars, `PascalCase` classes |
| JavaScript/TypeScript | `kebab-case.ts` (or `camelCase.ts` if that's the existing repo convention — pick one per repo, don't mix) | `camelCase` functions/vars, `PascalCase` classes/components |
| Rust | `snake_case.rs` | `snake_case` functions/vars, `PascalCase` types |
| Go | `snake_case.go` (idiomatic Go uses lowercase, no underscores preferred: `handler.go`) | `camelCase`/`PascalCase` per Go convention (exported = PascalCase) |
| C#/.NET | `PascalCase.cs` | `PascalCase` everywhere per .NET convention |

Rule: **don't invent a company-wide casing rule that fights a language's own ecosystem convention** — that creates friction with every external library, linter default, and new hire's prior experience.

## 3. Differentiating Code From Documentation — By Folder, Not Just Casing

Casing alone is fragile (someone will get it wrong). Enforce the split structurally:

```
repo-root/
├── docs/                  ← all documentation, numeral-prefixed, kebab-case
│   ├── 00-readme.md
│   ├── 10-architecture.md
│   └── ...
├── src/                   ← application source code, language-idiomatic naming
│   └── ...
├── scripts/               ← build/tooling/CLI scripts — NOT application source
│   └── ...
└── .github/               ← workflows, PR templates, issue templates
```

- `docs/` — never contains executable code.
- `src/` — never contains numeral-prefixed files.
- `scripts/` — one-off or build-time tooling, kept separate from `src/` so it's obvious what ships in the product vs. what supports building it.

## 4. Quick Reference

| Asking... | Answer |
|---|---|
| "Does this doc get a number?" | Yes, if it lives in `docs/`. Steps of 10. |
| "Does this script get a number?" | No — never. |
| "What casing for a `.py` file?" | `snake_case.py` |
| "What casing for a doc?" | `kebab-case.md` |
| "Where does a build script live?" | `scripts/`, not `src/` |
