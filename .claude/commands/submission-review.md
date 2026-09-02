---
description: Run a grading rubric against one student's actual submission — build it, run its tests, check calculations and git history for real — producing a draft-filled rubric with judgment calls left open. Pass the assignment slug and the student's repo path or URL.
---

Read PROJECT.md and skills/submission-review/SKILL.md carefully.

The assignment and submission to review: $ARGUMENTS

If the rubric, the submission path, or the student's name is not already clear, ask rather than assuming. If no rubric exists yet for this assignment at assignment rubrics/module-[n]/rubric.md, say so and suggest running grading-rubric first rather than improvising checks on the fly. Follow the steps in SKILL.md exactly — actually run every mechanically-verifiable check with real evidence, leave every judgment-call item open for the instructor rather than pre-scoring it, and write feedback directly to the student in second person, never in a third-person report voice.

Never run git add, git commit, or git push in the student's repo, at any point, including revisions after instructor feedback. Leave the output file untracked and let the instructor commit and push it themselves when ready.
