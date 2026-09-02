# Grading Rubric — Stream Flix Heroes (Module 2)

**Source:** assigment specs/module-2-assignment.md
**Generated:** 2026-09-02

---

## Functional requirements

### A. SQL Scripts — RPGHeroesDb (Appendix A)

- [ ] **`01_dbCreate.sql` creates the database** — *Mechanically verifiable: run the script against a fresh SQL Server instance; confirm a database named `RPGHeroesDb` exists afterward.* Evidence:
- [ ] **`02_tableCreate.sql` — Hero table schema** — *Mechanically verifiable: query `INFORMATION_SCHEMA.COLUMNS`/`sys.columns` for `dbo.Hero`; confirm `Id` (int, identity, PK), `Name`, `Class`, `Level` exist.* Evidence:
- [ ] **`02_tableCreate.sql` — Apprentice table schema** — *Mechanically verifiable: same check for `Apprentice`: `Id` (int, identity, PK), `Name`, `MentorId` exist.* Evidence:
- [ ] **`02_tableCreate.sql` — Skill table schema** — *Mechanically verifiable: same check for `Skill`: `Id` (int, identity, PK), `Name`, `Description` exist.* Evidence:
- [ ] **`03_relationshipHeroApprentice.sql` — FK constraint** — *Mechanically verifiable: query `sys.foreign_keys` for a FK from `Apprentice.MentorId` to `Hero.Id`.* Evidence:
- [ ] **`04_relationshipHeroSkill.sql` — linking table exists** — *Mechanically verifiable: a table exists with two FK columns (referencing `Hero` and `Skill`) and a composite primary key across both.* Evidence:
- [ ] **`04_relationshipHeroSkill.sql` — FK constraints** — *Mechanically verifiable: `sys.foreign_keys` shows two FKs from the linking table to `Hero` and `Skill`.* Evidence:
- [ ] **`05_insertHeroes.sql` — three heroes** — *Mechanically verifiable: after running 01–05 in order, `SELECT COUNT(*) FROM Hero` = 3.* Evidence:
- [ ] **`06_insertApprentices.sql` — three apprentices, linked** — *Mechanically verifiable: `SELECT COUNT(*) FROM Apprentice` = 3, every row's `MentorId` is non-null and matches an existing `Hero.Id`.* Evidence:
- [ ] **`07_skills.sql` — four skills** — *Mechanically verifiable: `SELECT COUNT(*) FROM Skill` = 4.* Evidence:
- [ ] **`07_skills.sql` — skills associated with heroes** — *Mechanically verifiable: linking table has rows after running 07; every pair references an existing Hero and Skill.* Evidence:
- [ ] **`07_skills.sql` — one hero with multiple skills** — *Mechanically verifiable: `GROUP BY HeroId` on the linking table, at least one hero has `COUNT(*) > 1`.* Evidence:
- [ ] **`07_skills.sql` — one skill linked to multiple heroes** — *Mechanically verifiable: `GROUP BY SkillId` on the linking table, at least one skill has `COUNT(*) > 1`.* Evidence:
- [ ] **`08_updateHero.sql` — updates one hero** — *Mechanically verifiable: script contains an `UPDATE` on `Class` or `Level`; after running 01–08 in order, one hero's value differs from what `05_insertHeroes.sql` set.* Evidence:
- [ ] **`09_deleteApprentice.sql` — deletes an apprentice by name, referential integrity intact** — *Mechanically verifiable: script contains `DELETE FROM Apprentice WHERE Name = ...`; after running 01–09 in order, `SELECT COUNT(*) FROM Apprentice` = 2, and no FK violation occurred.* Evidence:
- [ ] **All nine scripts run in order with no errors** — *Mechanically verifiable: execute `01`→`09` sequentially against a fresh database; zero errors.* Evidence:

### B. Repository pattern — StreamFlix console app (Appendix B)

