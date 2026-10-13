# KNOW HOW — Bug Fixes, Llama API Reuse, Document Management & Landing Page Enhancements

## Role

Act as a **Senior Full-Stack Engineer, AI/RAG Engineer, Backend Developer, and UI/UX Engineer** working on the existing KNOW HOW application.

Inspect and improve the current codebase. **Do not rebuild the application from scratch or redesign working screens unnecessarily.** Preserve the current User Mode layout and visual style unless a specific change is required below.

## 1. Reuse the Existing GPU-Hosted Llama API

### Current issue
Editor Mode training is blocked because the application reports that Phi-3 Mini is not installed or available. However, User Mode already uses a GPU-hosted Llama API for its chatbot.

### Required behavior
- Inspect the existing User Mode chatbot implementation, backend service, configuration, API client, and deployment settings.
- Reuse the existing Llama API integration for Editor Mode guidance generation.
- Do **not** require Phi-3 Mini to be installed locally or on the AI server.
- Do not create a duplicate LLM service if the existing Llama service can be reused.
- Keep RAG retrieval, document chunking, embeddings, and vector storage as distinct components where appropriate. Do not assume the Llama generation API also provides embeddings unless the existing implementation confirms that.
- Use existing service configuration rather than hardcoding credentials, URLs, or model names in UI components.
- Verify actual API readiness through a real backend health/readiness check. Do not show a model as ready merely because a configuration value exists.
- If the API is unavailable, show an accurate, actionable error. Do not simulate training, model responses, progress, or success.

### Acceptance criteria
- Editor Mode can generate guidance through the same Llama API integration used by User Mode.
- No Phi-3 Mini installation is required.
- API failures are surfaced clearly and do not produce false “trained” or “published” states.

## 2. Fix Trained Document List Status and Actions

Inspect the document registry, training records, published versions, and UI action logic.

### Required behavior
- A document marked **Published** must have a valid published version in the database.
- A document that is registered but has never completed training must display **Not Trained**.
- A document with generated or reviewed content that has not been published must display the appropriate non-published status, such as **Draft** or **Review Required**, according to the existing workflow.
- Do not infer training or publication status solely from a document name, hardcoded list, or UI flag.
- Separate a document’s status from its available actions. An authorized editor must be able to edit a published document.
- For the Cybersecurity Plan currently shown as Published, enable **Edit** if the user has the required permissions.
- For registered but untrained documents such as CSPL Handbook, Cybersecurity Case, Cybersecurity Interface Agreement (CIA), Cybersecurity Plan, and ISO Bulletin, show **Not Trained** when no successful training record exists. Enable **Edit** and **Train** where appropriate.
- Respect role-based access control. Do not expose Editor-only actions to User Mode.

### Acceptance criteria
- Every displayed status reflects actual persisted state.
- Edit/Train/Delete availability is determined by permissions and workflow rules, not merely by status.
- Refreshing the page does not revert the UI to fabricated or stale status values.

## 3. Update the Train Workflow to Support Multiple Reference Documents

When the editor selects **Train**, open a clear training setup dialog.

### Required behavior
- Ask the editor to select the document template (for example, the relevant DOCX template).
- Allow one or more additional reference documents.
- Include an **Add More Files** action so the editor can add multiple references.
- Show each selected file’s name and allow removing an individual file before starting.
- Validate required inputs, file types, file sizes, and duplicate selections according to supported backend limits.
- Clearly distinguish the template from reference documents.
- Show a confirmation/summary before training begins.
- Send selected files and configuration to the backend using the existing upload and processing architecture.
- Show real processing stages and progress only when the backend can report them. If exact progress is unavailable, show an honest indeterminate processing state.
- Handle upload, extraction, retrieval, generation, and persistence errors without falsely marking the document as trained.

### Acceptance criteria
- An editor can select a template and add multiple reference documents before training.
- The editor can remove a mistakenly selected file.
- Training starts only after validation and explicit confirmation.
- The final status is updated only after the backend confirms the corresponding operation.

## 4. Configurable Document-Level Guidance Fields

Add a configuration interface for **document-level fields**, such as Overview, Purpose, Scope, or other fields defined by the editor.

For each field, provide editable configuration properties such as:
- **Field name**
- **Generation instructions / prompt** — specific instructions for what the LLM should produce
- **Example output** — a sample of the expected answer format and level of detail
- **Additional context** — domain-specific guidance that can help retrieval and generation
- **Required/optional** — whether the field must be generated
- **Display order**, if supported by the existing UI

