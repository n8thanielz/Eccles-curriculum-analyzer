# Curriculum Analyzer — Roadmap & Recommendations

This document tracks architectural recommendations, open questions, and future upgrade phases for the DESB Curriculum Analyzer project.

---

## Open Questions (answers will shape priorities)

1. **What decision does this data feed into?** Curriculum redesign, AACSB assurance of learning documentation, accreditation self-study, resource allocation? This determines which views and exports matter most.
2. **Is there a target coverage level?** E.g., "every student should encounter Critical Thinking in at least 60% of required courses" — or is the goal purely descriptive for now?
3. **Will faculty ever see this?** Faculty-facing results require different framing around gaps than dean/admin-only views.
4. **What does the dean presentation look like?** Live demo, static export, printed report? Determines how much polish the Dashboard needs.
5. **Scale and program structure?** How many unique courses and syllabi does the school have? Are there program-level distinctions (BBA vs. MBA vs. specialized masters) that should be reflected in groupings?
6. **Required vs. elective courses?** Gap analysis means something very different if results can be weighted by enrollment or required status.
7. **What is this replacing?** Manual spreadsheet, existing AoL tracking system, or starting from scratch?

---

## Current State

### Version 1 — Syllabus Analyzer (`n8nathanielz/syllabus-analyzer`)
Basic extraction tool. Single-user, browser-only.
- PDF, DOCX, XLSX, TXT extraction via Anthropic API
- 7 configurable extraction fields
- Dual skills framework: 11 DESB Essential Skills + 8 NACE competencies
- Results table with filtering, search, and column toggles
- CSV and Excel export
- JSON database file for sharing

### Version 2 — Eccles Curriculum Analyzer (`n8thanielz/Eccles-curriculum-analyzer`)
Enhanced analytical version built on top of V1.
- Skill intensity levels: Primary / Secondary / Incidental
- Bloom's Taxonomy cognitive level tagging
- Department and course level auto-derivation (1000–7000)
- Department and level filters on Results and Skills Matrix
- Skills Matrix grouped by department with subtotals
- Dashboard tab: stat cards, skill coverage heatmap, Bloom's distribution chart
- Coverage Analysis tab: gap table with configurable threshold and per-dept coverage score
- Abbreviated column headers with full-name tooltips
- Intensity-colored skill chips in Results
- "—" display for missing values
- Department and Extraction Date as toggleable columns

---

## Near-Term Improvements (no infrastructure change needed)

### Printable PDF Report
Generate a print-optimized HTML report in a new browser tab. User saves as PDF via Ctrl+P.

Proposed structure:
1. Cover header with DESB branding, date, and summary line (e.g. "47 courses across 8 departments")
2. Executive summary stat cards
3. Skill Coverage by Department (heatmap table)
4. Coverage Gaps callout (skills below threshold, written in plain language)
5. Bloom's Taxonomy distribution
6. Full Skills Matrix as appendix

Options to decide: respect active filters vs. always report all data; use current gap threshold or fixed value; include table of contents.

### Dashboard Enhancements
- Top-3 lowest-covered skills callout above the heatmap
- Semester-over-semester comparison (requires multiple semesters of data)

### Skills Matrix Enhancements
- Sortable columns (click a skill header to rank courses by whether they cover it)
- Filter by course level (1000-level only, graduate only, etc.)

### Results Table
- Bulk edit (change semester or year across multiple selected rows)

---

## Phase 2 — Light Backend (low complexity, high security value)

**Trigger:** When more than 2–3 people need access, or when the API key needs to be protected.

### Serverless API Proxy
A ~20-line function on Cloudflare Workers or Vercel that holds the Anthropic API key server-side and proxies requests. The browser app calls the proxy instead of Anthropic directly.

- API key never exposed in browser localStorage
- No database, no auth — just a secure middleman
- Can be deployed in under an hour
- Cost: effectively free at this usage volume

### Shared Database
Replace the JSON file workflow with a hosted database (Supabase or Firebase are good fits for a browser-only frontend).

- Everyone opens the app and sees the same data live
- No file passing via OneDrive/email
- Multiple people can extract simultaneously
- Still no traditional server to maintain

---

## Phase 3 — Full Backend (higher complexity, enterprise-level capability)

**Trigger:** Faculty-level access, IT involvement, or integration with university systems.

### Authentication & Roles
- Admin: full access, can delete rows, manage fields, run extractions
- Contributor: can extract and edit their own department's rows
- Viewer: read-only access (dean's team, curriculum committee)

### University System Integration
- Pull course roster from Banner/PeopleSoft to pre-populate the extraction queue with correct course numbers, titles, and instructor names — eliminating manual metadata entry
- Push coverage data back to AoL tracking systems

### Historical Tracking
- Store a snapshot of each semester's skill coverage
- Dashboard shows semester-over-semester trend lines
- Flag courses whose skill profile changed significantly between extractions

### Background Processing
- Queue extractions server-side so users don't need to keep the browser tab open
- Email notification when a batch completes

### Higher-Quality Report Export
- Server-side PDF generation (Puppeteer or WeasyPrint) for pixel-perfect branded reports
- Scheduled report delivery (e.g., auto-email the dean after each semester's extraction run)

---

## Architecture Decision Summary

| Scenario | Recommended Architecture |
|---|---|
| 1–3 users, small team, JSON sharing is fine | Current single-file browser app |
| API key security concern | Add serverless proxy (Phase 2a) |
| Multiple simultaneous users needed | Add shared database (Phase 2b) |
| Faculty self-service across departments | Full backend with auth (Phase 3) |
| Banner/AoL system integration required | Full backend with university IT (Phase 3) |
