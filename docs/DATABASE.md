# Database Design --- Supabase PostgreSQL

## 1. Database choice

Use Supabase as the managed PostgreSQL host. The application backend
connects to it using Entity Framework Core and the Npgsql provider.
React must communicate with ASP.NET Core, not directly with the
database.

Keep schema changes in EF Core migrations or another single, documented
migration workflow. Avoid making untracked production-only schema
changes.

## 2. Proposed entities

  -----------------------------------------------------------------------
  Entity                  Purpose                 Important fields /
                                                  relationships
  ----------------------- ----------------------- -----------------------
  `users`                 Common identity and     `id`, `full_name`,
                          role                    `email` unique, `role`,
                                                  `is_active`, timestamps

  `students`              Student profile         `id`, `user_id` unique
                                                  FK,
                                                  `registration_number`
                                                  unique, department

  `advisors`              Advisor profile         `id`, `user_id` unique
                                                  FK, department,
                                                  designation

  `academic_sessions`     Academic period         `id`, unique name,
                                                  start/end dates, status

  `groups`                FYP group               `id`, session FK,
                                                  formation method,
                                                  status, creator,
                                                  approval metadata

  `group_members`         Group membership        group FK, student FK,
                                                  membership status,
                                                  joined/removed
                                                  timestamps

  `advisor_assignments`   Advisor assignment      group FK, advisor FK,
                          history                 assigning admin,
                                                  assigned/ended
                                                  timestamps

  `projects`              Project details         group FK unique, title,
                                                  description,
                                                  objectives,
                                                  methodology, status

  `milestones`            Project milestones      project FK, title, due
                                                  date, status,
                                                  completion timestamp

  `submissions`           Uploaded document       project FK, milestone
                          versions                FK nullable, submitter,
                                                  type, version, storage
                                                  key, metadata, status

  `reviews`               Advisor review of       submission FK, advisor
                          submission              FK, decision, comments,
                                                  reviewed timestamp

  `rubric_criteria`       Evaluation criteria     name, description,
                                                  maximum marks, weight,
                                                  session/configuration

  `evaluations`           Project evaluation      project FK, submission
                                                  FK, advisor FK, status,
                                                  feedback,
                                                  submitted/published
                                                  timestamps

  `evaluation_scores`     Per-criterion score     evaluation FK,
                                                  criterion FK, awarded
                                                  marks, comments

  `notifications`         User notifications      user FK, title,
                                                  message, type,
                                                  read/created timestamps

  `guideline_documents`   Official assistant      title, storage key,
                          knowledge sources       version, uploader,
                                                  active flag

  `audit_logs`            Sensitive-action        actor, action, entity
                          history                 type/id, structured
                                                  details, timestamp
  -----------------------------------------------------------------------

Names and fields are a starting design. Review them against
implementation and instructor requirements before finalizing migrations.

## 3. Relationships

-   One user has one student profile or one advisor profile, depending
    on role.
-   One academic session has many groups.
-   A group has many membership records and may have historical advisor
    assignments.
-   A group has one project in the initial design.
-   A project has many milestones and submissions.
-   A submission has many review records over time.
-   An evaluation has many criterion scores.
-   A user has many notifications.
-   Audit logs reference the actor when available and record the target
    entity.

## 4. Constraints and integrity

-   Unique user email and student registration number.
-   Unique `(group_id, student_id)` membership pair, or equivalent
    membership-history rules.
-   Prevent a student from having multiple active FYP group memberships
    in the same session. Enforce this in transactional service logic
    and, where feasible, with a database-supported constraint strategy.
-   One active advisor assignment per group by default; retain previous
    assignments.
-   Unique project `group_id` if the project model is one project per
    group.
-   Unique submission version per project and submission type, or
    another clearly defined versioning key.
-   Unique `(evaluation_id, criterion_id)` score.
-   Marks must be non-negative and not exceed the criterion maximum.
-   Due dates and session start/end dates must be validated.
-   Use foreign keys and indexes for frequently filtered foreign keys
    and status/date fields.

PostgreSQL partial unique indexes can enforce some active-record rules.
Where EF Core migrations do not express a rule cleanly, use a carefully
reviewed SQL migration and document it.

## 5. IDs and timestamps

Use UUIDs (`Guid` in C# / PostgreSQL `uuid`) for primary keys if that
fits the implementation. Use UTC timestamps for stored events. Keep
academic dates and local display time zones explicit.

## 6. File storage

Do not store large PDF/DOCX file contents directly in ordinary
relational rows unless there is a specific requirement. Store files in
private object storage and keep metadata in `submissions`: - generated
`storage_key` - original filename (display only; never use as a storage
path) - content type - file size - checksum if needed - uploader and
timestamp - version and status

Validate allowed content types and file signatures where practical, set
a size limit, and authorize every download. Avoid public buckets for
student reports.

## 7. Evaluations and marks

-   Store criterion-level marks in `evaluation_scores`.
-   Calculate the total on the server from validated scores.
-   A client must not be trusted to supply the final total.
-   Draft evaluations can be edited by the assigned advisor.
-   Submitted evaluations follow a controlled publication process.
-   Published marks are visible to students only after publication.
-   Corrections to published marks require an authorized, audited
    workflow.

## 8. Audit and retention

Audit group approval, membership changes, advisor
assignment/reassignment, review decisions, evaluation submission, result
publication, and administrative corrections. Do not put passwords,
tokens, or unnecessary personal data in audit details.

## 9. ER diagram

Create the ER diagram after the entity names and relationships are
agreed and migrations reflect the design. Keep the diagram in this file
or in a separate image/source file referenced here; do not allow the
diagram to drift from the actual schema.
