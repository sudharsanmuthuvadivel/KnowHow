# KNOW HOW — FAQ, Parameter Prompts, Parsing & AI Generation Enhancements

## Role and principles
Act as a senior full-stack, AI/RAG, database, and UX engineer enhancing the existing KNOW HOW application. Inspect the current codebase first. Do not rebuild the app or redesign working screens. Reuse existing components, APIs, LLM integration, and database patterns.

Implement real end-to-end functionality. Do not use mock data, placeholder FAQs, simulated parsing, fake progress, or false success messages. Ground all generated content in actual interpreted documents and approved knowledge sources. Preserve source traceability, versioning, editor approval, and role-based access.

## 1. Settings: Help / Feedback → FAQ / Feedback
Rename the top-right Settings option **Help / Feedback** to **FAQ / Feedback**. Selecting it should show two choices: **FAQ** and **Feedback**. Preserve the existing feedback workflow.

### FAQ user flow
1. Settings → FAQ / Feedback → FAQ.
2. Show active documents available to the logged-in user.
3. User selects a document.
4. Show FAQs associated with that document.
5. Provide a search box that filters results automatically as the user types.

Search across FAQ question, answer, section title/ID, parameter name, keywords, and tags. Support case-insensitive partial matching for words, phrases, abbreviations, and cybersecurity terms. Use simple local/database filtering for ordinary FAQ search; do not require an LLM call for every keystroke.

Show the selected document name, a way back to the document list, total FAQ count, matching result count, and FAQs in a readable accordion/expandable list. Highlight matching terms where practical. Provide loading, error, and “No matching FAQs found” states. Only show FAQs for active documents and permitted users. User Mode must not expose draft or unpublished FAQs.

## 2. Create at least 100 FAQs per document
For each active/registered document (e.g., Cybersecurity Plan, Cybersecurity Case, CSPL Handbook, ISO/SAE 21434 Handbook, CIA, and other configured documents), create and persist at least 100 distinct, useful FAQs per document **where the source material supports that coverage**.

Cover applicable topics:
- Purpose, overview, scope, objectives, and section intent
- Cybersecurity concepts, terminology, abbreviations, and standards
- Criticality, lifecycle phase, owner, roles, and responsibilities
- Work products, required evidence, evidence quality, and preparation
- Inputs, outputs, dependencies, related sections, clauses, and references
- Common mistakes, assessor feedback themes, and review expectations
- Practical examples and reusable best practices

Ground every FAQ answer in the actual document and approved reference sources. Link FAQs to document, section, parameter, topic/tags, and source location where possible. Avoid duplicates, near-duplicates, generic filler, and unsupported answers. Never invent ISO requirements, company processes, owners, criticality, phases, or evidence. If a source does not answer a question, omit it or clearly flag the information as unspecified and requiring confirmation.

Add an Editor Mode option such as **FAQ Management / Generate FAQs**:
- Editor selects a document and enters the desired FAQ count.
- The workflow should later use the configured RAG + LLM service to generate additional FAQs.
- For the initial implementation, create and store the 100 source-grounded FAQs per document using available source material; do not defer this task to a future feature.
- If there is insufficient source material for 100 distinct FAQs, report the actual count and coverage gap instead of fabricating FAQs.
- Editors can review, edit, approve, publish, archive, and regenerate individual FAQs according to permissions.
- Persist actual counts and lifecycle status (Draft, Approved, Published, Archived).
- Make FAQ creation idempotent so reruns do not create duplicates.

## 3. Editor Mode: Parameter requirements during section training
When an editor trains/interprets a section, add **Parameter Requirements** for each parameter, for example Overview, Objective, Scope, Detailed Information, Owner, Criticality, Phase, Evidence Required, Evidence Description, Inputs/Outputs, Related Sections, and Applicable References.

For each parameter allow the editor to enter:
- Requirement/instruction describing what the parameter must contain
- Additional context and constraints
- Example output
- Expected format/length
- Evidence expectations, where applicable

### Prompt preparation and approval workflow
1. Retain the editor’s original input.
2. Convert it into a clear, structured LLM-ready prompt.
3. Show both original input and converted prompt to the editor.
4. Allow the editor to edit the converted prompt.
5. Require explicit approval for that parameter.
6. Only after approval, send the approved prompt plus relevant RAG context to the LLM.
7. Show generated content for editor review, editing, approval, rejection, or regeneration.
8. Persist original input, converted prompt, prompt version, approval metadata, generated content, and source references.

