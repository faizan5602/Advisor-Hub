# Project Requirements

## 1. Purpose

The FYP Management System coordinates the Final Year Project lifecycle,
from group formation and proposal review to milestone tracking, final
submission, advisor evaluation, and publication of results.

## 2. User roles

### Admin

-   View system-wide counts and pending work.
-   Create, update, and deactivate student accounts.
-   Create and manage advisor accounts.
-   Create groups or review student-proposed groups.
-   Approve, reject, or request changes to group proposals.
-   Assign and reassign advisors while retaining assignment history.
-   Configure academic sessions, group-size limits, official milestones,
    deadlines, evaluation periods, and rubrics.
-   Monitor projects and overdue submissions.
-   Manage official FYP guidelines used by the portal assistant.
-   Publish finalized marks with an audit trail.
-   Review audit logs for sensitive actions.

### Student

-   Sign in and view a personalized dashboard.
-   Propose a group and invite members, subject to the configured group
    rules.
-   Track group approval status and view active group members.
-   View assigned advisor contact details.
-   Create or update project title, description, objectives, and
    methodology when editing is allowed.
-   Upload proposal, progress, and final-report files as PDF or DOCX.
-   View submission versions, timestamps, status, and advisor feedback.
-   Resubmit revisions when permitted.
-   Track milestones, deadlines, notifications, and progress.
-   View marks only after they are published.
-   Ask the portal assistant questions about approved guidelines,
    submission procedures, and portal usage.

### Advisor

-   View assigned groups and projects only.
-   See pending reviews, incomplete work, and upcoming internal
    deadlines.
-   Review submitted files.
-   Approve submissions, request revisions with comments and a deadline,
    or reject with an explanation.
-   Define internal milestones and feedback/revision deadlines within
    the official academic schedule.
-   Monitor assigned-group progress.
-   Evaluate projects using a configurable rubric.
-   Save draft evaluations and submit finalized marks.
-   View previous reviews and evaluations.
-   Receive notifications about new submissions and review deadlines.

## 3. Core workflows

### Group formation

1.  Admin may create a group directly, or a student may propose one.
2.  Proposed members and group size are validated.
3.  Admin reviews the proposal.
4.  Admin approves, rejects, or requests changes.
5.  An approved group can receive an advisor assignment.
6.  Membership and approval changes are recorded.

### Project and submission

1.  An approved group creates or receives a project record.
2.  The group submits the proposal.
3.  The assigned advisor reviews it and provides a decision and
    feedback.
4.  The group addresses revisions and resubmits when permitted.
5.  The project progresses through configured milestones.
6.  The group submits the final report.
7.  The advisor reviews and evaluates the project.

### Evaluation and results

1.  The advisor scores configured rubric criteria.
2.  The application validates each score against the criterion maximum.
3.  The advisor submits the evaluation.
4.  An authorized admin publishes the finalized result.
5.  The student can view the result after publication.
6.  Any correction to finalized marks must be authorized and audited.

## 4. Functional requirements

-   Authentication and role-based access.
-   Student/advisor account administration.
-   Academic-session configuration.
-   Admin-created and student-proposed groups.
-   Group membership validation and approval.
-   Advisor assignment and reassignment history.
-   Project details and status tracking.
-   Direct PDF/DOCX upload with version history.
-   Advisor review decisions and feedback.
-   Milestone and deadline tracking.
-   Rubric-based evaluation.
-   Controlled result publication.
-   In-app notifications.
-   Search, sorting, and filtering.
-   Portal-help assistant grounded in approved documentation.
-   Audit logs for sensitive actions.

## 5. Non-functional requirements

-   **Security:** server-side authorization, password/authentication
    best practices, input validation, private file storage, and secret
    management.
-   **Integrity:** foreign keys, unique constraints, transactions, and
    validated state transitions.
-   **Usability:** responsive layouts, accessible forms, clear statuses,
    and useful validation messages.
-   **Performance:** pagination for large lists, async I/O, and
    efficient database queries.
-   **Maintainability:** separation of API, business logic, persistence,
    and UI concerns.
-   **Reliability:** clear errors, logging without secrets, and
    recoverable upload/review workflows.
-   **Auditability:** preserve relevant history for assignments,
    reviews, evaluations, and publication.
-   **Privacy:** users can access only information appropriate to their
    role and group assignment.

## 6. Suggested statuses

Keep status categories separate.

-   **Group:** Proposed, Pending Approval, Approved, Rejected, Active,
    Completed, Dissolved.
-   **Project:** Draft, Proposal Submitted, Under Review, Revision
    Required, Proposal Approved, In Progress, Final Submitted,
    Evaluation Pending, Completed, Archived.
-   **Submission:** Submitted, Under Review, Revision Required,
    Approved, Rejected.
-   **Evaluation:** Draft, Submitted, Published.

Use explicit transition rules; not every status should transition to
every other status.

## 7. Scope boundaries and assumptions

-   The assigned advisor is the evaluator; a separate evaluation
    committee is not in scope.
-   Group formation supports admin-created and student-proposed groups.
-   Submissions are direct PDF/DOCX uploads, not only external links.
-   Admin owns official academic deadlines. Advisors manage internal
    milestones and can request official deadline changes.
-   Advanced AI features should be added after core workflows and access
    control are reliable.
-   Exact group size, grading rubric, academic-session rules, and
    file-size limits should be configurable rather than hard-coded.
