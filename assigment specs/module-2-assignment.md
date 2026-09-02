# Stream Flix Heroes

## Create a Database and Access It

### SQL Data Access

Write SQL scripts and build a console application in C#, Java/Kotlin or C++. Follow the guidelines given below, feel free to expand on the functionality. It must meet the minimum requirements prescribed.

**NOTE:** This assignment is to be completed in groups of 2. In addition to this, you should complete this assignment using Pair Programming.

---

## Instructions

### 1) Set up the Development Environment
Make sure you have installed at least the following tools (adapt according to your tech stack):

- Visual Studio 2022 with .NET 6 or greater
- SQL Server Management Studio (SSMS)

### 2) Optional: Database Diagrams
You can use SSMS to draw relationship diagrams of your database. You can include these in your repository.

### 3) Console Application and Database Requirements

#### a) SQL Scripts
Create SQL scripts to create a database according to the specifications in Appendix A.

#### b) Repository Pattern Implementation
Create a console application to access data from the provided StreamFlix database (see Appendix B).

#### c) Method Documentation
Include summary (`///`) tags for each method explaining:
- What the method does
- Any exceptions it can throw
- What data it returns (if applicable)

**Note:** Summary tags are not required for overloaded methods.

### 4) Submission Requirements

#### a) GitLab Repository
- Create a GitLab repository containing all your code.
- Optionally, include the generated class diagram in this repository.
- Ensure the repository is private and add your lecturer as a Maintainer.

#### b) Submission
- Submit only the link to your GitLab repository (not the "clone with SSH" link).
- Only one person from each group needs to submit but include the names of both group members in the submission.

---

## Appendix A: SQL Scripts to Create Database

### 1) Introduction and Overview
Create scripts to:
- Create a database
- Set up tables
- Add relationships
- Populate the tables with data

The database will be centered around RPG heroes and can be called `RPGHeroesDb`.

**REQUIREMENT:** Create a script called `01_dbCreate.sql` to create the database.

### 2) Tables

#### Tables to Create
- **Hero**: `Id` (autoincremented integer), `Name`, `Class`, `Level`
- **Apprentice**: `Id` (autoincremented integer), `Name`, `MentorId` (foreign key linking to Hero)
- **Skill**: `Id` (autoincremented integer), `Name`, `Description`

**REQUIREMENT:** Create a script called `02_tableCreate.sql` to create these tables with primary keys.

### 3) Table Relationships

#### Hero-Apprentice Relationship
- One hero can mentor multiple apprentices.
- One apprentice has one hero as their mentor.

**REQUIREMENT:** Create a script called `03_relationshipHeroApprentice.sql` to:
- Add a foreign key (`MentorId`) to the Apprentice table.
- Configure constraints to enforce this relationship.

#### Hero-Skill Relationship
- One hero can have many skills.
- One skill can belong to many heroes.

**REQUIREMENT:**
- Create a linking table with two foreign keys and a composite primary key.
- Create a script called `04_relationshipHeroSkill.sql` to:
  - Create the linking table.
  - Add foreign key constraints.

### 4) Inserting Data

#### Heroes
**REQUIREMENT:** Create a script called `05_insertHeroes.sql` to insert three heroes.

#### Apprentices
**REQUIREMENT:** Create a script called `06_insertApprentices.sql` to insert three apprentices, linking them to heroes.

#### Skills
**REQUIREMENT:** Create a script called `07_skills.sql` to:
- Insert four skills.
- Associate skills with heroes.
- Demonstrate one hero with multiple skills and one skill linked to multiple heroes.

### 5) Updating Data

**REQUIREMENT:** Create a script called `08_updateHero.sql` to update the class or level of one hero.

### 6) Deleting Data

**REQUIREMENT:** Create a script called `09_deleteApprentice.sql` to delete an apprentice by name. Ensure referential integrity is maintained.

**NOTE:** The scripts should be able to run in order with no errors.

---

## Appendix B: Reading Data with SQL Client

### 1) Introduction and Overview

Use SQL Client to manipulate SQL Server data in Visual Studio. You are provided with a `StreamFlix` database that models a movie service database.

Create a C# console application and:
- Install the required SQL Client library.
- Create a repository (pattern) to interact with the database.

### 2) User Requirements

Implement the following functionalities for Users:

1. Read all users:
   - Display: `Id`, `First Name`, `Last Name`, `Email`, `Subscription Type`, `Date Joined`
2. Read a specific user by `Id`.
3. Read a specific user by name (use `LIKE` for partial matches).
4. Return a page of users using `LIMIT` and `OFFSET`.
5. Add a new users (only fields listed above).
6. Update an existing user.
7. Return the number of users in each Subscription Type (ordered descending).
8. Return the most watched movies (ordered descending)
9. For a given user, return their most popular genre watched (display titles).

**NOTE:**
- Create classes for each data structure (e.g., `User`, `UserReview`, `UserMostWatched`, `UserSubcriptionType`).
- Place these classes in a `Models` folder.

### 3) Repositories

Implement the repository pattern. The version of the pattern is up to you.

(Optional), Implement the Controller -> Service -> Adapter architecture


