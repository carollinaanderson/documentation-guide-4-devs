# 10 — Engineering & Compliance Guidelines

**Audience:** full dev team. Assumes working competence — this is a reference standard, not a tutorial.
**Does not cover:** Git/GitHub workflow (see `30`), file/folder naming (see `20`), commit message format (see `40`).
**Compliance disclaimer:** the sections below translate GDPR, LGPD, SOX, and ISO 27001 into engineering practice. This is not legal advice — consult legal/compliance for regulated systems.

---

## 1. Clean Code Principles

| Principle | Rule | Example |
|---|---|---|
| **Naming** | Names say what a thing does/is, not how it's implemented. No single-letter names outside tight loop counters. | ❌ `d = calc(u)` → ✅ `discount = calculate_loyalty_discount(user)` |
| **Function size** | One function does one thing. If you need "and" to describe it, split it. | ❌ `validate_and_save_and_notify(order)` → ✅ three functions, one caller |
| **Single Responsibility** | A class/module has one reason to change. | A `User` model shouldn't also format emails — that's a `UserNotifier`'s job |
| **Avoid duplication (DRY)** | Extract repeated logic once it appears a 3rd time — not the 2nd (avoid premature abstraction). | Rule of three, not rule of two |
| **Guard clauses over nesting** | Return/raise early instead of nesting conditionals. | ❌ `if (a) { if (b) { ... } }` → ✅ `if (!a) return; if (!b) return; ...` |
| **No magic numbers/strings** | Named constants for anything with business meaning. | ❌ `if status == 3` → ✅ `if status == OrderStatus.SHIPPED` |

## 2. Code Formatting

Formatting is **not a matter of taste** — it's automated and enforced, not debated in review.

| Language | Formatter | Enforcement |
|---|---|---|
| Python | `black` + `ruff` | CI fails on unformatted diff |
| JS/TS | `prettier` + `eslint` | pre-commit hook + CI |
| Rust | `rustfmt` + `clippy` | `cargo fmt --check` in CI |
| Go | `gofmt` | built into `go vet` step |

Rule: if your formatter and a reviewer disagree, the formatter wins — file a config change, don't hand-override in a PR.

## 3. Code Commenting

| Comment when... | Don't comment when... |
|---|---|
| Logic is non-obvious (an algorithm choice, a regex, a workaround) | The code already says it (`i++  // increment i` — delete this) |
| A field/variable holds sensitive data — tag it: `# PII`, `# SOX-relevant`, `# regulated` | You could rename the variable instead to make it self-explanatory |
| A workaround exists for a known bug/limitation — link the issue | Restating a function's obvious behavior |
| A business rule isn't derivable from the code alone ("tax rule per LGPD Art. 46") | — |

Rule of thumb: a comment should explain **why**, never **what** — the code already says what.

## 4. Optimization Practices

- **Default: readability over cleverness.** Do not optimize until a real bottleneck is measured.
- **Valid triggers to optimize:** a profiler shows the actual hotspot, or a measured SLA/latency budget is breached.
- **Invalid triggers:** "this might be slow," "I read that X is faster," aesthetic preference.
- When you do optimize: leave a comment stating the measured problem and the profiling evidence, so a future reader doesn't "clean up" your optimization back into the slow version.

## 5. Compliance: Applicability First

**Not every repo needs every framework below.** Before applying a section, check:

| Framework | Applies when your service... |
|---|---|
| GDPR | Processes personal data of EU residents |
| LGPD | Processes personal data of Brazilian residents |
| SOX | Touches financial reporting, revenue recognition, or systems an auditor would review |
| ISO 27001 | Applies broadly as a baseline security posture — treat as default-on unless explicitly scoped out |

### GDPR / LGPD — Engineering-Actionable Rules
- **Data minimization:** don't collect/store a field "just in case" — every stored field needs a stated purpose.
- **PII classification:** tag PII fields in schemas and code comments (`# PII: email`) so agents and humans don't handle them carelessly.
- **Right to erasure:** data models for any PII-holding entity must support a real delete path, not just a soft-delete flag with no downstream cleanup.
- **Consent logging:** where consent gates data use, log the consent event itself (timestamp, scope, version of terms) — not just a boolean.
- **Cross-border transfer flags:** if data can leave its origin region (e.g. US-hosted processing of EU data), that path must be explicitly documented, not incidental.

### SOX — Engineering-Actionable Rules
- **Change-control traceability:** every change to a financially relevant system must be traceable to an approved PR — ties directly to the PR review bar in `30`.
- **Audit-log retention:** systems touching financial data retain audit logs per the company's defined retention period — don't let logs rotate out silently.
- **Segregation of duties:** the person who approves a PR to a SOX-relevant system should not be the same person who authored it.

### ISO 27001 — Engineering-Actionable Rules
- **Access control:** least privilege by default — request the narrowest scope/role that does the job.
- **Secure SDLC checkpoint:** security review is a gate before merge for anything touching auth, payments, or PII — not a post-launch afterthought.
- **Incident-response hook:** know the internal channel/process to report a suspected security issue — don't sit on it.
- **Periodic access review:** access grants (repo, prod, secrets) are reviewed on a recurring cadence, not granted once and forgotten.

### Security by Design & Sensitive Data
- **Threat model at design time** for anything touching auth, payments, or PII — not as a pre-merge checklist afterthought.
- **Never commit secrets** — see `30` for `.env`/`.gitignore` rules and token rotation procedure.
- **Input validation at every trust boundary** (API edge, file upload, third-party webhook) — never trust client-supplied data implicitly.
- **PII/sensitive fields tagged in code** so both humans and AI code agents can identify them without inferring from context.
