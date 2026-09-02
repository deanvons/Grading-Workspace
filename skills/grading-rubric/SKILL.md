# Skill: grading-rubric

> Converts an assignment brief's core and stretch requirements into a per-submission grading
> checklist — split into what can be verified mechanically (build, tests, git history) and what
> stays an instructor judgment call (code quality, whether something is bad enough to flag).

**Reads:** PROJECT.md · `assignment specs/module-[n]-assignment.md`
**Inputs:** Assignment number slug (or a path to its `assignment.md`)
**Output:** `assignment rubrics/[assignment-slug]/rubric.md`

---

## Purpose

Grading takes too long when every submission is read cold against requirements held in memory.
This skill front-loads that work once per assignment, not once per submission: decompose the
brief into discrete, checkable items, and — the actual point of this skill — decide up front
which items a grading pass can verify mechanically (it compiles, the tests pass, a formula
reproduces the worked example, git history shows a failing test before the passing
implementation) versus which items are irreducibly a judgment call (code quality, whether
something is bad enough to call out).

Get that split right here and `submission-review` (the companion skill that runs this rubric
against an actual student repo) can auto-fill the mechanical items with real evidence and leave
the judgment items genuinely open. Get it wrong and either the instructor re-verifies things
that were already checked, or trusts an unsupervised "assessment" of something no agent should
be calling on its own.

---

## Step 1 — Locate the assignment

Confirm which assignment (slug or path). Read its `assignment.md` in full: Core requirements,
Stretch requirements, any worked examples, the Testing section, the Submission section, and any
Clean Code / quality expectation named explicitly. If the brief has a two-phase (solo → swap)
structure, note that — Phase 2 requirements (SOLID violation found and fixed, a pattern applied,
an extension built) need their own section since they can't be checked until the swap happens.

---

## Step 2 — Decompose Core requirements into atomic checklist items

For each requirement group, break it into the smallest independently checkable claims — not one
line per paragraph, one line per fact a grader would otherwise have to hold in their head while
reading code. "Three survivor roles with correct starting attributes" is really several
separable checks (role exists, each role's attribute table matches exactly) — decide item
granularity by what's genuinely independent, not by mechanically exploding every noun in the
brief.

For each item, classify it:

- **Mechanically verifiable** — the agent can check it directly: run the build, run the test
  suite, execute a specific calculation and compare to a known value, grep for a specific
  exception type being thrown where the brief requires it, inspect `git log` for a pattern.
  State **how** to check it, not just that it's checkable — this instruction is what
  `submission-review` will actually execute later, so vague ("verify the design is good") is
  not a valid mechanical check.
- **Judgment call** — requires reading and interpreting code quality, structure, or intent.
  Leave this as an open field for the instructor; do not attempt to pre-score it here or later.

---

## Step 3 — Turn worked examples into exact reproduction checks

Every worked example in the assignment (a formula, a calculation, a scenario with a stated
correct result) becomes its own mechanically verifiable item: state the exact inputs and the
exact expected output from the brief. The check is "does the student's implementation produce
this number for these inputs" — not "does the formula look approximately right."

---

## Step 4 — Pull the Testing section in directly

If the assignment has a testing/TDD requirement, list every specific behavior the brief names as
requiring coverage, each as its own checklist item (mechanically verifiable — does a test exist
covering it, does it pass). If the brief asks for TDD evidence in commit history (a failing test
committed before the implementation that passes it), add that as its own mechanically verifiable
item with the check method spelled out: inspect `git log`/`git show` for the red-before-green
pattern, for at least the number of behaviors the brief names as the bar.

---

## Step 5 — Submission / process checklist

Pull the Submission section's requirements (README present, meaningful commit history, group
member names if applicable) into their own short checklist. Most are mechanically verifiable
(does a README exist, does commit history show more than one lump commit) — but repo-access
items an agent genuinely cannot observe from the code (e.g. "lecturer added as Maintainer") must
be marked **instructor-only, not checkable from the code**, not silently skipped or guessed at.

---

## Step 6 — Code quality / judgment section

If the assignment names a Clean Code or quality expectation, give it its own dedicated section —
free-text, not checkboxes. State plainly in the rubric itself that this section is instructor
judgment; `submission-review` may add tentative, clearly-labeled observations here, but they are
pointers to look at, never a verdict to accept as-is.

---

## Step 7 — Present for instructor review

Show the full rubric before saving. Ask:

- Did the decomposition miss a requirement, or split one into checks that are too fine/too coarse?
- Is the mechanical/judgment split right — anything marked judgment that's actually checkable,
  or vice versa?
- Do the worked-example reproduction checks use the exact numbers from the brief?
- Is anything from the Submission section wrongly marked checkable when it isn't observable from
  code alone?

Adjust until confirmed. Do not save before confirmation.

---

## Step 8 — Save

Save to `assignment rubrics/[assignment-slug]/rubric.md`.

---

## Output format

```markdown
# Grading Rubric — [Assignment Title]

**Source:** assigment specs/module-[n]-assignment.md
**Generated:** [date]

---

## Functional requirements

### [Requirement group]

- [ ] **[Item]** — *Mechanically verifiable: [exactly how to check it].* Evidence:
- [ ] **[Item]** — *Judgment call.* Notes:

[repeat per group]

---

## Worked-example checks

- [ ] **[Calculation name]** — Inputs: [...]. Expected: [exact value from the brief]. Actual:

---

## Testing

- [ ] **[Specific behavior named in the brief]** — test exists and passes. Evidence:
...
- [ ] **TDD evidence** — *Mechanically verifiable: `git log` shows a failing test committed
  before the implementation that passes it, for at least [N] core behaviors.* Evidence:

---

## Submission / process

- [ ] **[Item]** — *Mechanically verifiable* / *Instructor-only, not checkable from code*

---

## Code quality (instructor judgment — not scored by an agent)

[Free text. An agent may add tentative, clearly-labeled observations here — never a score.]

---

## Overall feedback (instructor-authored)

[Free text — the actual message to the student. Not filled in by `grading-rubric` or
`submission-review`.]
```
