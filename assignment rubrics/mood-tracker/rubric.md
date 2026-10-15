# Grading Rubric — Mood Tracker

**Source:** content/pool/assignments/mood-tracker/assignment.md
**Generated:** 2026-10-15

> **Scope note.** The brief has no Core/Stretch labels. Anything it marks "Optional", "may" or
> "try" is listed as *(Optional, not scored)*. Everything else is treated as core.
>
> **API under test:** `https://frontend-assignment-api-2awo.onrender.com` — `GET /moods`
> (read-only) and `/affirmation_users` (json-server, full REST). Mood entries are stored as
> emoji strings in the user record's `affirmations` array: `{ id, username, affirmations: [] }`.
> The free Render instance cold-starts and can take 30–60 s on the first request.
>
> **Shared API hygiene for submission-review:** for any behavioural check, log in with a
> throwaway username nobody else will use (e.g. `rubric-check-<timestamp>`). Never touch another
> user's record. Delete that one record when you're done.

---

## Functional requirements

### 1. Setup and build

- [ ] **React + Vite project** — *Mechanically verifiable: `package.json` lists `react` and `react-dom` as dependencies and `vite` as a dev dependency; a `vite.config.*` exists.* Evidence:
- [ ] **Installs and builds cleanly** — *Mechanically verifiable: `npm ci` (or `npm install`), then `npm run build` exits 0.* Evidence:
- [ ] **Runs locally** — *Mechanically verifiable: `npm run dev` starts and the root URL serves the app without a console error on load.* Evidence:

### 2. Component tree diagram

- [ ] **Diagram exists in the repo as a PDF** — *Mechanically verifiable: `git ls-files '*.pdf'` returns at least one file that is recognisably a component tree.* Evidence:
- [ ] **Diagram names the three required parts** — *Mechanically verifiable: open the PDF and confirm it has nodes for (a) the mood selection form, (b) the affirmation display, (c) the profile history.* Evidence:
- [ ] **Diagram matches the code** — *Judgment call: do the components in `src/` roughly match the tree, or was the tree drawn afterwards and never followed?* Notes:

### 3. HTML and CSS styling

- [ ] **App is styled** — *Mechanically verifiable: the project has CSS files/modules, or a Tailwind/Bootstrap dependency that is actually imported.* Evidence:
- [ ] **Layout follows the wireframes and is usable** — *Judgment call.* Notes:
- [ ] *(Optional, not scored)* Responsive layout.

### 4a. Startup / Login page

- [ ] **Login is the first screen for a new session** — *Mechanically verifiable: clear localStorage, load `/`, and confirm a name input and a submit control render (or, statically, confirm the default route maps to the login component).* Evidence:
- [ ] **Checks whether the user exists before creating one** — *Mechanically verifiable: the code issues `GET /affirmation_users?username=<name>` (or fetches all users and filters) **before** any `POST`. Behaviourally: log in twice with the same throwaway name, then `GET /affirmation_users?username=<name>` returns exactly **one** record.* Evidence:
- [ ] **A new user is created with the standard shape** — *Mechanically verifiable: after first login with a throwaway name, the API record has `username` equal to the entered name and `affirmations` equal to `[]`.* Evidence:
- [ ] **An existing user is reused, not overwritten** — *Mechanically verifiable: give the throwaway user one entry, log out, log in again with the same name, and confirm the entry is still in `affirmations`.* Evidence:
- [ ] **Login navigates to the Mood Selector page** — *Mechanically verifiable: after submitting the name, the Mood Selector view renders.* Evidence:
- [ ] *(Optional, not scored)* Session kept in localStorage, so a returning logged-in user lands directly on the Mood Selector.

### 4b. Mood Selector / Entry page