Prompt conversion must preserve the editor’s intent and must not silently add unsupported requirements. It should clearly state the parameter purpose, expected scope, output format, constraints, evidence expectations, source-grounding rules, and expected length. Instruct the LLM not to invent missing facts and to flag gaps or conflicts. Target approximately 4–5 meaningful lines per parameter unless the editor specifies otherwise.

The editor must be able to see and edit the converted prompt. Do not generate final content for a parameter until its prompt has been approved.

## 4. Apply the same prompt workflow to new-document interpretation
The existing new-document interpretation flow already accepts editor input for each parameter. Improve it to use the same process:
- Convert each parameter’s editor input into an LLM-ready prompt.
- Display original input and converted prompt.
- Allow editing and explicit per-parameter approval.
- Track approval separately for every parameter; approving one must not approve all.
- Save prompt versions and associate them with document interpretation version, section, and parameter.
- Allow prompt revision before generation.
- If source documents change, identify potentially stale prompts/content and request review rather than silently reusing stale output.

## 5. Improve reference-document parsing

### 5.1 Comprehensive extraction
Parse all supported relevant content, not just headings or selected snippets: paragraphs, headings, tables, lists, captions, headers/footers, and structured content supported by the file format/parser. Preserve order, hierarchy, page number and source location where available. Do not silently skip content in tables or nested sections. Report unsupported content and extraction failures. Avoid duplicate chunks on reprocessing.

### 5.2 Abbreviation and acronym detection
Detect abbreviations such as CV, FC, CSPL, CSM, and other short forms:
- Capture the full form when explicitly defined in the source.
- Store the definition, context, document, section, and source location.
- Build a searchable document-level glossary.
- Track multiple meanings when context differs.
- Do not guess an expansion when it is not supported; mark it unresolved for editor review.
- Make approved glossary entries available to RAG retrieval, FAQ generation, and content generation.

### 5.3 Cross-reference extraction and recursive resolution
Identify references to sections, clauses, tables, annexes, appendices, and related documents, such as “Refer to Section 5.2”, “See Table 3”, or “As defined in Clause 9”.

Resolve relevant referenced content recursively to a maximum traversal depth of **four levels**:
- Level 0: Current section/parameter
- Level 1: Directly referenced content
- Level 2: References found in Level 1
- Level 3: References found in Level 2
- Level 4: References found in Level 3

Requirements:
- Resolve only when the target can be identified confidently.
- Maintain a visited-reference set to prevent circular references and repeated traversal.
- Never exceed depth four.
- Preserve the reference chain and source locations.
- Include relevant referenced passages in generation context, clearly distinguished from primary section content.
- Flag unresolved/ambiguous references for editor review.
- Do not include unrelated content merely because a reference is ambiguous.
- Support same-document references and explicitly identified cross-document references.
- Keep document versions separate; do not mix obsolete and current versions without an explicit rule.

Store source document/version, extracted text, content type, hierarchy, location, abbreviation records, cross-reference source/target, traversal depth, resolution status, parsing status/errors, and deduplication hash as appropriate to the existing schema.

## 6. Improve RAG context assembly and content generation
Before generating each parameter, assemble and analyze:
1. Approved parameter prompt
2. Current document and section context
3. Relevant extracted content from the interpreted document
4. Relevant content from selected reference documents
5. Ranked RAG-retrieved passages
6. Resolved cross-reference content up to depth four
7. Relevant approved abbreviation definitions
8. Source-supported metadata such as owner, phase, criticality, and evidence expectations
9. Source identifiers and locations for traceability

Generate approximately **4–5 meaningful lines per parameter**, including Overview, Objective, Scope, and Detailed Information. This is a target, not permission to add filler. Keep each parameter focused and avoid repeating the same content across fields. Use precise automotive cybersecurity terminology. Distinguish mandatory requirements from recommendations, examples, and explanatory guidance. Do not treat an example as a binding requirement unless the source says so.

Never invent owners, criticality ratings, phases, evidence, requirements, ISO clauses, or implementation details. If information is missing, flag the gap or request editor input. If sources conflict, show the conflict and cite both sources for editor resolution. Do not generate content based only on a parameter name or generic prompt.

For each generated parameter retain source document/version, section/page location where available, retrieved passage IDs, cross-reference chain, abbreviation definitions used, approved prompt version, generation timestamp, and model/service metadata where available. Provide editors a way to inspect the sources used.

## 7. Database, API, security, and lifecycle
Inspect the existing schema before changes. Reuse current models and endpoints where practical. Add migrations only when required.

