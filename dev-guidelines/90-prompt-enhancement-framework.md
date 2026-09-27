# 90 — Prompt & Document Enhancement Framework

**Audience:** anyone drafting or reviewing prompts, specs, or standards documents like the ones in this folder.
**Purpose:** this is the reasoning framework actually used to draft and audit documents `00` through `40`. Documented here so the method is repeatable, not tacit.

---

## The 7 Steps

| # | Step | Question it answers | What it produces |
|---|---|---|---|
| 1 | **Find the real job** | What does success look like if this runs *unattended*, with no one watching? | A restated goal, sometimes different from the literal request |
| 2 | **Find the unstated assumptions** | What is the writer assuming the reader already knows? | A list of guesses converted into explicit rules |
| 3 | **Weigh recall vs. precision** | Am I adding structure that helps — or rules that fight the original intent? | Only the rules you're confident preserve intent |
| 4 | **Turn vague cautions into checkable behavior** | Can a reader/model actually act on this, or just feel warned by it? | Concrete, testable rules instead of "be careful with X" |
| 5 | **Front-load constraints** | Are rules positioned where they'll actually be followed/found? | Constraints stated before the substantive content, not appended after |
| 6 | **Define "done" from the grader's seat** | What would the person judging this output actually check for? | An explicit, ordered deliverables list |
| 7 | **Check scope, not just structure** | Did I just clarify — or did I quietly add new requirements? | A changelog the requester/reader can audit |

**Rule of thumb:** Steps 1–2 find the gaps. Steps 3–5 fix them without breaking intent. Steps 6–7 make sure the fix is checkable and honest.

## Applied to This Folder

| Document | Where the framework shows up |
|---|---|
| `00-readme.md` | Step 6 — explicit index/deliverables list up front |
| `10-...guidelines.md` | Step 5 — compliance applicability stated before the framework-by-framework detail |
| `20-...standard.md` | Step 2 — resolved two previously undecided naming questions into explicit rules with justification |
| `30-...practices.md` | Step 1 — real job restated as "a junior can follow this with zero prior Git experience" |
| `40-...enforcement.md` | Step 4 — "commit message enforcement" turned into a pre-commit hook, not another unread guideline |

## Self-Review Checklist

- [ ] Step 1: Could I state the real goal in one sentence without re-reading the draft?
- [ ] Step 2: Did I write down every assumption instead of leaving it implied?
- [ ] Step 3: Does every added rule come from a real gap, not structure for its own sake?
- [ ] Step 4: Is every caution a checkable behavior?
- [ ] Step 5: Are constraints placed before the substantive content?
- [ ] Step 6: Is there an explicit, ordered "done" list?
- [ ] Step 7: Did I only clarify, or did I add scope — and if scope was added, is it flagged?
