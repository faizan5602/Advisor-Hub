# Coding Agent Instructions --- FYP Management System

You are working on a university Final Year Project (FYP) Management
System. Read the relevant documentation before making changes. Follow
the existing project structure and avoid introducing unnecessary
complexity.

## 1. Project goals

Build a secure, responsive portal for three roles: - **Admin:** manage
students, advisors, groups, academic sessions, official deadlines,
advisor assignments, guidelines, and result publication. - **Advisor:**
manage only assigned groups, review submissions, give feedback, set
internal milestones, and evaluate projects. - **Student:** view
group/project information, propose groups, submit documents, track
progress and feedback, and view published results.

The system is intended for a DSA lab course. Use C# and ASP.NET Core for
the backend so the project demonstrates real backend and data-structure
concepts.

## 2. Required technology architecture

-   **Frontend:** React + TypeScript + Tailwind CSS.
-   **Backend:** ASP.NET Core Web API in C#.
-   **ORM / database driver:** Entity Framework Core + Npgsql PostgreSQL
    provider.
-   **Database:** Supabase-hosted PostgreSQL.
-   **File storage:** private storage for PDF/DOCX submissions. Prefer a
    protected storage service or server-controlled upload flow.
-   **Deployment target:** frontend on Vercel or equivalent; ASP.NET
    Core API on a compatible .NET host; database on Supabase.

The browser must call the ASP.NET Core API for application data. Do not
connect React directly to PostgreSQL. Do not expose database passwords,
privileged Supabase keys, or other secrets to the browser.

## 3. Documentation map

Read these files as appropriate before implementing a feature:

-   [Project requirements](docs/PROJECT_REQUIREMENTS.md) --- scope,
    roles, requirements, permissions, and workflows.
-   [Architecture](docs/ARCHITECTURE.md) --- components, request flow,
    security boundaries, and deployment.
-   [Database design](docs/DATABASE.md) --- entities, relationships,
    constraints, and persistence decisions.
-   [DSA implementation plan](docs/DSA_IMPLEMENTATION.md) --- data
    structures, algorithms, complexity, and lab demonstrations.
-   [API contract](docs/API.md) --- proposed routes, authorization, and
    response conventions.
-   [Local setup](docs/SETUP.md) --- prerequisites and environment
    configuration.
-   [Deployment](docs/DEPLOYMENT.md) --- production configuration and
    hosting checklist.
-   [AI assistant](docs/AI_ASSISTANT.md) --- portal-help assistant
    scope, grounding, and privacy.
-   [Contribution workflow](CONTRIBUTING.md) --- branches, code quality,
    and pull-request expectations.

`README.md` is intentionally maintained separately as the user-facing
project overview. Do not replace it with this file.

## 4. Non-negotiable engineering rules

1.  Inspect the current codebase before creating or renaming files.
    Preserve useful existing structure.
2.  Do not invent endpoints, database columns, environment variables,
    package versions, or business rules. Check the code and docs first;
    update documentation when a decision changes.
3.  Enforce authentication and authorization on the server for every
    protected operation. Hiding a button in React is not security.
4.  Apply least privilege. Students may access their own and their
    approved group's data; advisors may access only currently assigned
    groups; admins may manage academic configuration.
5.  Validate all input on the server. Use DTOs rather than binding
    database entities directly to incoming requests.
6.  Keep secrets in environment variables or secret configuration. Never
    commit `.env` files, connection strings with passwords, API keys,
    tokens, or uploaded student documents.
7.  Use parameterized database operations through EF Core. Do not build
    SQL queries by concatenating user input.
8.  Use database constraints and transactions for important multi-step
    actions such as group approval, advisor reassignment, and result
    publication.
9.  Store uploaded files privately. Validate allowed file types and
    size, generate safe storage keys, and authorize every download.
10. Never silently change finalized marks. Corrections must be
    authorized and auditable.
11. Avoid fake statistics, sample credentials that look real, and
    fabricated policy or deadline information in the UI.
12. Prefer clear, maintainable code and simple solutions suitable for a
    student project. Do not add a new dependency without a clear reason.

## 5. Workflow for coding tasks

Before coding: 1. Read this file and the relevant documentation. 2.
Inspect related components, API endpoints, models, migrations, and
tests. 3. State the smallest sensible implementation plan if the task
spans multiple files. 4. Identify security, data integrity, and
backward-compatibility concerns.

While coding: 1. Follow existing naming and formatting conventions. 2.
Keep controllers thin; put business rules in services where appropriate.
3. Use async database calls and cancellation tokens where practical. 4.
Return appropriate HTTP status codes and consistent error responses. 5.
Add or update validation, tests, and documentation alongside the
feature. 6. Never report that a command or test passed unless it was
actually run and passed.

After coding: 1. Run relevant build, test, lint, or type-check commands
available in the repository. 2. Review the diff for secrets, unrelated
changes, and missing authorization. 3. Summarize changed files,
behavior, tests run, and any remaining limitations. 4. If something
could not be verified, say so clearly.

## 6. Important business rules

-   A student cannot belong to multiple active FYP groups in the same
    academic session.
-   Group formation supports both admin-created groups and
    student-proposed groups.
-   Group proposals must be reviewed before becoming approved/active.
-   Each active group has one assigned advisor by default; preserve
    assignment history when reassigned.
-   Advisors can review and evaluate only groups assigned to them.
-   Admins define official academic sessions and deadlines. Advisors can
    set internal milestones and revision deadlines within those limits,
    and request official deadline changes from an admin.
-   Submission versions and review history must be preserved.
-   Marks are not visible to students until published.
-   Total evaluation marks should be calculated from criterion-level
    scores, not trusted from a client-supplied total.
-   Group, project, submission, and evaluation statuses are separate
    concepts. Do not collapse them into one status field.

## 7. DSA lab expectations

Use C# for meaningful demonstrations such as: - Dictionary/hash-based
lookup by registration number. - Sorting/filtering projects and
students. - Queue-based notification processing if implemented. -
Priority queue for ordering overdue/upcoming deadlines if implemented.

Use built-in .NET collections for production-quality application code.
If the instructor requires implementing a structure manually, keep that
implementation isolated and document its use and complexity. Do not add
a graph or other structure solely for decoration.

## 8. When requirements conflict

Treat explicit user instructions and confirmed project decisions as
authoritative. If code and documentation disagree, inspect current
behavior, identify the inconsistency, and update the affected
documentation. Ask a concise clarification only when a consequential
requirement cannot be safely inferred.
