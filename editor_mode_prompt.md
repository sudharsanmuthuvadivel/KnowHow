# KNOW HOW — EDITOR MODE, RAG/LLM DOCUMENT TRAINING, ADMIN MODE & INTERACTIVE GUIDANCE PROMPT

## 1. ROLE AND RESPONSIBILITY

Assume you are a Senior Full-Stack Developer, AI/RAG Architect, LLM Engineer, UI/UX Architect, Database Architect, and Enterprise Application Designer.

You have extensive experience with:

- React, TypeScript, Vite, Tailwind CSS
- Python and FastAPI
- MySQL database design and integration
- Retrieval-Augmented Generation (RAG)
- Large Language Models (LLMs)
- Document parsing, chunking, embeddings, and semantic retrieval
- DOCX and PDF processing
- Section-aware document analysis
- Enterprise authentication and role-based access control
- AI-generated content review and approval workflows
- Interactive onboarding and guided UI experiences
- Framer Motion and professional enterprise UI/UX

You are continuing development of the existing application:

**KNOW HOW — Automotive Cybersecurity Engineering Platform**

IMPORTANT: Do not redesign the existing application from scratch. The existing User Mode UI is already good and must be preserved.

Implement the requested functionality by extending the existing application, components, APIs, and database architecture.

First inspect the existing source code, database schema, authentication mechanisms, document rendering components, and current settings menu. Reuse existing functionality wherever possible.

---

# 2. CORE APPLICATION CONCEPT

KNOW HOW must support three modes:

1. User Mode
2. Editor Mode
3. Admin Mode

Each mode has a distinct responsibility.

### User Mode — Consume Approved Knowledge

User Mode displays approved document content and section guidance retrieved from MySQL.

The current UI, including the three-panel layout, section navigation, document rendering, guidance popups, animations, and AI chatbot, must remain visually consistent with the existing implementation.

Do not expose unapproved or draft-generated guidance to ordinary users.

### Editor Mode — Generate, Review, and Publish Knowledge

Editor Mode allows authorized editors to:

- Upload a document template.
- Upload additional reference documents.
- Configure the fields to generate.
- Run a RAG-based document analysis workflow.
- Generate section-specific guidance using an LLM.
- Review and edit generated content.
- Regenerate individual sections.
- Approve individual sections.
- Publish approved content to MySQL.
- Update previously trained documents.

### Admin Mode — Manage Application Administration

Admin Mode provides an administrative area for authorized administrators.

It should support:

- Managing editor and administrator accounts.
- Reviewing user feedback and queries.
- Managing access permissions.
- Viewing document training status.
- Monitoring training and publishing operations.
- Managing system-level configuration where appropriate.

Implement only the features supported by the existing backend, adding new APIs where necessary.

Do not make Admin Mode available to every logged-in editor.

---

# 3. SETTINGS MENU

Preserve the existing settings icon and dropdown menu in the top-right corner.

Update the menu to contain:

1. Mode
   - User Mode
   - Editor Mode
   - Admin
2. Help / Feedback
3. About, if already supported

### Mode Switching

- Selecting User Mode opens the existing user-facing interface.
- Selecting Editor Mode starts the editor authentication flow.
- Selecting Admin starts the administrator authentication flow or checks that the current user has the administrator role.
- Unauthorized users must not be allowed to access restricted pages by manually changing URLs or frontend state.

Show the current mode clearly in the UI.

Do not allow a frontend dropdown selection alone to grant permissions.

---

# 4. HELP / FEEDBACK FUNCTIONALITY

The existing Help option must function as a user feedback and query collection interface.

When selected, open a dedicated Help / Feedback panel.

### Fields

- Subject or query category
- User query / feedback
- Optional document name
- Optional section number
- Submit button
- Cancel button

Example categories:

- UI issue
- Document content issue
- Section guidance issue
- Feature request
- General feedback

### Submission Workflow

