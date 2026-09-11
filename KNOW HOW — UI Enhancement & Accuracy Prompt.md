# KNOW HOW — UI ENHANCEMENT & DOCUMENT ACCURACY PROMPT

## ROLE

Assume you are a **Senior Full-Stack Web Developer, Senior Frontend Developer, UI/UX Architect, Frontend System Architect, Document Rendering Engineer, and Enterprise Application Designer** with extensive experience in building high-quality enterprise engineering applications.

You have strong expertise in:

- React
- TypeScript
- Vite
- Tailwind CSS
- Framer Motion
- HTML/CSS
- Python
- FastAPI
- DOCX parsing and rendering
- Excel parsing
- Document structure extraction
- Table reconstruction
- Enterprise UI/UX
- Interactive document viewers
- Animation design
- AI-ready application architecture

You are continuing development of the existing application:

# KNOW HOW

Do **not redesign the existing application from scratch**.

The current overall UI structure is already good and must be preserved.

Make the following enhancements and corrections while maintaining the existing architecture, visual language, and three-panel structure.

---

# 1. EXISTING UI STRUCTURE — KEEP IT

Maintain the current three-panel layout:

```text
┌─────────────────────┬────────────────────────────────┬─────────────────────┐
│                     │                                │                     │
│ DOCUMENT +          │       MAIN DOCUMENT            │     AI CHATBOT      │
│ SECTION PANEL       │       CONTENT PANEL            │                     │
│                     │                                │                     │
│                     │                                │                     │
└─────────────────────┴────────────────────────────────┴─────────────────────┘
```

### Left

**Document / Section Navigation Panel**

### Center

**Main Document Content Panel**

### Right

**AI Chatbot Panel**

Do not change this fundamental structure.

---

# 2. PRIMARY OBJECTIVE OF THIS UPDATE

The most important objective is:

> **The KNOW HOW UI must represent the source Template DOCX accurately, especially section numbering, tables, rows, columns, and table contents.**

Do not approximate the document structure.

Do not simplify tables.

Do not remove rows or columns.

Do not create visually similar but structurally incorrect tables.

The actual template document is the source of truth.

---

# 3. SECTION NUMBER — CRITICAL UPDATE

Currently:

- Section name is visible.
- Section number is missing.

Fix this in **both the left section panel and the main content panel**.

---

## 3.1 LEFT SECTION PANEL

Every section displayed in the section navigation must contain its actual section number.

Example:

```text
01  Project Name
02  Document Information
03  Revision History
04  Purpose
05  Scope
06  Audience
07  Related Documents
08  Cybersecurity Objective
09  Cybersecurity Management Scope
...
```

However, **do not hardcode these numbers**.

The application must extract the section number from the actual Template DOCX.

If the document contains:

```text
1. Project Name
2. Document Information
3. Revision History
```

the UI must show exactly those numbers.

If the document contains:

```text
1
1.1
1.2
2
2.1
```

preserve that hierarchy.

If the template contains:

```text
5. Cybersecurity Concept
5.1 Cybersecurity Goals
5.2 Cybersecurity Requirements
```

display exactly:

```text
5    Cybersecurity Concept
5.1  Cybersecurity Goals
5.2  Cybersecurity Requirements
```

The DOCX is the authoritative source.

---

# 4. SECTION NUMBER — MAIN CONTENT PANEL

The section number must also appear in the main content panel.

Example:

```text
08
CYBERSECURITY OBJECTIVE
```

or, if the template uses:

```text
8. Cybersecurity Objective
```

render:

```text
8. CYBERSECURITY OBJECTIVE
```

The exact numbering format should follow the Template DOCX.

Do not invent a different numbering system.

---

# 5. SECTION NUMBER CONSISTENCY

The following three representations must always correspond:

```text
Template DOCX
      ↓
Section Navigation
      ↓
Main Content Section
```

For example:

```text
Template:
8. Cybersecurity Objective

Left Panel:
8  Cybersecurity Objective

Main Panel:
8. CYBERSECURITY OBJECTIVE
```

There must never be a situation where:

```text
Left panel = 7
Main panel = 8
```

or where one panel has a number and another does not.

---

# 6. SECTION NAME COLOR — MAIN CONTENT

The section heading in the main content panel is already good.

Keep its existing typography and structure, but change the section heading color to a professional **dark blue**.

Example:

```text
8. CYBERSECURITY OBJECTIVE
```

Use a dark-blue enterprise tone.