### Required behavior
- Allow editors to add and remove fields.
- Allow editors to edit existing field configurations.
- Validate field names and required configuration values.
- Persist field configurations in the database and reload them when the editor reopens the configuration.
- Pass field-specific instructions, examples, and additional context into the generation workflow.
- Use retrieved source content as evidence. Examples and instructions must guide format and scope, not be treated as factual source material.
- Do not hardcode field definitions in the frontend when the purpose is to make them configurable.
- Prevent deletion of a field from silently destroying already-published content. Use confirmation and the existing versioning/archival approach.

## 5. Configurable Section-Level Guidance Fields

Apply the same configurable-field capability to **section-level pop-up guidance** generated for sections extracted from the template.

For each section-level field, support applicable properties such as:
- **Field name**
- **Generation instructions / prompt**
- **Example output**
- **Additional context**
- **Required/optional**
- **Display order**

### Required behavior
- Allow the editor to add, edit, and remove section-level fields.
- Support section-specific context and instructions where necessary.
- Keep document-level fields separate from section-level fields, while reusing common field-configuration components and backend logic where practical.
- Associate generated content with the correct document, section, field, and version.
- Keep source references or provenance where the current architecture supports them, so an editor can verify generated content.
- Ensure editing one section or field does not unintentionally overwrite unrelated sections or fields.
- Allow regeneration of an individual section or field without full document retraining, if supported by the existing architecture.
- Provide an explicit **Apply All**, **Train Again**, and **Cancel** flow where it fits the existing workflow. Do not publish changes automatically just because generation completed.

## 6. Add a Safe Delete Option for Documents

Add a **Delete** action for a document when the current user has the appropriate permission.

### Required behavior
- Ask for confirmation before deleting and identify the document clearly.
- Prefer soft deletion or archival when needed to preserve audit history, prior published versions, and references from other records.
- Ensure deletion does not leave broken section, field, training, or vector-store references.
- If associated data must be removed or archived, handle it through a deliberate backend transaction or coordinated cleanup process.
- Prevent deletion when business rules prohibit it, and explain why.
- Refresh the document list and status after successful deletion.
- Log the action in the existing audit mechanism, if available.
- Do not show success until the backend confirms the operation.

## 7. Global Landing Page and Document Selection Behavior

### Current issue
The application automatically selects the Cybersecurity Plan on initial load. This causes document content and the section hierarchy to appear before the user selects a document.

### Required behavior
Apply the same initial landing behavior when the application opens in **User Mode, Editor Mode, or Admin Mode**, subject to each mode’s permissions.

- On initial entry, do not automatically select Cybersecurity Plan or any other document.
- Start with **no document selected**.
- Show the existing User Mode welcome experience as the initial landing page.
- In the main content panel, display an attractive, professional **KNOW HOW** welcome screen with tasteful animation.
- Keep the welcome screen consistent with the existing design system. Avoid distracting animations, excessive visual effects, or a full redesign.
- While no document is selected, keep the document section/subsection hierarchy empty. Do not display stale sections from a previously selected document.
- Selecting an available document should load its **Overview** in the main content panel and populate the left-side section hierarchy for that document.
- Clear any previous document’s section selection, pop-up guidance, generated content, and related state when switching documents.
- Switching modes or returning to the landing page must not accidentally retain a stale document selection.
- Preserve role-based permissions. The common landing screen must not expose privileged Editor or Admin actions to unauthorized users.
- Make the welcome screen responsive and accessible, including reduced-motion support where applicable.

### Acceptance criteria
- On first load in each mode, no document is selected.
- The main panel displays the KNOW HOW welcome screen.
- The section/subsection panel is empty until a document is selected.
- Selecting a document displays its Overview and correct section hierarchy.
- Switching documents does not show content or pop-ups belonging to the previous document.

## 8. User Mode Content and Publication Rules

User Mode must display only content that is approved and published according to the application’s actual workflow.

- Load published document and guidance content from the database.
- Do not expose draft, unreviewed, failed, or unpublished generated content to ordinary users.
- Ensure document availability is based on valid published versions, not a hardcoded document list.
- If a document has no published version, show the appropriate unavailable/not-published state rather than fabricated content.
- When a new version is published, ensure User Mode displays the correct active published version.
- Preserve the existing three-panel layout: document/section navigation on the left, main content in the center, and chatbot on the right.