When a user submits feedback:

1. Validate the input.
2. Store the feedback in MySQL.
3. Generate a unique feedback ID.
4. Record the submission timestamp and status.
5. Make the feedback available to authorized Editors and Admins.
6. Show a clear submission confirmation.

Do not display feedback publicly.

Provide status tracking where practical, such as:

- New
- Under Review
- Resolved

Editors and Admins should be able to review submitted feedback and queries through their authorized interfaces.

---

# 5. EDITOR MODE — AUTHENTICATION LANDING PAGE

When a user selects Editor Mode, display a dedicated login page before opening the editor workspace.

### UI Design

Create an attractive, futuristic, premium enterprise welcome screen for KNOW HOW.

Use:

- A sophisticated automotive cybersecurity-inspired background.
- Subtle animated network patterns or abstract vehicle-system graphics.
- Glassmorphism for the login card.
- Professional typography.
- Restrained gradients and soft glow.
- Smooth transitions.
- Clear branding.

The User ID and Password fields must be centrally positioned and easy to use.

Include:

- KNOW HOW logo or wordmark
- Welcome message
- User ID
- Password
- Show/hide password control
- Login button
- Back to User Mode option
- Appropriate error and loading states

Do not overuse neon colors or create a gaming-style interface.

### Authentication Requirements

User IDs and passwords must be managed securely through the backend.

- Store user identifiers in MySQL.
- Never store plaintext passwords.
- Store passwords using a strong password-hashing algorithm such as Argon2id or bcrypt.
- Validate credentials on the server.
- Use secure session management or appropriately configured tokens.
- Enforce login throttling and reasonable security controls.
- Avoid exposing sensitive authentication details in error messages.

After successful authentication, verify the user's assigned role.

Only users with the Editor role may enter Editor Mode.

Users without the required role must be denied access.

---

# 6. EDITOR MODE — MAIN WORKSPACE

After successful login, open the Editor Workspace.

Create a professional, futuristic, enterprise-grade UI consistent with the KNOW HOW visual identity.

The workspace should contain:

1. Editor header and user profile.
2. Trained Documents panel.
3. New Document action.
4. Document training workflow.
5. Section-wise generated guidance workspace.
6. Training status and progress indicators.
7. Clear actions for reviewing and publishing content.

Do not replace the existing User Mode interface. Editor Mode is a separate workspace that reuses existing document components and styling wherever practical.

---

# 7. TRAINED DOCUMENTS PANEL

Display a list of documents already registered and trained in the application.

Examples:

- Cybersecurity Plan
- Cybersecurity Case
- Cybersecurity Interface Agreement
- Other registered cybersecurity work products
- Future cybersecurity guides and reference documents

Retrieve the list from MySQL.

Do not hardcode the trained document list.

Each document card or row should display relevant metadata, such as:

- Document name
- Document category
- Training status
- Last updated timestamp
- Version, if supported
- Available actions

### Actions

Each trained document should provide:

**Edit**

Open the existing document's section-wise guidance editor.

**Train**

Start the retraining workflow for the selected document.

Retraining must not overwrite approved content until the editor explicitly approves the new result.

Display a confirmation dialog before starting a retraining operation.

Example:

"Retraining will generate a new draft of the document guidance. Existing approved content will remain unchanged until you publish the new version. Do you want to continue?"

Buttons:

- Confirm
- Cancel

The document list must refresh after successful training or publishing.

---

# 8. NEW DOCUMENT WORKFLOW

Place a prominent **New** button above the Trained Documents list.

When selected, open a new document registration and training workflow.

The editor must be able to provide:

1. Document name
2. Document category
3. Template document
4. Optional reference documents
5. Optional document description or metadata

The template defines the main document structure that will eventually be displayed in User Mode.

### Template Upload

Allow the editor to upload the authoritative template document.

Initially support the file types that the existing parser can reliably process, such as DOCX and PDF.

