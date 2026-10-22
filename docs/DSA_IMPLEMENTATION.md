# DSA Implementation Plan

## Purpose

Demonstrate meaningful data structures and algorithms in C# within the
FYP Management System. Each structure should solve a real problem and
have a documented complexity analysis. Do not add structures just to
claim they were used.

## Candidate applications

  -----------------------------------------------------------------------
  Concept                 Practical use           Typical complexity
  ----------------------- ----------------------- -----------------------
  Dictionary / hash table Find a student by       Average O(1) lookup;
                          registration number in  worst case can be O(n)
                          an in-memory lookup     

  Sorting                 Sort projects by        Usually O(n log n) with
                          deadline or students by an efficient comparison
                          registration number     sort

  Searching               Search/filter project   O(n) for a simple
                          and student lists       linear scan; indexed DB
                                                  search may differ

  Queue                   Process queued          O(1) enqueue/dequeue
                          notification tasks in   with a suitable queue
                          FIFO order              

  Priority queue          Process the next most   O(log n)
                          urgent overdue/upcoming insert/remove-min; O(1)
                          deadline                peek

  State machine           Validate allowed        Depends on transition
                          transitions for group,  representation;
                          submission, and         explicit checks are
                          evaluation statuses     often O(1)
  -----------------------------------------------------------------------

These are typical bounds, not guarantees for every implementation. State
the assumptions and collection used.

## Suggested demonstrations

### 1. Student lookup

-   Build a `Dictionary<string, StudentSummary>` keyed by normalized
    registration number for a bounded in-memory demonstration.
-   Explain why average hash lookup is O(1).
-   For the real application, use a unique database index and query the
    database rather than loading every student into memory.

### 2. Deadline prioritization

-   Represent each deadline with project ID, due date, and status.
-   Use a `PriorityQueue` ordered by due date to find the earliest
    pending deadline.
-   Explain insertion/removal complexity and how overdue tasks are
    identified.
-   Rebuild or update the queue when deadlines/statuses change.

### 3. Sorting

-   Sort an authorized list of projects by due date, title, or status.
-   Compare the cost of sorting `n` items with a simple scan.
-   Prefer database-side sorting and pagination in normal production
    endpoints.

### 4. Notification queue (optional)

-   Queue notification jobs and process them in FIFO order.
-   For a real distributed deployment, use a durable background-job
    system rather than relying on an in-memory queue that is lost when
    the process stops.

## Implementation guidelines

-   Use built-in .NET collections for normal application code.
-   If the lab requires implementing a linked list, stack, queue, or
    sorting algorithm manually, put it in an isolated DSA module and add
    tests.
-   Avoid writing a custom hash table or priority queue for production
    use unless explicitly required.
-   Include edge cases: empty collections, duplicate keys, missing
    student, equal deadlines, cancelled projects, and changed due dates.
-   Do not expose one student's information while demonstrating lookups.
-   Document time and space complexity and include a small input/output
    example for each lab demonstration.

## Testing

For each manual implementation, test: - empty input - one element -
multiple elements - duplicates where relevant - already sorted and
reverse-sorted input for sorting - missing lookup key - equal
priority/deadline - invalid state transition

Add a short table in the final report showing the algorithm, purpose,
time complexity, space complexity, and where it appears in the project.
