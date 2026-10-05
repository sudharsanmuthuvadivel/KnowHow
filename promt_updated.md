# KNOW HOW — UI ENHANCEMENT, DATABASE INTEGRATION & DOCUMENT ACCURACY PROMPT

## ROLE

Assume you are a **Senior Full-Stack Web Developer, Senior Frontend Developer, UI/UX Architect, Frontend System Architect, Document Rendering Engineer, and Enterprise Application Designer** with extensive experience building high-quality enterprise engineering applications.

You have strong expertise in:

* React, TypeScript, Vite
* Tailwind CSS, Framer Motion, HTML/CSS
* Python, FastAPI
* MySQL database integration
* DOCX parsing and rendering
* Excel parsing and data migration
* Document structure extraction and table reconstruction
* Enterprise UI/UX design
* Interactive document viewers
* Responsive layouts and animation design
* AI-ready application architecture
* REST API design and integration

You are continuing development of the existing application:

# KNOW HOW

**IMPORTANT: Do not redesign the existing application from scratch.**

The current overall UI structure, visual language, and three-panel layout are already good. Preserve them and implement the following enhancements and corrections within the existing architecture.

Before making changes, inspect the existing source code, components, APIs, database schema, and current functionality. Reuse existing components and services wherever possible. Avoid unnecessary rewrites.

---

# 1. DOCUMENT PANEL — DOCUMENT CATEGORIZATION

Restructure the existing document navigation panel into two main categories.

### Category 1: Cybersecurity Work Products

This category contains cybersecurity engineering work products, including:

* Cybersecurity Plan
* Cybersecurity Case
* Cybersecurity Interface Agreement
* Other cybersecurity work products added in the future

### Category 2: Cybersecurity Documents / Guides

This category contains reference and guidance documents, including:

* CSPL Handbook
* ISO/SAE 21434 Bulletin
* Other cybersecurity guides and reference documents added in the future

### Document Availability and Lock State

Currently, only **Cybersecurity Plan** is active.

All other documents must remain visible but appear in a locked or disabled state, similar to the existing locked Cybersecurity Case presentation.

For locked documents:

* Display a lock icon or equivalent visual indicator.
* Show a clear "Coming Soon" or "Under Development" label.
* Prevent users from opening unavailable documents.
* Maintain the existing visual style.
* Design the document list so additional documents can be activated later without modifying the overall navigation architecture.

Do not remove or hide future documents simply because they are not yet active.

---

# 2. DOCUMENT OVERVIEW — NAVIGATION AND MAIN CONTENT

When a user selects an active document, its navigation panel must contain the following hierarchy:

1. Overview
2. Important Notes
3. Document Sections

   * Section 1
   * Section 1.1
   * Section 1.2
   * Section 2
   * Remaining sections and subsections

The existing dynamic section extraction and navigation functionality is already implemented.

**Do not unnecessarily modify or break the existing section navigation logic.**

### Overview Placement

The Overview section must always appear first in the navigation panel, before Important Notes and the document sections.

When a document is selected:

* Automatically display its Overview in the main content panel.
* Highlight Overview in the left navigation.
* Keep the remaining document sections available below it.
* Allow users to navigate between Overview, Important Notes, and individual document sections.

### Document-Specific Overview

Each document must have its own Overview content.

For example:

* Selecting Cybersecurity Plan loads the Cybersecurity Plan Overview.
* Selecting Cybersecurity Case loads the Cybersecurity Case Overview when that document becomes active.
* Future documents must follow the same structure.

Do not display the same Overview content for every document unless the database explicitly defines it that way.

---

# 3. MAIN CONTENT PANEL — DATABASE-DRIVEN OVERVIEW

The Overview content is stored in the **MySQL database**.

Retrieve the appropriate Overview content from the database through the existing backend/API architecture.

### Display Requirements

* Render the Overview in the main content panel.
* Present the content using the same typing/typewriter animation currently used for Important Notes.
* Display the content character by character.
* Include a subtle blinking cursor during the animation.
* Keep the animation smooth and readable.
* Do not introduce unnecessary delays.
* Ensure the animation does not block other UI interactions.

### Data Handling

* Do not hardcode Overview text in frontend components.
* Retrieve content based on the selected document.
* Handle empty or missing Overview content gracefully.
* Avoid unnecessary repeated database requests.
* Use appropriate caching where practical.
* Preserve the existing API and database architecture.

The Overview must be fully database-driven so administrators can update its content without modifying frontend code.

---

# 4. DOCUMENT TABLE FORMATTING — CONSISTENCY