For DOCX templates, extract:

- Section numbers
- Section names
- Nested subsections
- Paragraphs
- Tables
- Relevant document structure

Do not manually hardcode sections.

The template must be registered as a document entity in the database.

### Reference Documents

Allow the editor to upload additional reference material.

Examples:

- Existing cybersecurity guidelines
- Internal engineering documents
- Relevant standards or guidance documents that the organization is authorized to use
- Customer-specific requirements
- Approved reference work products

Reference documents are optional.

The editor must be able to:

- Upload multiple files.
- View uploaded filenames.
- Remove an uploaded file before training.
- Add more reference documents using an **Add** button.
- See file validation errors.
- Cancel the workflow.

Provide an explicit Train button at the bottom of the upload area.

---

# 9. DYNAMIC TRAINING PARAMETERS

After the editor uploads the template and reference documents and selects Train, open a parameter configuration dialog.

The editor defines which information fields should be generated for each section.

### Example

Parameter 1:

Objective

[Add] [Remove]

Parameter 2:

ISO Reference

[Add] [Remove]

Parameter 3:

Evidence Required

[Add] [Remove]

Other possible parameters include:

- Section Summary
- Owner
- Checklist
- Criticality
- Phase
- Cybersecurity Requirements
- Required Work Products
- Related References

These examples are configurable options, not mandatory fields for every document.

### Dynamic Field Management

The editor must be able to:

- Add a new parameter.
- Remove an unnecessary parameter.
- Reorder parameters, if practical.
- Review the configured fields before training.
- Confirm the configuration.
- Cancel the operation.

Validate that required parameters are not accidentally removed.

Store the parameter configuration against the document or training job so retraining uses the correct configuration.

Do not hardcode the parameter list into the frontend if it is intended to be configurable.

---

# 10. RAG + LLM DOCUMENT TRAINING PIPELINE

This is the core functionality of Editor Mode.

Implement a genuine RAG-based pipeline rather than simulating document training in the frontend.

### Training Flow

Template + Reference Documents
→ Document Parsing
→ Template Section Extraction
→ Reference Document Chunking
→ Metadata Enrichment
→ Embedding Generation
→ Section-Aware Retrieval
→ LLM Generation
→ Validation
→ Section-Wise Draft Review
→ Editor Approval
→ MySQL Publishing
→ User Mode Display

### Step 1 — Parse the Template

Extract the authoritative section hierarchy from the uploaded template.

For each section, retain:

- Document ID
- Section ID
- Section number
- Section title
- Parent section ID
- Section order
- Relevant template content

Support nested sections.

### Step 2 — Parse Reference Documents

Extract usable text and structure from supported reference files.

Preserve source metadata, including:

- Source document ID
- Source filename
- Page number, where available
- Section or heading, where available
- Source location or chunk identifier

Do not assume that every reference document uses the same structure.

### Step 3 — Chunk Reference Material

Use a chunking strategy appropriate to the content.

Prefer semantic or structure-aware chunking based on:

- Headings
- Sections
- Subsections
- Paragraphs
- Tables
- Logical content boundaries

Use chunk size and overlap appropriate to the selected embedding model and LLM context window.

Avoid breaking tables, requirements, or definitions in ways that destroy their meaning.

Each chunk should retain its source metadata.

### Step 4 — Generate Embeddings

Generate embeddings using the configured embedding model.

Store them in the vector store selected for the application.

The implementation may use a compatible vector database or an existing vector retrieval service.

Do not assume that MySQL alone provides vector-search functionality unless the deployed version and configuration support it.

Keep vector storage and relational database responsibilities clearly separated.

### Step 5 — Section-Aware Retrieval

For each template section:

1. Build a retrieval query from the section number, title, template content, and configured parameters.
2. Retrieve relevant reference chunks.
3. Prefer chunks that are semantically relevant and traceable to their sources.
4. Use section metadata and document identity to reduce unrelated retrieval.
5. Avoid using the entire reference corpus indiscriminately for every section.

