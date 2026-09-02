# Skill: submission-review

> Runs a rubric (from `grading-rubric`) against one student's actual submission — builds it, runs
> its tests, checks calculations and git history for real — and produces a draft-filled copy of
> the rubric with evidence, leaving judgment-call items genuinely open for the instructor.

**Reads:** the rubric at `assignment rubrics/module-[moduleNumber]/rubric.md` · the student's
submission (a local path, or a repo to clone if not already local)
**Inputs:** Assignment slug (or rubric path) · path or URL to the student's submission · student
name
**Output:** a draft-filled rubric, written as an **untracked file** in the student's local repo
(see Step 5). **Never committed or pushed by this skill, under any circumstance** — that action
belongs to the instructor alone, once, when they're satisfied with the content.

---

## Purpose

This is the half of the workflow that keeps the instructor genuinely in the loop rather than
rubber-stamping an AI verdict. The job here is narrow and specific: execute every
mechanically-verifiable check the rubric defines, with real evidence — not a plausible-sounding
guess — and leave every judgment-call item genuinely open. A rubric item marked "Met" by this
skill means the check was actually run and passed, not that the code looked probably fine on a
skim. If a check can't be run (build failure, missing dependency), say so explicitly rather than
guessing at an answer.

---

## Step 1 — Confirm inputs, and actually read the rubric

Confirm: which rubric, which student's submission (a local path — clone it first if given a repo
URL rather than an already-cloned path), and the student's name. If any of these is missing or
ambiguous, ask rather than assuming.

**Read the rubric file in full, every time, even for the tenth submission against the same
assignment.** Do not work from memory of the brief or from recall of what a previous submission
needed — the rubric is the checklist specifically because memory drops items. Keep it open as
you work through Step 4 and go down its actual line items in order, checking each one off
explicitly, rather than reviewing the code holistically and letting coverage fall out of
whatever you happened to notice. A requirement you don't independently recall checking is not
checked, no matter how thorough the rest of the review looks — this is exactly how a missing
`PerformAction` method got through an otherwise-detailed review: real bugs were found by reading
the code closely, but the rubric's own explicit line for it was never actually consulted.

---

## Step 2 — Confirm what you're actually grading

Before touching the rubric, check the submission's `git log --all --graph` for signs the
assignment has already moved past the phase you're grading — most concretely, a merge commit
authored by (or merged from) someone other than the student, or a branch entirely authored by
someone else, either of which usually means a two-phase "solo, then swap" assignment has already
had its swap. If you find one:

- Identify the last commit before that other author's work starts — that's the actual Phase 1
  (solo) boundary.
- Grade against that commit, not `HEAD`, if it's the solo phase you've been asked to grade. Say
  explicitly, in the output, which commit you graded against and why (name the commit/branch
  that marks the boundary) — this is a scope decision the instructor needs visible, not a silent
  default.
- If you need to actually build/run code at that historical point, check out that commit
  (detached HEAD is fine for inspection), and **check the repo out back to its original branch
  when you're done, before finishing the review.** Don't leave a student's repo sitting in
  detached HEAD.
- If it's genuinely unclear which phase you've been asked to grade, or whether a found merge
  represents the swap the rubric assumes, ask rather than guessing.