The current document rendering must be improved to ensure consistent table typography.

### Requirements

All text within document tables must use a consistent font size and font style.

Apply consistent typography across:

* Table headers
* Table body cells
* All rows
* All columns
* Merged cells
* Nested table content, where applicable

Preserve the existing table structure and formatting as much as possible.

**Do not sacrifice table accuracy for visual consistency.**

The following must remain correct:

* Exact row count
* Exact column count
* Complete cell content
* Cell order
* Merged cells
* Row spans
* Column spans
* Borders
* Alignment
* Table widths
* Horizontal scrolling for wide tables

Do not remove, combine, or invent rows or columns.

---

# 5. GUIDANCE POPUP — MYSQL DATABASE INTEGRATION

### Current Issue

The guidance popup currently retrieves information from `knowledge.xlsx`.

However, the relevant content has now been stored in the MySQL database.

### Required Change

Update the popup data source to retrieve information from MySQL through the backend API.

Do not continue using `knowledge.xlsx` as the runtime source for popup content.

The database must be the authoritative source for:

* Section guidance
* ISO Reference
* Objective
* Evidence Required
* Owner
* Checklist
* Criticality
* Phase

### Important

Before implementing the integration:

1. Inspect the existing MySQL schema.
2. Identify the tables and relationships that contain the required information.
3. Reuse existing database models and APIs where possible.
4. Avoid creating duplicate tables or redundant data.
5. Implement missing API endpoints only when necessary.

Do not assume database table names or column names without inspecting the existing implementation.

### Popup Behavior

Preserve the existing popup design, animations, and section-selection behavior.

Only change the data source and add the required Phase information.

Ensure that selecting a section retrieves the correct database record using its document and section identity.

---

# 6. ADD PHASE INFORMATION BESIDE CRITICALITY

The existing guidance popup displays the section's Criticality.

Add a new field called **Phase**.

The Phase must be displayed next to the Criticality flag near the top of the popup.

Example:

`[ MAJOR ] [ CV ]`

or

`Criticality: Major | Phase: CV`

Supported phase values may include:

* CV
* FC
* QP

Use the actual phase values stored in the database. Do not hardcode the phase of individual sections.

### Design Requirements

* Criticality and Phase must be visually distinct.
* Use compact badges or tags.
* Maintain the existing popup design.
* Ensure both values are readable.
* Handle missing Phase values gracefully.
* Do not display incorrect or inferred phase information.

Preserve the existing Criticality color mapping:

* Mandatory = Red
* Major = Orange
* Moderate = Yellow
* Informational = Green

---

# 7. AI CHATBOT — SECTION-AWARE KNOWLEDGE RETRIEVAL

Enhance the existing AI chatbot architecture so that it can identify and retrieve relevant section knowledge from the database.

### User Query Examples

The user may ask:

* "Explain Section 5."
* "What is Section 5.2?"
* "Tell me about TARA."
* "What is the objective of the Cybersecurity Plan?"
* "What evidence is required for this section?"
* "Explain the purpose of this cybersecurity activity."

The chatbot must support queries that identify a section using:

* Section number only
* Section name only
* Section number and name
* Natural-language descriptions
* Relevant keywords

### Section Identification

When a query refers to a particular section:

1. Identify the intended document, if specified.
2. Identify the section number or section name.
3. Retrieve the corresponding section knowledge from MySQL.
4. Use the retrieved information as context for generating the answer.
5. Provide a clear, relevant response.

If the section is ambiguous, ask the user a brief clarification question instead of selecting an arbitrary section.

### Response Content

The chatbot must not simply copy the database record.

It should provide a useful explanation that combines:

1. **Section-specific information** retrieved from the database.
2. **Relevant general knowledge** about the topic.
3. Clear explanations of technical terms.
4. Practical context or examples when useful.

For example, when a user asks about TARA, the chatbot should explain the relevant section-specific content and provide a general explanation of Threat Analysis and Risk Assessment in the context of automotive cybersecurity.

### Accuracy Requirements

* Do not invent database-specific requirements.
* Do not claim that a statement comes from the database unless it was retrieved.
* Distinguish general explanations from document-specific information when necessary.
* If relevant information is unavailable, communicate that clearly.
* Do not return unrelated section content.
* Preserve document context when answering follow-up questions.

### Architecture

Prepare the chatbot for future integration with:

* Llama
* Retrieval-Augmented Generation (RAG)
* MySQL-based knowledge retrieval
* Document-aware context
* Conversation history
* Semantic search
* Vector database, if required in the future

Do not fake AI responses if the actual model integration is not yet available.

---