The same source chunk may support more than one section when relevant.

### Step 6 — LLM Generation

Send the following information to the configured LLM:

- Template section number
- Template section title
- Relevant template content
- Configured generation parameters
- Retrieved reference chunks
- Source metadata
- Output schema
- Content-quality requirements

Generate structured, section-specific guidance.

Use a defined JSON schema or equivalent validated output format so that generated values map reliably to the configured fields.

For example:

{
  "section_number": "5.2",
  "section_title": "Cybersecurity Requirements",
  "fields": {
    "Objective": "...",
    "ISO Reference": "...",
    "Evidence Required": "...",
    "Checklist": "..."
  },
  "sources": [
    {
      "document": "Reference Document.docx",
      "section": "...",
      "page": 12
    }
  ]
}

This is an illustrative structure. Adapt it to the existing application data model.

### Step 7 — Validation

Before presenting the generated results:

- Validate the output schema.
- Check that the section identity is correct.
- Detect missing required fields.
- Detect malformed or incomplete responses.
- Preserve source references.
- Flag unsupported claims where practical.
- Do not invent ISO clause references or citations.
- Clearly indicate when the retrieved sources do not provide sufficient evidence.

Treat uploaded reference content as untrusted data. It must not be allowed to override system instructions, change access permissions, or trigger unauthorized actions.

### Step 8 — Present Drafts

Display the generated guidance in the Editor workspace, grouped by template section.

The editor must be able to review, edit, regenerate, and approve the content before it reaches User Mode.

**Important: AI-generated content is always a draft until explicitly approved.**

---

# 11. SECTION-WISE GENERATED GUIDANCE EDITOR

After training, show a section-wise review interface.

For every section, display:

- Section number
- Section title
- Generated fields
- Source references, where available
- Generation status
- Approval status

Each field should be editable.

Use appropriate controls based on content type:

- Single-line input for short values.
- Multiline text area for descriptions.
- Rich-text editor only if supported by the existing application.
- Structured list editor for checklists where appropriate.

Preserve the configured field names and order.

### Actions for Each Section

**Apply**

Validate and save the section as approved content, according to the publishing workflow.

**Delete**

Request confirmation before removing the section from the current draft.

Deleting a generated draft section must not automatically delete an existing approved section.

**Train**

Regenerate only the selected section using RAG + LLM.

Do not retrain the entire document when only one section is selected.

Retain the existing approved version until the regenerated result is reviewed and approved.

### Regeneration Behavior

When regenerating one section:

- Reuse the registered template and reference documents.
- Retrieve relevant chunks again.
- Apply the current parameter configuration.
- Generate the selected section only.
- Preserve other section drafts and approvals.
- Clearly identify the result as a new draft.

If a section cannot be regenerated, show a useful error and preserve its previous draft.

---

# 12. DOCUMENT-LEVEL TRAINING ACTIONS

At the bottom of the generated guidance workspace, provide:

### Apply All

Validate all eligible sections and publish the approved content in a controlled transaction or versioned publishing workflow.

If some sections are invalid or incomplete, identify them and prevent accidental publication of invalid content.

The editor must be able to review the final changes before confirming publication.

### Train Again

Regenerate the document's section guidance using the current template, references, parameters, and model configuration.

Show a confirmation dialog before starting.

Preserve the existing approved version until the new content is approved and published.

Display training progress and allow the editor to see which sections have completed.

### Cancel

Cancel the current draft/review workflow.

Ask for confirmation if unsaved changes or an active training job would be affected.

Do not delete previously approved content when the editor cancels.

### Background Jobs

For long-running training operations:

- Run the work through a backend job mechanism.
- Display progress and status.
- Handle timeouts and failures.
- Prevent duplicate submissions where appropriate.
- Allow safe recovery from interrupted jobs.
- Do not simulate successful training before the backend confirms completion.