The color should be:

- Professional
- Easy to read
- High contrast
- Consistent throughout the application

Do NOT use bright blue.

Do NOT use neon blue.

Do NOT use excessive glow.

The section heading should feel similar to a professional engineering document.

---

# 7. MAIN CONTENT PANEL — DOCUMENT ACCURACY

This is the **highest-priority correction**.

The current table rendering is not sufficiently accurate.

Many tables currently have:

- Missing rows
- Missing columns
- Incorrect column count
- Incorrect row count
- Incorrect merged cells
- Incorrect cell widths
- Incorrect cell heights
- Missing table content
- Incorrect alignment
- Incorrect borders
- Incorrect cell structure

All of these must be corrected.

---

# 8. TABLE RENDERING — SOURCE OF TRUTH

The Template DOCX must be treated as the **single source of truth**.

For every table:

```text
Template DOCX
      ↓
Extract exact table structure
      ↓
Extract rows
      ↓
Extract columns
      ↓
Extract merged cells
      ↓
Extract cell content
      ↓
Extract formatting
      ↓
Render accurately in UI
```

Do NOT manually recreate tables.

Do NOT estimate the number of rows.

Do NOT estimate the number of columns.

Do NOT assume all tables have the same structure.

---

# 9. EXACT TABLE ROW COUNT

For every table in the Template DOCX:

```text
DOCX row count
=
UI row count
```

Example:

If the original table contains:

```text
10 rows
```

the UI must contain:

```text
10 rows
```

If the original table contains:

```text
25 rows
```

the UI must contain:

```text
25 rows
```

No rows may be silently removed.

---

# 10. EXACT TABLE COLUMN COUNT

For every table:

```text
DOCX column count
=
UI column count
```

Example:

If the template has:

```text
6 columns
```

the UI must have exactly:

```text
6 columns
```

Do not collapse columns simply because the screen is smaller.

Use horizontal scrolling where required.

---

# 11. TABLE CONTENT ACCURACY

Every table cell must be populated from the actual DOCX.

For example:

```text
DOCX

| ID | Requirement | Owner | Status |
|----|-------------|-------|--------|
| 01 | ...         | ...   | ...    |
| 02 | ...         | ...   | ...    |
```

The UI must reproduce:

```text
| ID | Requirement | Owner | Status |
|----|-------------|-------|--------|
| 01 | ...         | ...   | ...    |
| 02 | ...         | ...   | ...    |
```

Do not:

- Replace content with "...".
- Remove long text.
- Truncate important information.
- Merge unrelated cells.
- Replace tables with cards.

---

# 12. MERGED CELLS

This is extremely important.

If the original DOCX contains merged cells:

```text
┌───────────────┬───────┬───────┐
│               │ A     │ B     │
│  Merged Cell  ├───────┼───────┤
│               │ C     │ D     │
└───────────────┴───────┴───────┘
```

the UI must preserve the merged-cell structure.

Support:

- Horizontal merges
- Vertical merges
- Multi-row merges
- Multi-column merges

Do not render merged cells as separate independent cells.

---

# 13. TABLE WIDTH AND RESPONSIVENESS

Do not distort tables to force them into the available width.

If a table is wider than the center panel:

```text
┌──────────────────────────────┐
│ Table                         │
│                              │
│ ← horizontally scrollable →  │
└──────────────────────────────┘
```

Use horizontal scrolling.

Maintain:

- Column proportions
- Cell alignment
- Readability
- Original structure

The user must be able to inspect the complete table.

---

# 14. TABLE TEXT

Long text inside table cells must remain readable.

Use:

```text
word-wrap
overflow-wrap
appropriate line-height
```

Do not hide text.

Do not use aggressive ellipsis such as:

```text
This requirement is responsible for...
```

unless the user explicitly chooses a compact view.

Default behavior should prioritize document accuracy.

---

# 15. TABLE FORMATTING

Where available from the DOCX, preserve:

- Borders
- Border thickness
- Cell background
- Font
- Font size
- Bold
- Italic
- Text alignment
- Vertical alignment
- Paragraph spacing
- Cell padding
- Merged cells
- Row height
- Column width

The resulting table should visually resemble the original template.

---

# 16. DOCX PARSER IMPROVEMENT

If the existing DOCX parser only extracts plain text, improve it.

The parser should build a structured representation such as:

```json
{
  "type": "table",
  "rows": [
    {
      "cells": [
        {
          "text": "Requirement",
          "rowSpan": 1,
          "colSpan": 2
        }
      ]
    }
  ]
}
```