Support the necessary relationships for:
- FAQs, document/section/parameter associations, tags, status, versions, and provenance
- FAQ count requests and generation jobs
- Original editor input, parameter requirements, converted prompt versions, and approvals
- Generated parameter content and editor approval
- Parsed source content, source locations, abbreviations, and cross-reference links
- RAG chunks, retrieval metadata, document versions, and source associations

Enforce authorization on the backend, not just in the UI. Validate all inputs server-side. Use safe database access, transactions for related changes, secure configuration for secrets, and audit history for prompt edits, approvals, FAQ changes, and publishing. Prevent duplicate FAQs/chunks/references on reprocessing. Preserve approved/published versions. Make long-running processing recoverable and report real status. Do not expose unpublished material in User Mode. Do not claim success until persistence is confirmed.

## 8. UI/UX
Preserve the existing KNOW HOW design system and three-panel layout. Integrate FAQ navigation into Settings without disrupting document navigation. Make the prompt workflow visually clear:

**Editor input → Converted prompt → Editor approval → RAG/LLM generation → Content review → Publish**

Show clear states such as Pending Approval, Approved, Generated, Needs Review, Published, and Failed. Provide loading, empty, error, and retry states. Keep the UI responsive and accessible. Follow existing modal/panel conventions and avoid unnecessary pop-ups.

## 9. Acceptance checklist

### FAQ
- [ ] Settings label is FAQ / Feedback, with separate FAQ and Feedback choices.
- [ ] FAQ lists active documents available to the user.
- [ ] Selecting a document loads only its FAQs.
- [ ] Search filters questions, answers, sections, parameters, and keywords as the user types.
- [ ] Search is case-insensitive and supports partial matches.
- [ ] Empty, loading, error, and no-result states work.
- [ ] At least 100 distinct, grounded FAQs are stored per document where sources support them; shortfalls are reported honestly.
- [ ] FAQs are associated with correct documents and sections and include provenance where possible.
- [ ] Editors can manage FAQ lifecycle; User Mode sees only published FAQs.
- [ ] Re-running FAQ creation does not create duplicates.

### Prompts and generation
- [ ] Editors can define requirements for each section parameter.
- [ ] Original input and converted prompt are both visible.
- [ ] Converted prompt is editable and requires explicit per-parameter approval.
- [ ] Generation is blocked until that parameter’s prompt is approved.
- [ ] New-document interpretation uses the same workflow.
- [ ] Generation uses approved prompts and relevant parsed/RAG context.
- [ ] Output is approximately 4–5 meaningful lines per parameter, without filler or repetition.
- [ ] Missing/conflicting information is flagged, not invented.
- [ ] Editors can inspect source traceability before approval/publication.

### Parsing and engineering
- [ ] Supported paragraphs, tables, lists, headings, and other relevant content are extracted.
- [ ] Abbreviations are detected and linked to definitions/source locations; unresolved items are flagged.
- [ ] Cross-references are resolved recursively to a maximum depth of four.
- [ ] Circular references are prevented; unresolved references are surfaced.
- [ ] Backend authorization, validation, persistence, and audit behavior are tested.
- [ ] Existing User Mode and Feedback functionality remain functional.
- [ ] Relevant unit/integration/UI tests, type checks, linting, and production build are run; actual failures are reported.

## 10. Implementation sequence
1. Inspect current FAQ, feedback, Editor Mode, parsing, RAG, LLM, database, and authorization architecture.
2. Identify root causes and propose minimal schema/API changes.
3. Implement FAQ navigation, document selection, search, and persistence.
4. Create the initial source-grounded FAQ library; report any source-coverage shortfall rather than fabricating records.
5. Implement parameter requirements and prompt conversion/review/approval.
6. Apply the same prompt workflow to new-document interpretation.
7. Improve parsing, abbreviation extraction, and cross-reference resolution.
8. Improve RAG context assembly and parameter-level generation.
9. Test the full frontend → backend → parsing/RAG → LLM → database → refreshed UI workflow.

## Expected final report
Provide root causes and architecture findings; changed files; database migrations and API endpoints; FAQ count per document and any coverage gaps; parsing results including abbreviations and resolved/unresolved references; tests/build results; and remaining limitations/deployment steps.

**Final instruction:** Build on the existing KNOW HOW application. Keep FAQs, prompts, parsed content, and generated parameter outputs grounded in real sources. Never fabricate source facts or claim processing, generation, approval, or persistence succeeded unless the backend confirms it.
