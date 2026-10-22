# Portal Help Assistant

## 1. Purpose

The assistant helps users understand how to use the FYP portal and
interpret official FYP instructions. It is not a replacement for an
advisor or admin decision.

## 2. Recommended implementation stages

### Stage 1 --- FAQ assistant

-   Use predefined question/answer entries for login help, group
    proposals, submissions, file formats, feedback, deadlines, and
    result visibility.
-   Offer suggested questions and a clear fallback when a question is
    not recognized.
-   This can be implemented without an AI API and is inexpensive and
    predictable.

### Stage 2 --- AI assistant

-   React sends the question to an ASP.NET Core endpoint such as
    `POST /api/assistant/chat`.
-   The backend validates the request and calls the chosen AI provider.
-   Ground responses in admin-approved FYP guidelines and portal
    documentation.
-   Return a concise answer and, where possible, the document/section
    supporting it.
-   Keep provider keys on the server.

### Stage 3 --- Retrieval-augmented answers (optional)

-   Extract and chunk approved guideline documents.
-   Store searchable text and, if selected, embeddings.
-   Retrieve relevant chunks for each question.
-   Provide source references and a fallback if evidence is missing.
-   Add this only after the basic portal is secure and useful.

## 3. Rules for trustworthy answers

-   Do not invent official deadlines, grading policy, required formats,
    or approval outcomes.
-   If the relevant policy is missing or ambiguous, tell the user to
    contact the advisor/admin.
-   Do not claim a submission was accepted or marks were published
    unless the application data confirms it.
-   Separate official policy from general suggestions.
-   Do not use AI output as the authority for permissions or workflow
    transitions; the backend enforces these.
-   Avoid giving one user another user's private details, feedback,
    marks, or documents.

## 4. Authentication and privacy

-   Treat user questions and retrieved documents as untrusted input.
-   Authenticate requests when private or user-specific context is
    involved.
-   Apply authorization before retrieving project-specific information.
-   Send only the minimum necessary context to the AI provider.
-   Do not send passwords, tokens, database credentials, or unrelated
    personal records.
-   Set request size, rate, and timeout limits.
-   Log operational metadata without unnecessarily storing full
    conversations or sensitive content.

## 5. User experience

-   Place the assistant launcher on the landing page and inside the
    authenticated portal where useful.
-   Provide suggested prompts such as:
    -   How do I propose a group?
    -   Which file formats are accepted?
    -   How do I resubmit after revision?
    -   Where can I see advisor feedback?
    -   When will published marks become visible?
-   Make it clear when the assistant does not know an answer.
-   Provide a contact path to the admin/advisor for policy-specific
    questions.

## 6. Testing

Test known FAQs, ambiguous deadline questions, unsupported policies,
prompt-injection attempts in uploaded text, unauthorized project
queries, provider timeouts, and empty/oversized input. Verify that the
assistant cannot bypass backend authorization.