Support:

```text
rowSpan
colSpan
cell formatting
paragraphs
runs
text formatting
```

Do not use a simple:

```text
table.rows.map(...)
```

implementation if it loses merged-cell information or formatting.

---

# 17. TABLE VALIDATION

Create an internal validation mechanism.

For every extracted table:

```text
Template table:
Rows = X
Columns = Y

Rendered table:
Rows = X
Columns = Y
```

The application should be able to detect mismatches during development.

Example development log:

```text
Table #07
Expected:
Rows: 12
Columns: 6

Rendered:
Rows: 12
Columns: 6

Status: PASS
```

If there is a mismatch:

```text
Status: FAIL
```

This validation is for development/debugging and does not necessarily need to be visible to normal users.

---

# 18. POPUP BOX — CRITICALITY

Add a **Criticality tag/flag** to the section popup.

The criticality value is available in:

```text
knowledge.xlsx
```

The Criticality field appears **after the Checklist column** in the Excel data structure.

Example:

```text
Section | Objective | Evidence Required | Owner | Checklist | Criticality
```

Read the actual Excel header rather than relying only on a fixed column index.

---

# 19. CRITICALITY TAG LOCATION

The criticality indicator should appear in a suitable position at the **top-left or top-right corner of the popup**.

Recommended layout:

```text
┌─────────────────────────────────────────────────┐
│  OBJECTIVE                     🔴 MANDATORY     │
│                                                 │
│  Objective content...                           │
│                                                 │
│  OWNER                                          │
│  Owner content...                               │
│                                                 │
│  CHECKLIST                                      │
│  ✓ Checklist item                               │
│  ✓ Checklist item                               │
└─────────────────────────────────────────────────┘
```

The tag must be visually noticeable without dominating the popup.

---

# 20. CRITICALITY COLORS

Use the following semantic color mapping:

### Mandatory

```text
RED
```

Meaning:

> Most important / mandatory compliance or action.

### Major

```text
ORANGE
```

Meaning:

> Important section requiring attention.

### Moderate

```text
YELLOW
```

Meaning:

> Lower priority but still relevant.

### Informational

```text
GREEN
```

Meaning:

> Informational/reference-only content.

Use professional muted shades rather than excessively bright colors.

The color should work with the overall enterprise theme.

---

# 21. CRITICALITY TAG STYLE

Example:

```text
● MANDATORY
```

or:

```text
[ MANDATORY ]
```

Use:

- Rounded pill/tag
- Small icon if appropriate
- Strong but professional typography
- Semantic color
- Subtle shadow/glow

Do not use a huge banner.

---

# 22. POPUP INTERNAL STRUCTURE

The popup should no longer display all information as one large block.

Each information category must be represented as an independent container.

Example:

```text
┌──────────────────────────────────────┐
│ Section Name          🔴 MANDATORY   │
├──────────────────────────────────────┤
│                                      │
│  ISO REFERENCE                       │
│  ┌────────────────────────────────┐  │
│  │ ISO/SAE 21434 Clause 15        │  │
│  └────────────────────────────────┘  │
│                                      │
│  OBJECTIVE                           │
│  ┌────────────────────────────────┐  │
│  │ ...                            │  │
│  └────────────────────────────────┘  │
│                                      │
│  OWNER                               │
│  ┌────────────────────────────────┐  │
│  │ ...                            │  │
│  └────────────────────────────────┘  │
│                                      │
│  CHECKLIST                           │
│  ┌────────────────────────────────┐  │
│  │ □ ...                          │  │
│  │ □ ...                          │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
```

---

# 23. POPUP INFORMATION CONTAINERS

Support separate containers for:

```text
ISO Reference
Objective
Owner
Checklist
```

If an additional supported field exists in the Excel structure, design the data model so it can be added later.

Do not hardcode the UI so that adding new metadata becomes difficult.

---

# 24. CONTAINER SEQUENTIAL ANIMATION

When a user selects a section:

```text
Section selected
       ↓
Popup opens
       ↓
ISO Reference container appears
       ↓
Objective container appears
       ↓
Owner container appears
       ↓
Checklist container appears
```

The containers should appear **one after another quickly**.

The animation should feel:

- Smooth
- Fast
- Responsive
- Premium

Do not make users wait several seconds.

Recommended animation:

```text
100–200 ms per container
```

Adjust based on actual UI feel.

---

