# Deployment Guide

## Deployment architecture

-   **Frontend:** Vercel, Netlify, or a comparable React hosting
    service.
-   **Backend:** a host that supports the selected ASP.NET Core/.NET
    runtime.
-   **Database:** Supabase PostgreSQL.
-   **Files:** private object storage or another protected file service.

Confirm current pricing, free-tier limits, sleeping/idle behavior,
bandwidth, build limits, and supported .NET versions before choosing
providers.

## Environment configuration

Configure production secrets through the hosting provider: - PostgreSQL
connection string - authentication signing or identity-provider
settings, if applicable - file storage credentials - AI provider key, if
the assistant uses a hosted model - allowed frontend origins / CORS
configuration - logging and monitoring settings

Never commit production `.env` files or put server-only secrets in
frontend variables.

## Database release

1.  Review migrations and schema changes.
2.  Back up or establish a recovery plan for important data.
3.  Apply migrations to a staging database first.
4.  Verify constraints, indexes, and authorization-related data.
5.  Apply production migrations through a controlled release process.
6.  Confirm the API uses the intended production database.

## API release

-   Configure HTTPS and allowed CORS origins.
-   Set a health check if supported.
-   Use structured logs without secrets.
-   Set appropriate upload limits and request timeouts.
-   Verify authorization policies and protected file access.
-   Ensure errors returned to users do not reveal stack traces or
    database details.

## Frontend release

-   Configure the production API base URL.
-   Check that no private keys or connection strings are included in the
    build.
-   Test responsive layouts and error states.
-   Verify login, role-specific navigation, submissions, and published
    results.

## Post-deployment smoke tests

-   Login as an admin, student, and advisor test account.
-   Confirm each role sees only authorized features and data.
-   Attempt unauthorized access to another group's project and file.
-   Submit and review a test document.
-   Confirm marks remain hidden until publication.
-   Check logs and error reporting.
-   Remove or disable test accounts and test data when no longer needed.
