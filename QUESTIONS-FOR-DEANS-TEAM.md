# Questions for the Dean's Team
*Curriculum Analyzer — Requirements Clarification*
*October 2026*

---

## Accreditation & Compliance

1. **What are our current AACSB Assurance of Learning goals and objectives?**
   The tool tags against DESB's 11 Essential Skills and 8 NACE competencies, but AACSB requires mapping to the school's specific AoL plan. Do our internal skill labels map 1:1 to AoL goals, or do we need a separate layer of alignment?

2. **Does AACSB require a minimum number of credit hours — not just courses — to demonstrate coverage of each learning goal?**
   The tool now extracts credit hours per section. The Coverage Analysis currently counts courses; if AACSB reviewers require credit-hour-weighted calculations, that weighting can be applied to the existing data.

3. **What is the review cycle for AoL reporting, and who owns it?**
   Knowing whether this tool needs to produce a formal report annually, every three years, or on a different cycle shapes how much rigor the export format needs to have.

4. **Are there specific skill coverage minimums we're already committed to — either internally or with AACSB?**
   For example: "Every required core course must address at least one Essential Skill at Secondary or Primary intensity." If those targets exist, they should drive the gap threshold in the tool rather than a manually guessed number.

5. **Which skills or competencies are currently under scrutiny from AACSB or internal review?**
   If there are known weak spots — for example, Quantitative Literacy or Technology & Digital Literacy — those should be prioritized in how the tool surfaces gaps.

---

## Programs & Course Structure

6. **Which programs do we need to track separately — BSBA, MBA, MAcc, MS programs, certificates?**
   The tool currently organizes by department and filters by department in both the Dashboard and Coverage Analysis. An Associate Dean typically needs to answer accreditation questions at the program level, not the department level. A course can serve multiple programs — how should we handle that mapping?

7. **Do we have a current list of required core courses for each program?**
   Required vs. elective is a fundamental distinction for curriculum planning. A gap in a required course is far more significant than a gap in an elective. Is this list maintained somewhere we can import or reference?

8. **Who decides when a course's required/elective status changes, and how is that communicated?**
   If a course moves from required to elective mid-year, the coverage analysis could change significantly. Is there a process or system that tracks this?

9. **How do we handle courses that serve multiple programs with different requirements?**
   For example, if MGMT 3500 is required for BSBA but elective for MBA, should it count toward the required-course coverage for both, or only for BSBA?

10. **Are there cross-listed courses or shared syllabi between departments or colleges?**
    The tool assumes one department per course based on the course number prefix. Cross-listed courses (e.g., FIN 6500 / MGMT 6500) would need special handling.

---

## Data Ownership & Process

11. **Who is responsible for collecting syllabi each semester, and what is the current process?**
    Is there a central repository (Canvas, OneDrive, Box, a shared drive)? Is collection consistent, or does it depend on individual faculty cooperation? The tool's value is directly proportional to how complete and timely the syllabus data is.

12. **How complete is our syllabus collection historically — what percentage of sections actually submit?**
    If submission rates are low for certain departments, the coverage gaps the tool surfaces may reflect missing data rather than actual curriculum gaps.

13. **Who has authority to verify or override the AI's skill tagging?**
    The tool lets any user edit all extracted data — skills, intensities, Bloom's levels, and custom fields. Should there be a designated reviewer per department, or a central curriculum coordinator who signs off on the final data? A "Verified" flag per row is on the roadmap if a formal review workflow is needed.

14. **What happens when a faculty member disputes how their course was tagged?**
    Is there an appeals process? Who has the final say on whether a course counts as developing a particular skill at a given intensity?

15. **Is there an existing curriculum mapping effort we'd be duplicating or should be integrating with?**
    Many schools have a spreadsheet, an Airtable, or a Curriculog workflow already. Before building out this tool further, it's worth knowing what's already in place and what problem it isn't solving.

---

## Audience & Reporting

16. **Who are the primary audiences for the output of this tool?**
    The dean's office, department chairs, AACSB reviewers, faculty curriculum committees, and the provost's office each need different views. Knowing the priority audience shapes what the reports and exports need to look like.

17. **What decisions will this tool actually be used to make?**
    Examples: revising required courses, flagging under-covered skills for new elective development, informing faculty about where their course fits in the broader curriculum, preparing AACSB documentation. Different use cases require different data precision.

18. **Will department chairs see their own department's data, or will all data be visible to everyone?**
    This affects whether we need any access controls or whether the open single-file approach is sufficient.

19. **Does the dean's office need a live dashboard, or is a periodic export (e.g., each semester) sufficient?**
    A live tool hosted on GitHub Pages is already in place. If stakeholders need to access it without managing a JSON database file, the tool's sharing model would need to change.

20. **Is there appetite for presenting this data to faculty at the school or department level?**
    Faculty often push back on curriculum mapping tools if they feel surveilled. Knowing the political context — whether this is framed as a support tool or a compliance requirement — affects how transparent the process should be and what we communicate to faculty.

---

## Skill Frameworks

21. **Are the 11 DESB Essential Skills definitions finalized, or are they still being refined?**
    If the definitions change, previously extracted data may need to be re-tagged. It's worth locking the definitions before building out a multi-year curriculum database.

22. **Are there any skills we're currently not tracking that the dean's team wants visibility into?**
    For example: sustainability and ESG competencies, AI literacy, entrepreneurial mindset, or global/intercultural competency. AACSB and peer schools are increasingly emphasizing these.

23. **Do the DESB Essential Skills map cleanly to our stated AoL learning goals, or is there a mismatch we need to resolve?**
    If the skill labels in the tool don't match the exact language in the AoL plan, we'll need a crosswalk table — or to rename the skills in the tool.

24. **Should Bloom's Taxonomy levels factor into coverage requirements, or is it informational only?**
    For example: "At least one required course per program must demonstrate Evaluate or Create." Bloom's levels are now extracted per course, editable by reviewers, and available as individual columns in the export. If Bloom's depth is a real requirement, it can be built into the gap analysis rather than shown as a chart only.

---

## Technology & Sustainability

25. **Who will own and maintain this tool long-term — curriculum staff, IT, or a faculty champion?**
    The current architecture (a single HTML file, no server, no login) is easy to hand off but has limits. Knowing who is responsible shapes decisions about complexity and support burden.

26. **Is the Anthropic API cost expected to come from a central budget, a department budget, or grant funding?**
    At roughly $0.50–$2.00 per syllabus extraction, a school processing 500 syllabi per semester is spending $250–$1,000 per run. This should be budgeted explicitly rather than absorbed informally.

27. **Are there data privacy or IT security requirements we need to satisfy before sending syllabus content to an external AI API?**
    Syllabi are generally not sensitive documents, but some may contain student accommodation information or proprietary course materials. IT or legal may have a position on this.

28. **Should this eventually integrate with existing university systems — Banner, Curriculog, Canvas, or the course catalog?**
    If so, which system is the authoritative source for course metadata (credits, prerequisites, program requirements)? Knowing the integration landscape early prevents building something that has to be rearchitected later.
