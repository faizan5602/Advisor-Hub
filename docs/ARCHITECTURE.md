# System Architecture

## 1. High-level design

The application uses a React frontend, an ASP.NET Core Web API, Entity
Framework Core, and a Supabase-hosted PostgreSQL database.

``` text
Browser
  React + TypeScript + Tailwind
          |
          | HTTPS / JSON
          v
ASP.NET Core Web API
  Controllers / endpoints
          |
          v
Application services
  Authorization + business rules
          |
          v
Entity Framework Core + Npgsql
          |
          v
Supabase PostgreSQL

File upload/download flow:
Browser -> ASP.NET Core authorization and validation -> private file storage
```

The browser must not connect directly to PostgreSQL. Database
credentials and privileged storage credentials stay on the server.

## 2. Backend responsibilities

-   Authenticate requests and identify the current user.
-   Enforce role and resource-level authorization.
-   Validate DTOs and business rules.
-   Coordinate database operations and transactions.
-   Store submission metadata and maintain version history.
-   Authorize file upload/download operations.
-   Calculate evaluation totals from validated criterion scores.
-   Publish results only through an authorized workflow.
-   Log important events without recording passwords, tokens, or
    unnecessary sensitive content.

## 3. Suggested backend layers

-   **Controllers / endpoints:** HTTP handling, input binding, response
    status codes.
-   **DTOs:** request and response shapes; do not expose database
    entities directly.
-   **Services:** business rules such as group approval, assignment,
    reviews, and publication.
-   **Data layer:** EF Core `DbContext`, entity configurations, and
    migrations.
-   **Authorization:** policies and resource checks for admin, student,
    and advisor access.
-   **Infrastructure:** file storage, email/notification delivery, AI
    provider integration, and logging.

Avoid adding repositories or abstractions that do not solve a real
project need. A clear service plus EF Core is adequate for an academic
project.

## 4. Request flow example

1.  Student submits a proposal file from React.
2.  API authenticates the request and confirms the student belongs to
    the project group.
3.  API validates file type, size, and submission stage.
4.  File is stored privately using a generated storage key.
5.  Submission metadata and version number are saved in PostgreSQL.
6.  API returns a submission summary.
7.  Assigned advisor receives a notification and can review the
    submission.

If the database save fails after file upload, handle cleanup or mark the
file for reconciliation so orphaned files do not accumulate.

## 5. Deployment components

-   **Frontend:** Vercel, Netlify, or another static frontend host.
-   **API:** a hosting provider that supports the selected .NET version.
-   **Database:** Supabase PostgreSQL.
-   **Files:** private object storage or server-controlled file storage.
-   **Secrets:** hosting-provider environment variables or secret
    manager.

Use HTTPS, production-specific environment variables, database
migrations, and restricted database credentials. Confirm current host
limits and pricing before selecting deployment providers.

## 6. Security boundaries

-   All business authorization is enforced by ASP.NET Core.
-   The frontend may hide unavailable actions for usability, but this is
    not a security control.
-   Students cannot query or modify other groups' records by changing an
    ID.
-   Advisors cannot review groups outside their active assignment.
-   Only authorized admins can approve groups, assign advisors,
    configure official deadlines, and publish results.
-   Files remain private; downloads require an authorization check.
-   The AI assistant must not expose private project records or marks to
    unauthorized users.

## 7. Observability and error handling

-   Use structured server-side logs.
-   Return consistent validation and error responses.
-   Never return stack traces or connection strings to the browser.
-   Log sensitive administrative actions with actor, action, target, and
    timestamp.
-   Do not log passwords, authentication tokens, API keys, or full
    uploaded document contents.
