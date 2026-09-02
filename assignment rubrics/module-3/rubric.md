# Grading Rubric — Music Label API (Module 3)

**Source:** assigment specs/module-3-assignment.md
**Generated:** 2026-09-02

---

## Functional requirements

### A. EF Core Code First — schema & seeding

- [ ] **MusicLabel entity** — *Mechanically verifiable: table exists with `Id` (PK, autoincrement), `Name`, `Description`.* Evidence:
- [ ] **Album entity** — *Mechanically verifiable: table exists with `Id` (PK, autoincrement), `Title`, `Genre` (string), `ReleaseYear`, `LabelId` (FK), `CoverImage` (string/URL).* Evidence:
- [ ] **Artist entity** — *Mechanically verifiable: table exists with `Id` (PK, autoincrement), `FullName`, `StageName`, `Picture`, `Biography`.* Evidence:
- [ ] **Artist ↔ Album is many-to-many** — *Mechanically verifiable: a join table (or shadow join entity via EF Core) links Artist and Album, allowing one album to have many artists and one artist to appear on many albums.* Evidence:
- [ ] **Album → MusicLabel is one-to-many, via `LabelId`** — *Mechanically verifiable: `Album.LabelId` is a FK to `MusicLabel.Id`; a label can have multiple albums, an album has exactly one label.* Evidence:
- [ ] **`StageName` is nullable** — *Mechanically verifiable: schema check — brief says "if applicable," so the column must allow NULL.* Evidence:
- [ ] **No `NVARCHAR(MAX)` / `VARCHAR(MAX)` columns anywhere** — *Mechanically verifiable: query `INFORMATION_SCHEMA.COLUMNS`, confirm no string column uses `MAX` length.* Evidence:
- [ ] **Seed: at least 3 Music Labels** — *Mechanically verifiable: `SELECT COUNT(*) FROM MusicLabels` ≥ 3.* Evidence:
- [ ] **Seed: at least 6 Albums, distributed among labels** — *Mechanically verifiable: `SELECT COUNT(*) FROM Albums` ≥ 6, AND albums span more than one distinct `LabelId` (not all 6+ under a single label).* Evidence:
- [ ] **Seed: at least 10 Artists** — *Mechanically verifiable: `SELECT COUNT(*) FROM Artists` ≥ 10.* Evidence:

### B. Web API — Generic CRUD

- [ ] **MusicLabel: Create** — *Mechanically verifiable: `POST` endpoint creates a new label row.* Evidence:
- [ ] **MusicLabel: Read (list + by id)** — *Mechanically verifiable: `GET` endpoints return all labels and a single label by id.* Evidence:
- [ ] **MusicLabel: Update** — *Mechanically verifiable: `PUT`/`PATCH` endpoint persists a change to an existing label.* Evidence:
- [ ] **MusicLabel: Delete** — *Mechanically verifiable: `DELETE` endpoint removes the label row.* Evidence:
- [ ] **Album: Create / Read / Update / Delete** — *Mechanically verifiable: same four checks as above, against the Albums endpoints.* Evidence:
- [ ] **Artist: Create / Read / Update / Delete** — *Mechanically verifiable: same four checks as above, against the Artists endpoints.* Evidence:
- [ ] **Deleting a MusicLabel does not delete its Albums** — *Mechanically verifiable: delete a label that has ≥1 album; afterward, those album rows still exist with `LabelId = NULL`.* Evidence:
- [ ] **Deleting an Artist does not delete Albums they're featured on** — *Mechanically verifiable: delete an artist linked to ≥1 album; afterward, those album rows still exist, and the join-table row(s) for that artist are gone (artist removed from the album's artist list, not the album itself).* Evidence:

### C. Web API — Updating related data

- [ ] **Update artists on an album** — *Mechanically verifiable: dedicated endpoint takes an `int[]` of artist IDs in the body and an Album ID in the path; after the call, the album's artist set equals exactly the given IDs.* Evidence:
- [ ] **Update albums on a music label** — *Mechanically verifiable: dedicated endpoint takes an `int[]` of album IDs in the body and a Label ID in the path; after the call, those albums' `LabelId` equals the given label.* Evidence:

### D. Web API — Reporting

- [ ] **Report 1: All albums for a specific label** — *Mechanically verifiable: endpoint returns only albums where `LabelId` matches the given label.* Evidence:
- [ ] **Report 2: All artists featured in a specific album** — *Mechanically verifiable: endpoint returns exactly the artists linked to the given album via the join table.* Evidence:
- [ ] **Report 3: All artists under a specific label** — *Mechanically verifiable: endpoint returns the distinct set of artists featured on any album belonging to the given label.* Evidence:

### E. DTOs with AutoMapper