- [ ] **SQL Client library installed** — *Mechanically verifiable: project file references `Microsoft.Data.SqlClient` (or `System.Data.SqlClient`).* Evidence:
- [ ] **Repository abstraction exists** — *Mechanically verifiable: a Repository class/interface exists, separate from `Program.cs`/`Main`, encapsulating the SQL access.* Evidence:
- [ ] **Model classes live in a `Models` folder** — *Mechanically verifiable: a `Models/` folder exists with distinct classes for each data shape needed (user, subscription-count result, most-watched-movie result, most-popular-genre result).* Evidence:
- [ ] **Req 1 — Read all users** — *Mechanically verifiable: method returns all users with `Id`, `FirstName`, `LastName`, `Email`, `SubscriptionType`, `DateJoined` populated.* Evidence:
- [ ] **Req 2 — Read user by Id** — *Mechanically verifiable: method takes an `int id`, queries with a parameterized `WHERE UserId = @id`.* Evidence:
- [ ] **Req 3 — Read user by name (partial match)** — *Mechanically verifiable: method builds a query using `LIKE` with wildcards.* Evidence:
- [ ] **Req 4 — Paged users** — *Mechanically verifiable: method takes page/page-size params and issues `OFFSET ... ROWS FETCH NEXT ... ROWS ONLY` (or equivalent); returns the correct subset.* Evidence:
- [ ] **Req 5 — Add new user (restricted fields)** — *Mechanically verifiable: create method accepts only `FirstName`, `LastName`, `Email`, `SubscriptionType` and inserts a new row.* Evidence:
- [ ] **Req 6 — Update existing user** — *Mechanically verifiable: update method persists a change, confirmed by re-querying after the call.* Evidence:
- [ ] **Req 7 — Users per Subscription Type, descending** — *Mechanically verifiable: query groups by `SubscriptionType` with `COUNT(*)`, `ORDER BY` count `DESC`.* Evidence:
- [ ] **Req 8 — Most watched movies, descending** — *Mechanically verifiable: query joins `WatchHistory`→`Movies`, groups by movie, orders by watch count `DESC`.* Evidence:
- [ ] **Req 9 — Most popular genre per user, with titles** — *Mechanically verifiable: for a given `UserId`, groups that user's `WatchHistory` by `Movies.Genre`, picks the top genre, returns the titles watched in it.* Evidence:

### C. Method documentation

- [ ] **`///` (or equivalent) summary on every public method, excluding overloads** — *Mechanically verifiable: grep public methods; each non-overload has a `<summary>` block.* Evidence:
- [ ] **Exceptions documented where thrown** — *Mechanically verifiable: methods that throw have an `<exception>` tag naming the type.* Evidence:
- [ ] **Return value documented where applicable** — *Mechanically verifiable: methods that return data have a `<returns>` tag.* Evidence:

---

## Worked-example checks

> Computed from the provided StreamFlix seed data (`assigment specs/streamflix-postgres.txt`). This reference file is written in PostgreSQL syntax while the assignment targets SQL Server / SQL Client — these exact values assume the SQL Server dataset students received seeds identical values to this file. Re-verify against the actual student-facing database before treating a mismatch as a student error.

- [ ] **Req 2 — Get user by Id = 1** — Expected: FirstName "Alice", LastName "Johnson", Email "alice.johnson@example.com", SubscriptionType "Premium". Actual:
- [ ] **Req 3 — Get user(s) by name LIKE "%Smith%"** — Expected: exactly one match — Bob Smith (UserId 2). Actual:
- [ ] **Req 4 — Page of users: page size 5, page 2 (`OFFSET 5 ROWS FETCH NEXT 5 ROWS ONLY`), ordered by UserId ascending** — Expected: UserIds 6–10 → Frank White, Grace Clark, Henry Lewis, Isla Walker, Jack Hall. Actual:
- [ ] **Req 7 — Users per Subscription Type** — Expected: Premium = 10, Basic = 10, Free = 9 (Free last; Premium/Basic are tied for first, either order acceptable). Actual:
- [ ] **Req 8 — Most watched movies** — Expected: MovieIds 1–10 each show a watch count of 1 (tied), all other movies show 0. Note: the seed data is sparse — there's no single distinguishing "most watched" movie, so grade this as a tie check, not a single winner. Actual:
- [ ] **Req 9 — Most popular genre for UserId = 1 (Alice Johnson)** — Expected: genre "Sci-Fi" (3 watches), titles: Inception, The Matrix, Interstellar. Actual:

---

## Testing

Not applicable — this assignment brief has no Testing/TDD requirement.

---

## Submission / process

- [ ] **GitLab repository created, contains all code** — *Mechanically verifiable* — Evidence:
- [ ] **Repository is private** — *Instructor-only, not checkable from code.*
- [ ] **Lecturer added as Maintainer** — *Instructor-only, not checkable from code.*
- [ ] **Submission is the repo link, not the "clone with SSH" link** — *Instructor-only — checked against what was submitted, not the code.*
- [ ] **Only one submission per group; both member names included** — *Instructor-only, not checkable from code.*
- [ ] *(Optional, not scored)* Database relationship diagram included in repo.
- [ ] *(Optional, not scored)* Controller → Service → Adapter architecture implemented.

---

## Code quality (instructor judgment — not scored by an agent)

Pointers to look at — not a checklist, not a score:
- Repository pattern: is the chosen version applied consistently, or does data-access logic leak into `Program.cs`/`Models`?
- SQL: parameterized queries throughout (no string-concatenated SQL), consistent naming, sensible async use if applicable.
- XML documentation: is it *accurate*, not just present — does it genuinely describe behavior, thrown exceptions, and return values?
- Naming, method size, separation of concerns across `Models`/`Repository`/`Program`.
- SQL script hygiene: consistent formatting, safe to re-run, comments where non-obvious.

---

## Overall feedback (instructor-authored)

*[Not filled in by this skill.]*