# 8. GLOBAL SETTINGS MENU

Add a settings option in the **top-right corner of the application**.

Use a compact three-line, three-dot, or equivalent menu icon consistent with the existing design.

When clicked, display a dropdown menu directly below the settings icon.

### Menu Options

The menu should contain:

1. Mode

   * User Mode
   * Editor Mode
2. Help
3. About (if supported by the existing application)
4. Other future settings, if required

Keep the dropdown compact, clean, and professional.

It must not obstruct the main document unnecessarily.

Close the dropdown when:

* The user selects an option.
* The user clicks outside the menu.
* The user presses Escape.

---

# 9. USER MODE AND EDITOR MODE

Implement a mode-selection mechanism within Settings.

### User Mode

User Mode is intended for engineers who need to:

* View documents
* Navigate sections
* Read Overview content
* Read Important Notes
* View section guidance
* View reference images
* Interact with the AI assistant when available

User Mode must not expose editing controls for protected document or knowledge content.

### Editor Mode

Editor Mode is intended for authorized users who need to maintain application content.

Prepare the interface to support:

* Updating Overview content
* Updating Important Notes
* Editing section guidance
* Updating database-backed content
* Maintaining document information

### Important Security Requirement

Do not treat a frontend mode switch as an authorization mechanism.

If editing functionality is implemented, enforce permissions on the backend as well.

Do not expose database write operations to unauthorized users.

If authentication or role management is not yet available, prepare the UI and architecture without introducing insecure edit access.

Preserve all existing read-only functionality.

---

# 10. HELP MENU — IN-APPLICATION SUPPORT

When the user selects **Help** from the Settings menu, open a dedicated Help interface.

The Help interface should allow users to enter questions about the KNOW HOW application itself.

### Example User Questions

* "How do I navigate between sections?"
* "How do I view the reference image?"
* "Where can I find the section guidance?"
* "What does the Criticality badge mean?"
* "How do I switch between User Mode and Editor Mode?"
* "How do I use the AI chatbot?"
* "How do I provide feedback?"

### Help Interface Requirements

Provide:

* A clear title: "KNOW HOW Help"
* A text input or multiline query field
* A Submit or Search button
* A response area
* A close button
* A clean, professional layout

The Help interface should be visually consistent with the existing application.

### Help Response Logic

The Help feature should answer questions about the application's UI and functionality, not confuse them with automotive cybersecurity knowledge questions.

If an actual help-response engine is not yet implemented:

* Build the interface and service abstraction.
* Do not fabricate dynamic AI answers.
* Display a clear message that Help Assistant functionality is under development.

Prepare the architecture for future integration with an application-specific help knowledge base.

---

# 11. RESPONSIVE DESIGN — CONSISTENT EXPERIENCE

The overall UI must provide a consistent, professional experience across different screen sizes.

Support:

* Large desktop monitors
* Standard desktop screens
* Laptops
* Tablets
* Smaller viewport widths

### Requirements

* Preserve the existing three-panel design wherever practical.
* Maintain consistent visual hierarchy.
* Avoid overlapping panels.
* Prevent content clipping.
* Keep navigation accessible.
* Keep settings and Help accessible.
* Ensure popups fit within the available viewport.
* Allow wide document tables to scroll horizontally.
* Adapt panel widths intelligently when screen space is limited.

Do not simply scale down the entire UI.

Use responsive layout rules while preserving usability and the existing visual identity.

---

# 12. UI/UX DESIGN PRESERVATION

The existing UI is already good.

**Do not replace it with a completely different design.**

Maintain:

* Existing color palette
* Existing typography
* Existing three-panel layout
* Existing navigation behavior
* Existing document rendering
* Existing popup visual language
* Existing animation style
* Existing spacing and component hierarchy

Introduce only the enhancements required by this prompt.

New components must look like natural extensions of the existing application.

Avoid unnecessary visual effects, excessive animations, or unrelated features.

---

# 13. DATABASE AND API INTEGRATION

Inspect the existing backend and MySQL database before modifying the application.

Ensure the following data flows are supported:

### Document Overview

Frontend → Backend API → MySQL → Overview content → Main content panel

### Section Guidance

Frontend → Backend API → MySQL → Section guidance → Popup

### Phase and Criticality

Frontend → Backend API → MySQL → Criticality + Phase → Popup badges

### AI Chatbot

User query → Section identification → Database retrieval → AI context → Response

### Help

User query → Help service or knowledge base → Help response

Use the existing API conventions and error-handling patterns.

Avoid duplicating business logic in the frontend.

Handle loading states, empty results, API failures, and database errors gracefully.