- [ ] **MusicLabel has distinct Create/Update/Read DTOs** — *Mechanically verifiable: grep `Models`/`DTOs` folder for label-specific request/response types, not the raw entity.* Evidence:
- [ ] **Album has distinct Create/Update/Read DTOs** — *Mechanically verifiable: same check for Album.* Evidence:
- [ ] **Artist has distinct Create/Update/Read DTOs** — *Mechanically verifiable: same check for Artist.* Evidence:
- [ ] **AutoMapper (or equivalent) used for entity↔DTO mapping** — *Mechanically verifiable: `AutoMapper` package referenced, mapping profiles defined.* Evidence:
- [ ] **DTO mapping is encapsulated in controllers; services/repositories return domain objects** — *Mechanically verifiable: grep service/repository method signatures — return types are entities, not DTOs; DTO conversion happens in controller action bodies.* Evidence:

### F. Swagger / documentation

- [ ] **`///` XML summary on every public method (functionality, exceptions, return data)** — *Mechanically verifiable: grep public methods/controller actions; each has a `<summary>` block.* Evidence:
- [ ] **Swagger annotations present** — *Mechanically verifiable: grep for `[ProducesResponseType]`, `[SwaggerOperation]` or equivalent on controller actions.* Evidence:
- [ ] **Swagger/OpenAPI UI is enabled and reachable** — *Mechanically verifiable: `AddSwaggerGen`/`UseSwaggerUI` (or equivalent) configured in `Program.cs`/`Startup.cs`; `/swagger` endpoint responds.* Evidence:

### G. Services / Repositories

- [ ] **At least a Service layer exists between controllers and `DbContext`** — *Mechanically verifiable: grep for Service classes/interfaces used by controllers instead of direct `DbContext` calls in controller actions.* Evidence:
- [ ] **Services (and Repositories, if used) are registered for DI and injected, not `new`'d** — *Mechanically verifiable: grep `builder.Services.Add...` registrations, and confirm controllers receive them via constructor injection.* Evidence:

---

## Stretch — Optional Advanced Requirements

> These are explicitly optional in the brief. Grade only if the student attempted them; absence should not cost core-requirement points.

- [ ] **Conditional inclusion of related data** — *Mechanically verifiable: `GET /api/albums/{id}` without `?includeArtists=true` (or `X-Include-Artists` header) omits the `artists` key entirely; with it, the response includes an `artists` array matching the album's actual linked artists (id + fullName).* Evidence:
- [ ] **Soft delete for Artists and Albums** — *Mechanically verifiable: entities have a `Deleted` flag; a "deleted" row is excluded from standard `GET` responses; `?includeDeleted=true` returns it; `POST /api/artists/{id}/restore` clears the flag and the artist reappears in standard responses.* Evidence:
- [ ] **Bulk import** — *Mechanically verifiable: `POST /api/albums/bulk` accepts an array payload, creates one row per valid item, and rejects/reports invalid items rather than inserting them.* Evidence:
- [ ] **Pagination and sorting** — *Mechanically verifiable: list endpoint accepts `?page=&limit=&sort=`; response contains `data[]` plus a `pagination` object with `currentPage`, `totalPages`, `totalItems`; `totalItems` matches the actual row count and `totalPages` = `ceil(totalItems / limit)`.* Evidence:

---

## Testing

Not applicable — this assignment brief has no Testing/TDD requirement.

---

## Submission / process

- [ ] **README present and describes the project (setup, run instructions)** — *Mechanically verifiable: `README.md` exists at repo root with non-trivial content.* Evidence:
- [ ] **Commit history shows real incremental work, not one lump commit** — *Mechanically verifiable: `git log` shows multiple commits across the build, not a single "initial commit."* Evidence:
- [ ] **Database diagram included** — *Optional, not scored — only if created.*
- [ ] **Repository is private** — *Instructor-only, not checkable from code.*
- [ ] **Lecturer added as Maintainer** — *Instructor-only, not checkable from code.*

---

## Code quality (instructor judgment — not scored by an agent)

Pointers to look at — not a checklist, not a score:
- README: is it actually well-structured (clear setup/run instructions, project overview), not just present?
- Commit messages and branch organization: are messages descriptive and is there sensible branch usage, or is it all pushed straight to `main`?
- Service/Repository layer: does it meaningfully reduce controller bulk, or is it a thin pass-through wrapper around `DbContext`?
- Nullability and string-length choices on entity properties: are they reasoned (e.g., `Description nvarchar(500)`), not just "whatever compiles"?
- Swagger/XML doc content: does it *accurately* describe behavior, exceptions, and return values, not just exist?
- General REST hygiene: correct status codes (201 on create, 404 on missing, etc.), consistent route naming.

---

## Overall feedback (instructor-authored)

*[Not filled in by this skill.]*
