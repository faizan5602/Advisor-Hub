# Contributing Guide

## Development principles

-   Keep changes focused and easy to review.
-   Follow the existing architecture and naming conventions.
-   Read `agent.md` and relevant files under `docs/` before implementing
    features.
-   Do not commit secrets, real student data, private documents, or
    local environment files.
-   Prefer small pull requests with clear descriptions.

## Branches

Suggested branch names: - `feature/group-approval` -
`feature/submission-upload` - `fix/advisor-authorization` -
`docs/database-design`

Use descriptive commit messages, for example: -
`feat: add student group proposal workflow` -
`fix: restrict advisor access to assigned projects` -
`docs: clarify evaluation publication rules`

## Before submitting a change

1.  Build the backend and frontend.
2.  Run relevant automated tests and lint/type checks, where configured.
3.  Verify authorization for each role affected by the change.
4.  Check validation, error handling, and database constraints.
5.  Update API and architecture documentation if behavior changes.
6.  Confirm no secrets or private files were added.

## Pull request description

Include: - What changed and why. - Main files or modules affected. - How
to test the change. - Tests/build commands run and their results. -
Known limitations or follow-up work.