**The boundary is who authored the work, not when it happened.** Students submit when they're
done — there is no submission deadline this skill should infer from commit dates, and a gap in
timing (a commit landing days or weeks after the rest) is not, on its own, evidence that it
belongs to a different phase. Only exclude a commit because it's the swap partner's work
(different author, or living on the swap partner's branch) — never because it's dated later than
the commits around it. A late commit still authored by the original student, unconnected to the
swap, is still part of their submission and must be graded, no matter how late it is. Getting
this wrong silently drops real, gradable work — if a late same-author commit's status is
ambiguous, say so and ask, don't default to excluding it.

---

## Step 3 — Get the submission running

Attempt to build it, and run its existing test suite once to see baseline state.

**If it doesn't build, work out whether that's the student's code or your environment before
concluding anything.** A version mismatch (their build config targets a toolchain version newer
or different than what's installed here) is an environment problem, not a defect — try a
reasonable command-line override (e.g. a compiler/target-version flag) to get a real signal on
the actual code, and say explicitly in the output that this happened and what the workaround
was. **Never edit the student's own config files** (their `pom.xml`, `.csproj`, `package.json`,
etc.) to make this happen — override at the command line only, so their submission stays exactly
as they wrote it. If it still doesn't build after a reasonable environment workaround, that's a
real top-line finding — many rubric items become unreachable, so note explicitly, per item,
which ones couldn't be checked as a result rather than silently marking them Missing.

---

## Step 4 — Work through the rubric, mechanical items only

For every item marked **mechanically verifiable** in the rubric, actually perform the stated
check — don't paraphrase or infer from reading the code alone if the rubric specifies a runnable
check:

- **Build/test-suite items** — run it; record pass/fail and the actual output, not a summary of
  what you expect it to say.
- **Worked-example checks** — actually exercise the student's code with the exact inputs stated
  in the rubric and compare to the exact expected output; paste the actual result obtained.
- **Exception/behavior checks** — locate the actual code (file:line) and confirm it does what's
  claimed; if concluding something is absent, confirm that by reading/grepping the relevant area
  before marking it Missing, not by failing to find it on a first pass.
- **TDD/git-history checks** — actually run `git log`, inspect the relevant commits, and cite the
  specific commit hashes/messages that do (or don't) show the failing-test-before-passing-code
  pattern the rubric asks for. Spot-check at least one claimed pattern directly (e.g. confirm the
  behavior genuinely doesn't exist yet at the earlier commit) rather than trusting commit
  messages at face value — a message can claim TDD without it being true.

Along the way, check whether the repo already contains evidence that corroborates (or
contradicts) a finding — a swap partner's own legacy-code notes, an existing doc, a prior
review comment — and cite it directly if it does. Independent evidence already sitting in the
repo is stronger than a finding generated fresh, and worth surfacing either way.

Mark each item Met / Partial / Missing with the evidence inline: a file:line reference, a command
and its actual output, a commit hash. An item with no evidence attached is not done, regardless
of what status it's marked.

---

## Step 5 — Judgment-call items — leave them open

Do not fill in a verdict for any item marked **judgment call**, nor for the Code Quality section.
A **tentative, clearly-labeled observation** is fine when something is genuinely conspicuous —
e.g. "NOTE: this class is 400 lines and handles five unrelated responsibilities — flagging for
your read, not scoring it" — but write it as a pointer to look at, never as a finding presented
as settled. The instructor decides what's worth saying to the student, and how.

---

## Voice — write to the student, not about them

The output of this skill is feedback a real student will read, not an internal audit report
being filed about them. Address them directly in the second person throughout — "you," "your" —
the same convention `phase-authoring` uses for README content, for the same reason: this is a
direct conversation with the person reading it.

Wrong: "Vibeke — who inherited this codebase cold for the Phase 2 swap, with no chance to ask
Christian for context — wrote in her own legacy-code notes... worth raising with Christian
directly given how deliberate the choice looks."

Right: "Vibeke, who read your code cold during the Phase 2 swap with no chance to ask you about
it, called this out unprompted in her own notes..."

This applies everywhere — findings, gaps, notes. Write like an instructor talking to the
student, not like a report being handed to someone else about them. Keep it natural and
specific, not clinical — say what you found and how you know it's true without sounding like an
audit log ("confirmed by search," "verified directly," "worth noting" as a reflexive hedge).
State it plainly instead.

**Never narrate the review process in the first person** — "I've been through," "I've reviewed,"
"I checked," "I confirmed," "I tested this directly" all read as the tool describing its own
work, not as the instructor speaking. State findings directly instead: not "I ran your code and
got 81.6" but "your code produces 81.6 for this exact scenario," not "confirmed over 100,000
trials" but "over 100,000 trials, X never happens." The instructor is the speaker throughout;
this skill's own involvement should be invisible in the output, even though the evidence behind
each finding is real. This is easy to get right on the first draft of a finding and then slip
back into on a later revision — before presenting the file, reread it once specifically hunting
for "I" as the sentence subject, not just for structure or content, since content review doesn't
reliably catch this pattern.

Wrong: "I've been through another Phase 1 solo submission for this same assignment that used an
enum-keyed map instead, specifically to support a generic weighted-sum calculation..."

**Never reference another specific student's submission as a comparison point in student-facing
output, at all, regardless of phrasing.** This isn't a voice problem to rephrase around — it's a
call about what's appropriate to share across students that belongs to the instructor alone. If
a genuinely useful cross-submission observation comes up while grading, raise it in conversation
with the instructor, separate from the file being written for the student, and let them decide
whether and how to use it. Don't write it into the student's feedback file on your own judgment.

## Output format — functional requirements as a checklist, everything else as notes

Render every functional-requirement item from the rubric as `- [✅] ...` for Met or `- [⚠️] ...`
for Missing/Partial (with a one-line note on what's missing) — same bracket structure as a
markdown checkbox, but with the emoji in place of `x`/a blank space. This should read at a
glance as "here's what's checked off," not as prose the instructor has to parse to find the one
item that didn't pass.

Everything else — code quality observations, process notes, judgment-call items, anything with
real nuance — stays as ordinary notes or bullets, never a checkbox. A checkbox implies a binary
pass/fail; reserve it for the things that genuinely are.

---

## Step 6 — Save and hand off

Write the filled draft as a file directly in the student's local repo — e.g.
`GRADING-FEEDBACK.md` at the repo root — unless the instructor has already established a
different location. Do not invent a different location silently if one's already established.

**Never run `git add`, `git commit`, or `git push` in the student's repo. Ever, for any reason,
including a follow-up revision.** Leave the file untracked. This is not a preference — it is a
hard rule, because of what happens without it: a grading pass is iterative (the instructor reads
the draft, asks for a rewrite, asks for a different tone, asks for a different format — this
happens routinely, not as an edge case), and every one of those intermediate revisions becomes a
permanent, visible commit in the student's history the moment it's pushed. The student — and any
other maintainer added for a swap phase — sees every rough draft, every corrected mistake, every
"actually, phrase it differently" pass, none of which was ever meant for them to see. There is no
version of "just this once" that's safe here; treat every single grading run as a case where
pushing would cause exactly this problem, because the previous version of this rule was learned
by doing it wrong and having to force-push a real student's repo back to a clean state afterward.

Committing and pushing are the instructor's action alone, done once, when they're satisfied with
the content — not something this skill does at any point in the process, draft or final.

---

## A note on how this gets invoked

This skill is most useful run from the same session/workspace that already has the content pool
available (so the rubric and this SKILL.md are both reachable) — reference the student's cloned
repo by its local path rather than needing it open as the active project root. If the instructor's
workflow instead opens each student repo as its own standalone project window, this file (plus
the target rubric) needs to be copied into that repo's local working tree first, the same way
`phase-authoring` was copied into `tractus-dotnet` for its autonomous run — ask which mode is in
use if it isn't already clear from context.