# 25. JELLY / SOFT SHAKE EFFECT

Each information container can have a very subtle "jelly" or soft bounce effect when it appears.

Example:

```text
scale:
0.96
 ↓
1.03
 ↓
0.99
 ↓
1.00
```

The effect should be:

- Subtle
- Professional
- Smooth

Do NOT make it look like a children's website.

Do NOT continuously shake the containers.

The jelly effect occurs only during appearance/activation.

---

# 26. POPUP VISUAL DESIGN

The popup should look:

**Futuristic + Enterprise + Automotive + Cybersecurity + Professional**

Use:

- Glassmorphism where appropriate
- Subtle gradients
- Fine borders
- Soft shadows
- Dark/light contrast
- Semantic colors
- Controlled glow
- Smooth animation

Avoid:

- Excessive neon
- Hacker-style green text
- Gaming aesthetics
- Excessive particles
- Excessive animation

---

# 27. POPUP SCROLLING

The popup must remain scrollable when content is large.

Use:

```text
max-height
overflow-y: auto
```

Do not allow the popup to cover the entire application unnecessarily.

The user should still see the main document behind/around it.

---

# 28. USER FEEDBACK / FUTURE IMPROVEMENTS PANEL

Add a separate small panel for:

```text
User Feedback
Future Improvements
Upcoming Features
```

This is independent of the AI chatbot.

---

# 29. FUTURE UPDATE PANEL LOCATION

Place it in a suitable top corner of the application.

Recommended:

```text
Top-right corner
```

or:

```text
Top-left corner
```

depending on which location best fits the existing header.

Do not interfere with:

- Document navigation
- Main document
- AI chatbot
- Section popup

---

# 30. FUTURE UPDATE PANEL APPEARANCE

It can be a small floating button/card.

Example:

```text
┌──────────────────────────────┐
│ ✨ Upcoming Improvements      │
│                              │
│ This feature is under        │
│ progress and will be         │
│ updated soon.                │
│                              │
│              View Details →  │
└──────────────────────────────┘
```

Or a compact version:

```text
✨ Updates
Under progress — will update soon
```

---

# 31. FUTURE UPDATE PANEL BEHAVIOR

The panel should be:

- Non-intrusive
- Dismissible
- Reopenable
- Animated subtly

Example:

```text
✨
```

floating indicator.

When clicked:

```text
Future Improvements
```

panel expands.

---

# 32. FUTURE UPDATE CONTENT

For the current version, show:

```text
This feature is under progress.
Will update soon.
```

The architecture should allow future entries such as:

```text
✓ Llama AI Integration
✓ Advanced Document Search
✓ Automated Requirement Analysis
✓ Cybersecurity TARA Assistance
✓ Document Version Comparison
✓ AI-based Checklist Review
```

Do not implement these features yet unless already available.

---

# 33. AI CHATBOT

Maintain the existing right-side AI chatbot panel.

The AI chatbot should still be designed as a future-ready feature.

Current state:

```text
AI Assistant

AI integration is currently
under progress.

Will be enabled soon.
```

Do not pretend that the AI is already operational.

The future chatbot must eventually be capable of answering questions about:

- Current section
- Other sections
- Entire Cybersecurity Plan
- Other cybersecurity documents
- knowledge.xlsx
- Reference information
- Document relationships

---

# 34. VISUAL PRIORITY

The UI hierarchy should be:

```text
                    KNOW HOW
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
      Navigation     Document      AI
          │            │            │
          │            │            │
       Section      Main Focus    Assistant
```

The document must remain the primary focus.

Do not allow popup animations or future-update notifications to dominate the interface.

---

# 35. ANIMATION PRINCIPLES

Use animations intentionally.

Good animations:

- Smooth section scrolling
- Section selection highlight
- Popup opening
- Information container sequential appearance
- Subtle jelly effect
- Reference image expansion
- Future update panel opening

Avoid:

- Continuous motion
- Excessive glowing
- Flashing
- Long delays
- Distracting animations

The application should feel premium, not animated for the sake of animation.

---

# 36. ACCESSIBILITY

Ensure:

- Sufficient text contrast
- Keyboard navigation
- Visible focus state
- Accessible close buttons
- Tooltips where necessary
- Screen-reader-friendly labels
- Semantic HTML where practical

Do not rely only on color to communicate criticality.

For example:

```text
🔴 MANDATORY
```

is better than only showing a red dot.

---

# 37. IMPORTANT DATA SOURCE RULE

