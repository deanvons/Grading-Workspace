# Grading Workspace

Hello Nicholas — this is where you grade assignment submissions, kept separate from
the content-factory repo where the assignments themselves are authored. See
[PROJECT.md](PROJECT.md) for how the workflow fits together.

## How this works

1. `/grading-rubric module-N` — builds a grading checklist from the assignment brief
   in `assigment specs/`, saved to `assignment rubrics/module-N/rubric.md`. Run once
   per assignment.
2. `/submission-review module-N <path-or-url>` — runs that rubric against a student's
   actual submission and writes a draft feedback file into the student's own repo. It
   never commits or pushes anything there — that's on you, once you're happy with it.

## Getting a student's submission onto disk

`submission-review` just needs a local path to the student's cloned repo (it can clone
a URL for you too). Two ways to keep that organized:

- Clone submissions into a `submission repos/` folder inside this workspace (create it
  the first time you need it) — keeps everything for this course in one place. This
  folder is gitignored, so cloned student repos never end up tracked in this repo.
- Or clone them anywhere else on disk, e.g. as a sibling of this project folder — just
  point `/submission-review` at the local path.

Either works fine. This repo never stores or commits student submissions themselves,
only the rubrics and the feedback drafts written back into each student's own repo.
