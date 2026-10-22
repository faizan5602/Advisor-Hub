# API Contract (Proposed)

These routes are a planning contract, not a claim that the endpoints
already exist. Adjust this file when implementation decisions change.

## Conventions

-   Base path: `/api`
-   Transport: HTTPS with JSON for normal requests.
-   Use DTOs for requests and responses.
-   Use pagination for list endpoints.
-   Return consistent validation errors and appropriate HTTP status
    codes.
-   Enforce role and resource authorization on the server.
-   For file uploads, use `multipart/form-data` or a protected upload
    workflow.
-   Do not return internal storage keys or secrets unless a server-side
    operation specifically requires them.

## Authentication

  -------------------------------------------------------------------------------------------
  Method            Route                 Purpose           Access
  ----------------- --------------------- ----------------- ---------------------------------
  POST              `/api/auth/login`     Sign in           Public

  POST              `/api/auth/refresh`   Refresh session   Authenticated/session-dependent
                                          if supported      

  POST              `/api/auth/logout`    End session       Authenticated
  -------------------------------------------------------------------------------------------

The authentication mechanism (JWT, secure cookies, or an identity
provider) must be chosen and documented before implementation. Store
password hashes only if the application itself manages passwords; never
store plaintext passwords.

## Admin

  ---------------------------------------------------------------------------------------------
  Method                  Route                                         Purpose
  ----------------------- --------------------------------------------- -----------------------
  GET                     `/api/admin/students`                         Search/list students

  POST                    `/api/admin/students`                         Create student
                                                                        account/profile

  PATCH                   `/api/admin/students/{id}`                    Update or deactivate
                                                                        student

  GET                     `/api/admin/advisors`                         List advisors

  POST                    `/api/admin/advisors`                         Create advisor
                                                                        account/profile

  POST                    `/api/admin/groups`                           Create a group

  POST                    `/api/admin/groups/{id}/approve`              Approve a proposed
                                                                        group

  POST                    `/api/admin/groups/{id}/request-changes`      Request proposal
                                                                        changes

  POST                    `/api/admin/groups/{id}/reject`               Reject a proposed group

  PUT                     `/api/admin/groups/{id}/advisor`              Assign or reassign
                                                                        advisor

  POST                    `/api/admin/results/{evaluationId}/publish`   Publish finalized
                                                                        evaluation

  GET                     `/api/admin/audit-logs`                       Review permitted audit
                                                                        records
  ---------------------------------------------------------------------------------------------

## Groups and projects

  --------------------------------------------------------------------------------------------
  Method            Route                             Purpose           Access
  ----------------- --------------------------------- ----------------- ----------------------
  POST              `/api/groups/proposals`           Propose a group   Student

  GET               `/api/groups/{id}`                View permitted    Authorized
                                                      group details     member/advisor/admin

  POST              `/api/groups/{id}/members`        Invite/add member Authorized user
                                                      under policy      

  GET               `/api/projects/{id}`              View project      Group member/assigned
                                                                        advisor/admin

  PATCH             `/api/projects/{id}`              Update permitted  Authorized group
                                                      project fields    member/admin,
                                                                        according to stage

  GET               `/api/projects/{id}/milestones`   List milestones   Authorized user

  POST              `/api/projects/{id}/milestones`   Add internal      Assigned
                                                      milestone         advisor/admin,
                                                                        according to policy
  --------------------------------------------------------------------------------------------

## Submissions and reviews

  ------------------------------------------------------------------------------------------------
  Method            Route                              Purpose                   Access
  ----------------- ---------------------------------- ------------------------- -----------------
  POST              `/api/projects/{id}/submissions`   Upload a                  Approved group
                                                       proposal/progress/final   member
                                                       report                    

  GET               `/api/projects/{id}/submissions`   List submission history   Group
                                                                                 member/assigned
                                                                                 advisor/admin

  GET               `/api/submissions/{id}`            View submission metadata  Authorized user

  GET               `/api/submissions/{id}/download`   Download file after       Authorized user
                                                       authorization             

  POST              `/api/submissions/{id}/reviews`    Record advisor decision   Assigned advisor
                                                       and feedback              

  POST              `/api/projects/{id}/evaluations`   Create draft evaluation   Assigned advisor

  PUT               `/api/evaluations/{id}`            Update draft criterion    Assigned advisor
                                                       scores                    

  POST              `/api/evaluations/{id}/submit`     Submit evaluation         Assigned advisor

  GET               `/api/projects/{id}/results`       View result when          Authorized user
                                                       permitted/published       
  ------------------------------------------------------------------------------------------------

## Notifications and assistant

  --------------------------------------------------------------------------------
  Method                  Route                            Purpose
  ----------------------- -------------------------------- -----------------------
  GET                     `/api/notifications`             List current user's
                                                           notifications

  PATCH                   `/api/notifications/{id}/read`   Mark own notification
                                                           as read

  POST                    `/api/assistant/chat`            Ask a portal-help
                                                           question
  --------------------------------------------------------------------------------

The assistant endpoint must authenticate the user when private context
is involved, use approved knowledge sources, and avoid exposing other
users' records.

## HTTP behavior

-   `200 OK`: successful read or update with a response.
-   `201 Created`: resource created.
-   `204 No Content`: successful operation with no response body.
-   `400 Bad Request`: invalid input.
-   `401 Unauthorized`: missing or invalid authentication.
-   `403 Forbidden`: authenticated but not permitted.
-   `404 Not Found`: resource absent or deliberately concealed by
    authorization policy.
-   `409 Conflict`: conflicting state, such as duplicate active
    membership.
-   `413 Payload Too Large`: upload exceeds configured size.
-   `422 Unprocessable Entity`: optional convention for semantically
    invalid input.
-   `500 Internal Server Error`: unexpected failure; do not expose stack
    traces.

## Implementation checklist

For each endpoint document: - request DTO and validation - response
DTO - authentication requirement - role/resource authorization -
state-transition rules - database transaction needs - expected error
responses - tests