Keep source responsibilities strictly separated.

```text
Template DOCX
     ↓
Document structure
Section numbers
Headings
Paragraphs
Tables
Rows
Columns
Formatting

knowledge.xlsx
     ↓
ISO Reference
Objective
Owner
Checklist
Criticality
Other guidance metadata

Reference-image/
     ↓
Visual reference input

AI
     ↓
Future intelligent assistance
```

Do not mix these sources.

---

# 38. DO NOT HARD-CODE DOCUMENT STRUCTURE

Do NOT write:

```text
section 1 = Project Name
section 2 = Document Information
section 3 = Revision History
```

Instead:

```text
DOCX
 ↓
Parser
 ↓
Heading detection
 ↓
Section number extraction
 ↓
Section hierarchy
 ↓
UI
```

The application must continue working if the template changes.

---

# 39. DO NOT HARD-CODE TABLE STRUCTURE

Do NOT write:

```text
if table == X:
    render 5 rows
```

Instead:

```text
DOCX table
 ↓
Extract actual rows
 ↓
Extract actual columns
 ↓
Extract merged cells
 ↓
Extract cell contents
 ↓
Render dynamically
```

Every table should be generated from actual source data.

---

# 40. REGRESSION PROTECTION

While making these improvements:

**Do not break existing functionality.**

Verify that the following continue working:

- Document selection
- Section navigation
- Important Notes
- Typing animation
- Section popup
- Reference image expansion
- Independent close buttons
- Section toggle
- AI panel
- Future update panel
- Document scrolling

---

# 41. ACCEPTANCE TEST — SECTION NUMBER

### Test

Open Cybersecurity Plan.

Expected:

```text
LEFT PANEL

8  Cybersecurity Objective
```

and:

```text
MAIN PANEL

8. CYBERSECURITY OBJECTIVE
```

The number must come from the Template DOCX.

---

# 42. ACCEPTANCE TEST — TABLE

For every table in the template:

Compare:

```text
Template DOCX
vs
KNOW HOW UI
```

Verify:

```text
✓ Same number of rows
✓ Same number of columns
✓ Same merged cells
✓ Same cell content
✓ Same table hierarchy
✓ Same headers
✓ Same formatting as technically possible
```

No missing rows or columns are acceptable.

---

# 43. ACCEPTANCE TEST — CRITICALITY

If Excel contains:

```text
Criticality = Mandatory
```

display:

```text
🔴 MANDATORY
```

If:

```text
Major
```

display:

```text
🟠 MAJOR
```

If:

```text
Moderate
```

display:

```text
🟡 MODERATE
```

If:

```text
Informational
```

display:

```text
🟢 INFORMATIONAL
```

---

# 44. ACCEPTANCE TEST — POPUP ANIMATION

When a section is selected:

```text
Popup opens
     ↓
ISO Reference container
     ↓
Objective container
     ↓
Owner container
     ↓
Checklist container
```

All should appear quickly with subtle motion.

No excessive delay.

---

# 45. ACCEPTANCE TEST — FUTURE UPDATE PANEL

The application must contain a separate small future-update panel showing:

```text
This feature is under progress.
Will update soon.
```

It must not interfere with the main document.

---

# 46. FINAL QUALITY REQUIREMENT

Before considering the task complete, compare the actual Template DOCX and rendered KNOW HOW UI.

Pay special attention to:

```text
1. Section numbering
2. Section names
3. Heading hierarchy
4. Tables
5. Row count
6. Column count
7. Merged cells
8. Cell contents
9. Table formatting
10. Typography
11. Alignment
12. Spacing
13. Document hierarchy
```

If a table in the UI does not match the template, fix the document parser/renderer rather than manually adjusting the individual table.

---

# 47. FINAL PRODUCT VISION

KNOW HOW should look like a:

> **Premium Automotive Cybersecurity Engineering Workspace**

It should combine:

```text
Official Document
       +
Engineering Guidance
       +
Criticality
       +
Reference Inputs
       +
Future AI Assistance
       +
Professional Enterprise UX
```

The final experience should be:

**Accurate**
**Professional**
**Futuristic**
**Enterprise-grade**
**Technically credible**
**Easy to navigate**
**Visually impressive**

Most importantly:

> **Document accuracy comes before visual effects.**

The application should never sacrifice the actual structure, content, rows, columns, or formatting of the Template DOCX merely to make the UI look attractive.

Build the UI around the real input files and validate the implementation against the actual source documents.