---

# 14. IMPLEMENTATION RULES

Before coding:

1. Inspect the existing project structure.
2. Identify the current frontend framework and component hierarchy.
3. Inspect existing document navigation.
4. Inspect the Overview and Important Notes implementation.
5. Inspect the MySQL schema and existing APIs.
6. Identify the current popup data flow.
7. Identify the existing chatbot architecture.
8. Identify existing settings or menu components.
9. Understand the current responsive behavior.

Then implement the changes incrementally.

### Do Not:

* Rewrite the entire application.
* Remove working functionality.
* Hardcode database content in frontend components.
* Continue using Excel as the runtime source for popup content.
* Invent database schema details.
* Break existing section navigation.
* Change document content unnecessarily.
* Introduce fake chatbot responses.
* Add insecure editing functionality.
* Replace the current UI design without a clear technical need.

### Do:

* Reuse existing components.
* Reuse existing APIs where possible.
* Follow current coding conventions.
* Keep components modular.
* Maintain backward compatibility.
* Add appropriate loading and error states.
* Validate changes before completion.

---

# 15. ACCEPTANCE CRITERIA

The enhancement is complete only when the following requirements are satisfied:

### Document Navigation

* [ ] Two main categories are displayed.
* [ ] Cybersecurity Work Products contains the relevant work products.
* [ ] Cybersecurity Documents / Guides contains the relevant reference documents.
* [ ] Cybersecurity Plan is active.
* [ ] Other unavailable documents display a locked/Coming Soon state.
* [ ] Existing section navigation continues to work.
* [ ] Overview appears first in the section panel.

### Overview

* [ ] Selecting a document displays its own Overview.
* [ ] Overview content is retrieved from MySQL.
* [ ] Overview uses the existing typing animation.
* [ ] Overview is not hardcoded in frontend components.

### Document Rendering

* [ ] Table text uses consistent font size and style.
* [ ] Exact row and column counts are preserved.
* [ ] All cell content remains intact.
* [ ] Merged cells are preserved.
* [ ] Wide tables remain readable.

### Guidance Popup

* [ ] Popup retrieves content from MySQL.
* [ ] Excel is no longer used as the runtime popup source.
* [ ] ISO Reference is displayed.
* [ ] Objective is displayed.
* [ ] Owner is displayed.
* [ ] Checklist is displayed.
* [ ] Criticality is displayed with the correct color.
* [ ] Phase is displayed next to Criticality.
* [ ] Missing values are handled gracefully.

### AI Chatbot

* [ ] Section number queries are supported by the retrieval architecture.
* [ ] Section name queries are supported.
* [ ] Relevant database knowledge is retrieved.
* [ ] Responses can combine section-specific and general explanations.
* [ ] Ambiguous queries are handled appropriately.
* [ ] No fake AI responses are displayed.

### Settings and Help

* [ ] Settings icon appears in the top-right corner.
* [ ] Dropdown opens below the icon.
* [ ] User Mode and Editor Mode options are present.
* [ ] Help option is present.
* [ ] Dropdown closes appropriately.
* [ ] Help opens a dedicated query interface.
* [ ] Help interface is visually consistent with the application.
* [ ] Editing permissions are not controlled by frontend state alone.

### Responsive UI

* [ ] Layout works on desktop and laptop screens.
* [ ] Layout adapts to tablet widths.
* [ ] Panels do not overlap.
* [ ] Content does not clip unexpectedly.
* [ ] Popups remain accessible.
* [ ] Existing visual identity is preserved.

### Overall Quality

* [ ] Existing functionality remains intact.
* [ ] No unnecessary redesign has occurred.
* [ ] Database integration works correctly.
* [ ] API errors are handled gracefully.
* [ ] Code remains modular and maintainable.
* [ ] Application runs successfully.

---

# FINAL INSTRUCTION

Act as a senior engineer enhancing an existing enterprise application, not as a developer creating a new application from scratch.

**Preserve what already works. Improve only what is required.**

Prioritize in this order:

1. Existing application stability
2. Correct database integration
3. Accurate document and section mapping
4. Correct Overview behavior
5. Accurate guidance popup data
6. Phase and Criticality presentation
7. Section-aware chatbot architecture
8. Settings and Help functionality
9. Responsive usability
10. Visual polish

After implementation, test the complete user flow:

**Select Document → View Overview → Navigate to Section → Open Guidance Popup → View Criticality and Phase → Ask Chatbot Question → Open Settings → Switch Mode → Open Help.**

Verify that the enhancements work together without breaking the existing KNOW HOW application.