- [ ] **Moods are fetched from the API at runtime** — *Mechanically verifiable: grep shows a `fetch`/`axios` call to `/moods`, and no hard-coded array of mood names or emojis is used as the rendered source.* Evidence:
- [ ] **Every API mood is rendered with its emoji and name** — *Mechanically verifiable: see the worked-example check "Mood list reproduces the API".* Evidence:
- [ ] **More than one mood can be selected at once** — *Mechanically verifiable: select two or more moods and confirm all of them are visibly selected at the same time.* Evidence:
- [ ] **Selected emojis are combined into one string per entry** — *Mechanically verifiable: see the worked-example check "Combined entry string".* Evidence:
- [ ] **Submitting persists the entry to the user's record** — *Mechanically verifiable: after submitting, `GET /affirmation_users/<id>` shows the new string appended to `affirmations`, and earlier entries are unchanged.* Evidence:
- [ ] **The last entry is shown on the selector page** — *Mechanically verifiable: after submitting, the page shows the just-saved emoji string (the wireframe's "Last Mood Entry:").* Evidence:

### 4c. Profile page

- [ ] **Shows the last 10 entries as emojis** — *Mechanically verifiable: see the worked-example check "Last-10 window".* Evidence:
- [ ] **Shows all entries when there are fewer than 10** — *Mechanically verifiable: with 3 entries, all 3 render and nothing breaks.* Evidence:
- [ ] **A "Clear History" button exists** — *Mechanically verifiable.* Evidence:
- [ ] **Clear History deletes the entries in the API, not just in the UI** — *Mechanically verifiable: click it, then `GET /affirmation_users/<id>` returns `affirmations: []`; reload the profile and the history stays empty.* Evidence:
- [ ] **Clear History affects only this user** — *Mechanically verifiable: the request targets `/affirmation_users/<own id>` only, and the user record itself is not deleted.* Evidence:
- [ ] *(Optional, not scored)* **3.3 Affirmation based on the dominant mood category** — if present, check it against the worked example "Dominant category".
- [ ] *(Optional, not scored)* **3.4 Background image or illustration based on the dominant mood category.**

---

## Worked-example checks

- [ ] **Mood list reproduces the API** — Inputs: live `GET /moods`. Expected: the rendered set equals the API response exactly. On 2026-10-15 that is **12 moods**: 😊 Happy, 🌊 Calm, ⚡ Energized, 🧘 Reflective, 🌄 Hopeful, 😴 Tired, 😟 Anxious, 😌 Content, 😣 Stressed, 🤔 Curious, 🎉 Excited, 😔 Lonely. Re-fetch at grading time and compare to that result, not to this list. ⚠ Before 2026-10-15 the brief's example showed 6 moods wrapped in `{ "moods": [...] }`, but the live API returns a **bare array**. A student working from the old brief who wrote `data.moods` gets an empty list. If the submission's code reveals this, treat it as an error in the brief, not the student's mistake. Actual:
- [ ] **Combined entry string** — Inputs: select Energized, Hopeful, Reflective, Curious (the wireframe's example) and submit. Expected: one string made of exactly the codepoints `⚡🌄🧘🤔`, appended to `affirmations`. Compare codepoints, not rendered glyphs. The brief doesn't specify order, so selection order and list order (`⚡🧘🌄🤔`) are both acceptable. Actual:
- [ ] **Last-10 window** — Inputs: give the throwaway user 12 distinct entries, e1 to e12, in order. Expected: the profile shows exactly e3 to e12; e1 and e2 are not shown. Display order is not specified. Actual:
- [ ] *(Optional, only if 3.3 or 3.4 is implemented)* **Dominant category** — Inputs: `affirmations` = `["😊🌊", "😴", "🧘🌄"]`. Expected: **Positive** (3 Positive, 1 Neutral, 1 Negative, counted per selected mood). Ties are unspecified, so judge them, don't check them mechanically. Actual:

---

## Testing

Not applicable — this assignment brief has no Testing/TDD requirement.

---

## Submission / process

- [ ] **Component tree PDF committed to the repo** — *Mechanically verifiable* (same check as section 2). Evidence:
- [ ] **Repo link submitted** — *Instructor-only — checked against the submission, not the code.*
- [ ] **Repository is private** — *Instructor-only, not checkable from code.*
- [ ] **Lecturer added as Maintainer** — *Instructor-only, not checkable from code.*
- [ ] *(Optional, not scored)* **Hosted (e.g. on Vercel)** — if a URL appears in the README or the submission, *mechanically verifiable: the URL loads, the login works, and the mood list renders from the live API.*

---

## Code quality (instructor judgment — not scored by an agent)

Pointers to look at — not a checklist, not a score:
- Component decomposition: do the components have one job each, or is there one god-component holding every screen?
- State: is it lifted to the right level (current user, selected moods, history)? Is anything duplicated that should be derived?
- API access: are calls collected in a service/module with one base URL, or are `fetch` calls with copy-pasted URLs scattered through components?
- `useEffect` hygiene: correct dependency arrays, no fetch loops, no state updates after unmount.
- Loading and error states: does the UI handle the Render cold start (30–60 s) and a failed request, or does it show a blank page?
- Write semantics: `PATCH` vs `PUT` when appending entries. Could a `PUT` wipe fields other apps wrote to the shared record?
- Empty submit: what happens if Submit is pressed with no mood selected?
- Accessibility: toggles are real `<button>`s with a selected state (`aria-pressed`), and emojis have text labels.
- Session handling: is the localStorage value validated, or does a stale or deleted user crash the app?

---

## Overall feedback (instructor-authored)

*[Not filled in by this skill.]*
