# Curriculum Analyzer — Associate Dean Review
*October 2026 — updated after implementation sprints*

---

## What's genuinely strong

**The intensity levels are the tool's most valuable design decision.** Capturing Primary / Secondary / Incidental rather than a simple yes/no means the Skills Matrix can actually distinguish between a course where Critical Thinking is a core graded competency versus one where it shows up incidentally. That distinction is what makes coverage data actionable rather than decorative.

**The extraction prompt is careful.** The skill definitions are grounded in AACSB, NACE, USHE, and U of U sources simultaneously, and the instruction to tag only with concrete evidence prevents garbage inferences from course titles. The Technology definition — explicitly excluding Canvas and CIS as non-qualifying platforms — is a good example of the kind of precision that matters.

**The evidence field per skill is an accreditation asset.** When an AACSB review team asks "how do you know this course develops Quantitative Literacy?", the AI's one-line evidence note is a starting point for the response. Most tools don't capture this at all.

**The custom fields system is genuinely useful.** Being able to define what the AI looks for — not just what boxes to fill in — means the tool can evolve as curriculum needs evolve without requiring code changes.

---

## Structural gaps — the things that matter most

### 1. ~~No credit hours field~~ — ✅ Implemented
Credit hours are now extracted as a standard field on every syllabus. The Coverage Analysis and Skills Matrix still count courses rather than credit hours, but the raw data is in the export and ready for credit-hour-weighted calculations in Excel. For AACSB compliance calculations (e.g., "at least 50% of credit hours in the business core must address this learning goal"), this data is now available.

### 2. No program affiliation
Departments are not programs. ACCTG courses serve the BSBA-Accounting, the BSBA-General Business, the MAcc, and sometimes MBA students. The tool has no concept of which program a course belongs to, or whether it's required vs. elective in a given program. An Associate Dean almost always thinks in terms of "what does an MBA student experience?" not "what does the MGMT department offer?" This is probably the biggest structural gap for an accreditation use case.

### 3. No course-level Bloom's filter in Coverage Analysis
The Bloom's distribution chart is aggregate only, and there's no way to filter Coverage Analysis by course level. One of the most important curriculum questions for an Associate Dean is: *"Are our upper-division and graduate courses actually operating at higher cognitive levels?"* You'd want to see that 5000-level courses cluster at Evaluate and Create while 2000-level courses cluster at Remember and Apply. Right now that question is unanswerable with the tool.

### 4. No semester-over-semester comparison
The semester filter narrows the view to one period at a time, but there's no side-by-side or trend view. An Associate Dean presenting to the faculty senate or an AACSB review wants to say: "Our coverage of Written Communication improved from 60% to 78% between 2024 and 2025 after we revised the core requirements." That narrative requires comparative data, and the tool currently can't surface it.

### 5. ~~Bloom's levels are read-only in the detail editor~~ — ✅ Implemented
Bloom's levels are now fully editable in the detail editor via checkboxes. Reviewers can add or remove levels when the AI missed a level due to unusual phrasing in the learning outcomes.

### 6. No audit trail on re-extraction
When a row is re-extracted, the previous data is silently replaced. There's no record of what changed, who re-extracted it, or when. For a curriculum repository used in an accreditation report, the ability to say "this data was reviewed and confirmed on this date" matters. Storing a `reviewedAt` timestamp and a simple "Verified" flag per row would help significantly.

---

## Coverage Analysis specific

### 7. ~~No department filter on Coverage Analysis~~ — ✅ Implemented
Coverage Analysis now has a department filter alongside the semester filter, matching the Dashboard. Department chairs can now see their department's gap data in isolation.

### 8. Threshold is global, not per-skill
The gap threshold is a single number applied uniformly. In practice, different skills have different appropriate benchmarks: "Written Communication should appear in 80% of courses; Career Readiness only needs 30%." A per-skill minimum coverage target would make the gap analysis far more actionable.

---

## CSV / Excel export

