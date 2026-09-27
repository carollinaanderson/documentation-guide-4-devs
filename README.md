Engineering Coding Best Practices — Index

Internal standards for teams coding with AI code-agent assistance. Read 10 before writing code, 20 before naming anything, 30 before your first PR, 40 before your first commit.

Naming Convention (why the numbers)

Documents are prefixed with a two-digit number in steps of 10 (00, 10, 20...), not sequential (01, 02, 03). This leaves room to insert a new document later (e.g. 15-...) without renumbering everything else — a common pain point in sequential schemes once a folder grows.

Range	Reserved for
00–09	Index/meta (this file)
10–19	Core engineering & compliance guidelines
20–29	Naming, structure, and formatting standards
30–39	Git/GitHub & workflow practices
40–49	Automation, agents, and enforcement tooling specs
90–99	Internal reference/appendix material (frameworks, meta-docs)
Folder Contents

File	Audience	Covers
00-readme.md	Everyone	This index
10-engineering-and-compliance-guidelines.md	Full dev team	Clean code, formatting, commenting, optimization, GDPR/LGPD, SOX, ISO 27001, security by design
20-naming-and-folder-structure-standard.md	Full dev team	Doc naming, code naming (per language), folder structure
30-git-github-practices-guide.md	Junior developers	Repo sizing, fork/branch/merge, PRs, issues, tokens, .env, .gitignore, CLI cheat sheet
40-commit-message-enforcement-skill.md	Implementers / DevOps	Spec for the commit-message enforcement skill + hook (see also the packaged commit-msg-enforcer skill)
90-prompt-enhancement-framework.md	Anyone writing/reviewing prompts or docs like these	The 7-step reasoning framework used to draft and audit this entire folder
Naming Rule Applied to This Folder Itself

Documentation → kebab-case, numeral-prefixed: 10-engineering-and-compliance-guidelines.md
Code/scripts → never numeral-prefixed, and never mixed into a docs folder. See 20-naming-and-folder-structure-standard.md for the full rule and per-language conventions.
Scope Disclaimer

These documents translate engineering-relevant parts of GDPR, LGPD, SOX, and ISO 27001 into practice. They are not legal advice and do not replace a compliance/legal review for systems that handle regulated data or financial reporting.