---

# 13. APPLY AND PUBLISHING RULES

Establish a clear distinction between:

1. Template registration
2. Training
3. Draft generation
4. Editing
5. Section approval
6. Document publication

Use versioning or equivalent transactional safeguards.

### Required Behavior

- A new document must not become an active User Mode document merely because it has been uploaded.
- Draft guidance must not be visible to ordinary users.
- Applying a section must follow the configured approval rules.
- Apply All must not silently publish invalid content.
- Retraining must not destroy the previous approved version.
- Publishing must update the relevant document and section mappings.
- User Mode must display the latest successfully published version.

Record useful audit metadata:

- Document ID
- Version
- Section ID
- Generated timestamp
- Updated timestamp
- Editor identity
- Approval status
- Publication timestamp
- Training job ID, where applicable

Use the existing database schema when possible; add migrations only where necessary.

---

# 14. USER MODE INTEGRATION

Preserve the existing User Mode UI.

When an editor successfully publishes a new document:

1. Register the document under the correct document category.
2. Make the document available in the appropriate navigation list.
3. Load its authoritative template into the main document viewer.
4. Display its published section guidance in the existing popup.
5. Show the configured Criticality and Phase fields when present.
6. Keep all section mappings consistent with the template.
7. Display only published content.

For example, after Cybersecurity Case has been successfully trained and published, it should appear in the Cybersecurity Work Products category in User Mode.

The document should display its own sections and approved guidance.

Do not require hardcoded React changes whenever a new document is published.

The navigation, Overview, section guidance, and document mapping must be driven by backend data.

---

# 15. ADMIN MODE

Add Admin as a separate option under Settings.

Admin Mode must be restricted to users with the Admin role.

Provide an administrative dashboard with modular areas for:

- User and role management
- Editor account management
- Submitted feedback and queries
- Trained document registry
- Training job status
- Publication/version history
- System configuration, where authorized

Admins should be able to review feedback submitted through Help / Feedback and route it to an appropriate editor.

Support account activation/deactivation and role assignment only with appropriate authorization and audit logging.

Do not automatically grant Admin privileges to Editors.

---

# 16. INTERACTIVE ONBOARDING AND GUIDED TOUR

Implement an interactive, step-by-step tutorial for both User Mode and Editor Mode.

The purpose is to teach users how to operate the application without requiring external documentation.

### Automatic First-Visit Guidance

When a user enters a mode for the first time, offer the relevant guided tour.

Provide:

- Start Tour
- Skip
- Next
- Previous
- Finish
- Close

Remember tutorial completion per authenticated user and mode where the existing persistence architecture supports it.

Users must be able to restart the tutorial from Help or Settings.

### Editor Mode Tutorial

Use highlighted UI elements and compact explanatory tooltips.

Example sequence:

Step 1 — Trained Documents
"View and manage documents that have already been registered and trained."

Step 2 — New
"Register a new document template and optionally upload reference documents."

Step 3 — Template Upload
"Upload the main document whose structure will be used in User Mode."

Step 4 — Reference Documents
"Add reference materials to provide supporting context for RAG-based generation."

Step 5 — Training Parameters
"Choose the fields that the LLM should generate for each section."

Step 6 — Train
"Start the document analysis and section-wise guidance generation workflow."

Step 7 — Review Generated Content
"Inspect and edit the generated fields. Review available source references."

Step 8 — Section Actions
"Apply approved section content, regenerate an individual section, or remove it from the current draft."

Step 9 — Apply All
"Review and publish the eligible finalized section guidance."

Step 10 — Trained Document Management
"Edit or retrain previously registered documents."

The tour must describe the actual UI controls and must not claim that a feature is available if it has not been implemented.

### User Mode Tutorial

Provide a separate tour explaining:

- Document categories
- Section navigation
- Overview
- Important Notes
- Section guidance popup
- Criticality and Phase badges
- Reference images
- AI chatbot, when available
- Help / Feedback
- Settings

### Future-Proofing

The onboarding system must be data-driven and modular.

When new features are introduced, developers should be able to register new tutorial steps without rewriting the entire tour component.

Use stable component identifiers or equivalent anchors.

If a target element is hidden or unavailable, skip that step safely.

Avoid blocking normal interaction. Users must be able to skip or close the tour at any point.

---

# 17. DATABASE ARCHITECTURE

Inspect the existing MySQL schema before making changes.

Reuse existing tables and relationships wherever possible.

The data model must support the following logical entities, implemented using the existing naming conventions:

- Users
- Roles and permissions
- Documents
- Document versions
- Template files
- Reference files
- Template sections
- Training jobs
- Training parameters
- Reference chunks and source metadata
- Generated section drafts
- Section fields
- Published section guidance
- Approval and publication history
- Feedback submissions
- Tutorial completion state, if persisted

Keep large files in appropriate file/object storage or the existing file-storage mechanism. Store their identifiers and metadata in MySQL rather than unnecessarily placing large binary files in relational rows.

Maintain referential integrity and enforce authorization in the backend.

Use database transactions for related updates where appropriate.

---

# 18. API AND SERVICE ARCHITECTURE

Follow the existing backend conventions and introduce endpoints only where necessary.

Possible logical API groups include:

Authentication:
- Login
- Logout
- Current user and role

Documents:
- List documents
- Register a document
- Upload template
- Upload reference files
- Retrieve document details

Training:
- Create training job
- Configure generation parameters
- Start training
- Check training status
- Retrieve generated section drafts
- Regenerate a section
- Retrain a document

Review and publication:
- Update section draft
- Apply section
- Delete draft section
- Apply all eligible sections
- Publish a document version
- Retrieve publication history

Administration:
- Manage authorized users and roles
- Review feedback
- View training jobs
- View document versions

Help / Feedback:
- Submit feedback
- List authorized feedback records
- Update feedback status

These are suggested API responsibilities, not instructions to duplicate existing endpoints.

Use asynchronous/background processing for long-running RAG and LLM jobs.

---

# 19. SECURITY AND DATA GOVERNANCE

Implement the following safeguards:

- Server-side role-based authorization.
- Secure password hashing.
- Input validation.
- File type and size validation.
- Safe filename handling.
- Path traversal prevention.
- Secure file storage.
- Protection against unauthorized file access.
- Rate limiting for authentication and expensive training operations.
- Server-side validation of every publishing action.
- Audit logging for sensitive operations.
- Safe handling of uploaded document content.
- Protection against prompt injection in retrieved documents.
- Secrets and LLM credentials stored server-side.
- No API keys in frontend source code.
- No execution of uploaded macros or embedded scripts.
- Clear errors without exposing stack traces or secrets.

Only authorized users may upload, train, modify, approve, or publish documents.

---

# 20. USER EXPERIENCE AND VISUAL DESIGN

The Editor workspace should feel futuristic, attractive, and professional while remaining consistent with the existing KNOW HOW application.

Use:

- Premium enterprise cards
- Clean typography
- Subtle gradients
- Controlled glassmorphism
- Fine borders
- Clear action hierarchy
- Smooth transitions
- Status badges
- Useful progress indicators
- Well-designed confirmation dialogs
- Accessible form controls

Avoid excessive animation, clutter, aggressive neon, and gaming-style effects.

Use distinct visual states for:

- Ready
- Uploading
- Parsing
- Training
- Generating
- Draft
- Needs Review
- Approved
- Published
- Failed

Provide accessible focus states, keyboard navigation, clear labels, and responsive layouts.

---

# 21. IMPLEMENTATION STRATEGY

Before implementation:

1. Inspect the current source code.
2. Understand the existing User Mode UI.
3. Inspect the settings and mode-switching logic.
4. Inspect MySQL models and APIs.
5. Identify existing document upload and parsing functionality.
6. Identify the current LLM or RAG integration, if any.
7. Determine what is implemented and what remains a placeholder.
8. Plan database migrations and backward-compatible API changes.

Implement incrementally.

Do not build fake end-to-end functionality using static arrays or simulated API responses.

If an LLM provider, embedding model, or vector store has not been configured, create the appropriate integration interface and display a clear setup requirement. Do not claim training has succeeded when it has not.

Maintain the existing User Mode experience throughout development.

---

# 22. ACCEPTANCE CRITERIA

## Settings and Access

- [ ] User Mode remains visually and functionally consistent.
- [ ] Editor Mode opens the dedicated login interface.
- [ ] Admin is added to Settings.
- [ ] Backend authorization enforces the correct roles.
- [ ] Unauthorized access is blocked.

## Editor Workspace

- [ ] Trained documents are loaded dynamically.
- [ ] Existing documents provide Edit and Train actions.
- [ ] New registers a new document.
- [ ] Template upload works.
- [ ] Multiple reference documents can be added and removed.
- [ ] Training parameters can be configured dynamically.
- [ ] Confirmation is shown before retraining.

## RAG and LLM

- [ ] Template sections are extracted dynamically.
- [ ] Reference files are parsed.
- [ ] Content is chunked with source metadata.
- [ ] Embeddings are generated using the configured model.
- [ ] Retrieval is section-aware.
- [ ] The LLM produces validated structured drafts.
- [ ] Source references are retained where available.
- [ ] Failures are surfaced clearly.

## Section Review

- [ ] Generated content is displayed section by section.
- [ ] Fields are editable.
- [ ] Apply works according to the approval workflow.
- [ ] Delete requests confirmation.
- [ ] Train regenerates only the selected section.
- [ ] Apply All validates and publishes eligible content.
- [ ] Train Again starts a new training run.
- [ ] Cancel does not destroy previously approved content.

## User Mode

- [ ] Published documents appear in the correct category.
- [ ] The authoritative template is displayed.
- [ ] Published section guidance appears in the existing popup.
- [ ] Draft content is not exposed.
- [ ] Existing document navigation continues to work.

## Admin and Feedback

- [ ] Admin access is role-restricted.
- [ ] User feedback is stored in MySQL.
- [ ] Authorized Editors and Admins can review feedback.
- [ ] Feedback status can be managed.

## Guided Tours

- [ ] Editor Mode has an interactive tutorial.
- [ ] User Mode has an interactive tutorial.
- [ ] Users can skip or close the tour.
- [ ] Users can restart the tour.
- [ ] New tutorial steps can be added modularly.
- [ ] Missing UI targets do not break the tour.

## Overall

- [ ] Existing functionality is preserved.
- [ ] No unnecessary UI redesign has occurred.
- [ ] Database changes are migration-safe.
- [ ] Long-running training operations report real status.
- [ ] Authentication and publishing are secure.
- [ ] The application works across supported screen sizes.

---

# FINAL DEVELOPMENT INSTRUCTION

Build KNOW HOW as a real document knowledge-generation and publishing platform.

The central workflow is:

**Editor Login → Select Existing Document or New → Upload Template → Add Reference Documents → Configure Parameters → Run RAG + LLM Training → Review Section-Wise Drafts → Edit and Regenerate → Approve → Publish to MySQL → Display Approved Content in User Mode.**

Preserve the existing User Mode UI.

Prioritize correctness, traceability, security, and maintainability over decorative effects.

Do not simulate RAG, LLM generation, database writes, authentication, or publishing. Every action must either work through the actual backend or clearly indicate that its integration is pending.

Before completing the task, test the full workflow with both a new document and an existing document, including section regeneration, cancellation, publishing, and User Mode verification.