### 9. ~~Skills are semicolon-concatenated — difficult to analyze~~ — ✅ Implemented
The export now has one column per Essential Skill (`ESS: Critical Thinking`, `ESS: Written Communication`, etc.) with the intensity value (Primary / Secondary / Incidental) or blank if the course doesn't address that skill. The old concatenated `Essential Skills` and `Essential Skills Intensity` columns have been removed. Every cell is now filterable and pivot-able directly in Excel.

### 10. ~~Bloom's levels are also concatenated~~ — ✅ Implemented
Each Bloom's level now has its own boolean column (`Bloom: Remember`, `Bloom: Understand`, etc.) with Yes or blank. The old concatenated `Bloom's Levels` column has been removed.

### 11. The Excel export is flat — it misses the opportunity for a multi-sheet workbook
The `.xlsx` export currently produces the same flat structure as the CSV. A more useful workbook would include:
- **Sheet 1: Course Roster** — one row per section, current format
- **Sheet 2: Skills Matrix** — courses as rows, skills as columns, intensity as cell values — ready to pivot
- **Sheet 3: Coverage Summary** — the dept × skill heatmap data as numbers, not colors
- **Sheet 4: Bloom's** — course-level Bloom's data, one column per level

### 12. ~~Intensity and evidence are separated into parallel columns~~ — ✅ Partially addressed
Evidence columns (`Essential Skills Evidence`, `NACE Evidence`) are now at the end of the export, separated from the per-skill intensity columns. The evidence strings remain pipe-delimited per-skill notes; fully splitting them into per-skill evidence columns would require one column per skill × framework.

### 13. CSV filename doesn't reflect active filters
If I export just ACCTG courses from Fall 2024, the file is named `syllabus-export-[date].csv` — the same as a full export. The filename should encode active filters so exports are self-documenting and don't silently overwrite each other.

### 14. ~~Reading Materials buried in a multi-line text cell~~ — ✅ Implemented
Reading Materials now exports as two separate boolean columns: `Textbook Required` and `Articles Required` (Yes / not stated). These are pivot-ready.

---

## Smaller but real usability issues

### 15. No unsaved-changes warning in the detail editor
Easy to lose edits by accidentally clicking Close. A simple dirty-state check and confirm prompt would prevent data loss.

### 16. The Results skill filter only covers Essential Skills, not NACE
If I want to find all courses that address NACE "Teamwork," I have to use the text search and hope the word appears somewhere visible. A NACE filter dropdown alongside the Essential Skills one would fix this.

### 17. The generated report is framework-specific
The report respects whichever of Essential Skills or NACE is selected in the Dashboard toggle, but there's no way to produce a combined report covering both frameworks in one document.

### 18. No "Verified" flag per row
There's currently no distinction between AI-generated data that no one has reviewed and data that a coordinator has verified against the actual syllabus. A simple Verified checkbox per row, visible in the Results table, would make the repository more trustworthy for accreditation purposes.

---

## Priority ranking

| Status | Item |
|--------|------|
| ✅ Done | Per-skill intensity columns in CSV/Excel export |
| ✅ Done | Bloom's level columns in CSV/Excel export (one column per level) |
| ✅ Done | Editable Bloom's levels in the detail editor |
| ✅ Done | Department filter on Coverage Analysis |
| ✅ Done | Department filter on Dashboard |
| ✅ Done | Credit hours as a standard extracted field |
| ✅ Done | Reading Materials split into Textbook Required / Articles Required columns |
| **High** | Program / required-vs-elective tracking |
| **High** | Bloom's distribution by department / by course level |
| **Medium** | Multi-sheet Excel workbook (roster + skills matrix + coverage + Bloom's) |
| **Medium** | Verified/reviewed flag per row |
| **Medium** | Filter-aware CSV filename |
| **Medium** | Semester-over-semester comparison view |
| **Lower** | Per-skill coverage thresholds |
| **Lower** | Unsaved-changes warning in detail editor |
| **Lower** | NACE filter on Results tab |
| **Lower** | Combined Essential Skills + NACE report |
