# Local Development Setup

This guide is a template. Confirm actual versions and commands against
the repository before treating them as final.

## Prerequisites

-   Git
-   Node.js LTS and npm
-   .NET SDK version selected by the project
-   Supabase project with PostgreSQL access
-   IDE/editor such as Visual Studio or VS Code

## Suggested repository layout

``` text
fyp-management-system/
├── agent.md
├── CONTRIBUTING.md
├── frontend/
├── backend/
│   └── FypManagement.Api/
├── docs/
└── README.md
```

Preserve the current repository structure if it already exists; do not
move files solely to match this example.

## Frontend

1.  Open a terminal in `frontend/`.
2.  Install dependencies using the package manager and lockfile already
    in the repository.
3.  Create a local environment file for public frontend configuration
    only.
4.  Run the existing development script from `package.json`.

Do not place database passwords, privileged Supabase keys, or AI
provider secrets in frontend environment variables. Frontend environment
values are generally visible to users after bundling.

## Backend

1.  Open a terminal in the backend project directory.
2.  Restore NuGet packages.
3.  Configure server-side environment variables or user secrets.
4.  Apply EF Core migrations only after reviewing the target database
    and migration.
5.  Run the API and verify its health endpoint, if implemented.

Typical commands, depending on the project setup:

``` bash
dotnet restore
dotnet build
dotnet test
dotnet run
```

Do not run a database migration against production until it has been
reviewed and a backup/recovery plan is available.

## Supabase PostgreSQL connection

-   Retrieve the appropriate PostgreSQL connection details from the
    Supabase project settings.
-   Prefer the connection option suitable for the hosting environment;
    connection pooling may be relevant for serverless or
    high-concurrency deployments.
-   Store the connection string in a server-side secret/environment
    variable such as `ConnectionStrings__DefaultConnection`.
-   Configure EF Core to use Npgsql.
-   Never commit connection strings containing credentials.
-   Use a least-privilege database role where practical.

The exact connection string and SSL settings depend on the connection
method and deployment environment. Use current Supabase documentation
rather than copying credentials into source code.

## Verification checklist

-   Frontend starts without errors.
-   Backend builds and starts.
-   API can connect to the intended Supabase database.
-   Migrations apply to a development database.
-   Login and role authorization work.
-   Unauthorized users cannot access another group's project or files.
-   Upload validation and protected download work.
-   No secrets appear in Git history or browser bundles.