## 9. Backend, Database, and API Requirements

Inspect the current schema and architecture before changing them. Reuse existing models and endpoints where practical; introduce migrations only when required.

Ensure the backend can represent, as needed:
- Document registry and lifecycle status
- Template and reference-file metadata
- Training jobs and actual outcomes
- Document-level field configurations
- Section-level field configurations
- Generated content associated with document/section/field/version
- Draft, review, and published versions
- User roles and permissions
- Delete/archive state and audit history

Implementation requirements:
- Validate all inputs on the backend; frontend validation alone is insufficient.
- Enforce role-based authorization on every protected endpoint.
- Store secrets in environment variables or the existing secure configuration mechanism.
- Use parameterized database queries or the ORM’s safe query facilities.
- Use transactions for related database updates where appropriate.
- Make retried operations safe where practical, and avoid duplicate training records caused by repeated requests.
- Add or update API schemas and migrations consistently with existing project conventions.
- Do not create duplicate API routes or services without first checking for existing equivalents.
- Keep frontend state synchronized with the authoritative backend response.

## 10. Error Handling and User Feedback

- Provide clear, actionable messages for unsupported files, extraction failures, unavailable LLM APIs, retrieval failures, generation failures, permission errors, database failures, and publication failures.
- Preserve editor changes where possible when an operation fails.
- Do not silently swallow errors.
- Do not use hardcoded fake document records, fake training progress, simulated LLM responses, or fabricated success notifications.
- Keep loading, empty, error, draft, published, and unavailable states visually distinct.
- Do not mark a document as trained or published unless the backend confirms the operation and the corresponding state is persisted.

## 11. Development Workflow

Before implementing:
1. Inspect the existing frontend, backend, database schema, document registry, training flow, and User Mode chatbot integration.
2. Identify the actual root causes of the Phi-3 Mini message, incorrect document statuses, disabled actions, and automatic document selection.
3. Reuse existing components, services, APIs, and design conventions wherever possible.
4. Propose the smallest coherent set of changes that fully addresses the requirements.
5. Implement backend and frontend changes together so UI states reflect real persisted data.
6. Add or update tests for the affected behavior.
7. Run available tests, linting, type checks, and build commands; report any checks that cannot be run.

Do not stop after changing only the UI. Verify the complete path from user action through API, processing, database persistence, and refreshed UI.

## 12. Final Verification Checklist

Before declaring the work complete, verify all of the following:

- [ ] Editor Mode reuses the existing GPU-hosted Llama API used by User Mode.
- [ ] No Phi-3 Mini installation is required.
- [ ] LLM readiness and errors reflect real service responses.
- [ ] Document statuses reflect actual training and publication records.
- [ ] Published documents can be edited by authorized editors.
- [ ] Untrained registered documents show **Not Trained** and expose appropriate actions.
- [ ] Training supports a template and multiple reference documents.
- [ ] **Add More Files** and individual file removal work correctly.
- [ ] Document-level guidance fields are configurable and persisted.
- [ ] Section-level guidance fields are configurable and persisted.
- [ ] Field-specific instructions, examples, and context are used during generation.
- [ ] Generated content remains associated with the correct document, section, field, and version.
- [ ] Delete/archival requires confirmation and preserves required history.
- [ ] The initial landing page has no selected document in User, Editor, and Admin modes.
- [ ] The KNOW HOW welcome screen appears on initial load.
- [ ] Section/subsection navigation remains empty until a document is selected.
- [ ] Selecting a document loads its Overview and section hierarchy.
- [ ] Switching documents clears stale content and selections.
- [ ] User Mode displays only valid published content.
- [ ] No fake statuses, fake progress, or false success messages remain.
- [ ] Relevant tests and build checks pass, or failures are documented accurately.

## Expected Final Report

When implementation is complete, provide:
1. A concise summary of the root causes identified.
2. The files changed and what changed in each.
3. Any database migrations or configuration changes required.
4. The tests and build checks run, with actual results.
5. Any remaining limitations or deployment steps.

**Important:** Preserve the working User Mode UI and existing application architecture. Make changes based on the actual codebase, and never claim that training, generation, saving, publishing, or deletion succeeded unless the backend confirms it.
