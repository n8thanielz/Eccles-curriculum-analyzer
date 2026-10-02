# Eccles Curriculum Analyzer

A browser-based tool for the David Eccles School of Business at the University of Utah. Upload course syllabi, extract structured curriculum data using the Anthropic API, and analyze skills coverage, Bloom's Taxonomy distribution, and curriculum gaps across departments and semesters.

**Live app:** https://n8thanielz.github.io/Eccles-curriculum-analyzer/

> For the original single-purpose Syllabus Analyzer, see [n8thanielz/syllabus-analyzer](https://github.com/n8thanielz/syllabus-analyzer).

---

## What it does

- Accepts PDF, Word (.docx), Excel (.xlsx), and plain text syllabi
- Extracts structured fields using Claude (learning outcomes, topics, engagement activities, assessments, reading materials, and more)
- Tags courses against two skills frameworks simultaneously:
  - **11 DESB Essential Skills** (synthesized from AACSB, NACE, USHE, and U of U definitions)
  - **8 NACE Career Readiness Competencies**
- Assigns a skill **intensity level** for each tag: Primary, Secondary, or Incidental
- Identifies **Bloom's Taxonomy** cognitive levels present in each course
- Auto-derives **department** and **course level** (1000–7000) from the course number
- Stores results in a filterable, searchable database with department and level filters
- Exports to CSV and Excel with intensity, Bloom's, and department columns

## Setup

1. Open the [live app](https://n8thanielz.github.io/Eccles-curriculum-analyzer/) in Chrome or Edge
2. Go to **Settings** and paste the Anthropic API key
3. Select a model (Sonnet is recommended; Haiku is faster and cheaper)
4. Save

The API key is stored only in your browser. Nothing is sent to any server other than the Anthropic API.

## Basic workflow

1. **Upload** — drag syllabus files onto the Upload tab, or click to browse. The app guesses course, semester, and year from the filename. Correct anything wrong before running.
2. **Extract** — click Run Extraction. Each syllabus takes roughly 20–60 seconds.
3. **Review** — go to Results. Yellow cells were flagged as uncertain by the AI. Click any row to open the editor and correct it.
4. **Save** — click Save Database File and store the JSON file in the shared folder next to the original syllabi. This file is the persistent database; load it back anytime on any computer.
5. **Analyze** — use the Dashboard and Coverage Analysis tabs to identify skill coverage patterns and curriculum gaps by department.

## Tabs

| Tab | Purpose |
|---|---|
| Upload & Extract | Queue and run syllabus extraction |
| Results | Browse, filter, search, and edit extracted rows |
| Skills Matrix | Course × skill grid with intensity indicators (● Primary, ● Secondary, ○ Incidental) |
| Dashboard | Stat cards, dept × skill coverage heatmap, Bloom's Taxonomy bar chart |
| Coverage Analysis | Gap analysis table with configurable threshold; highlights under-covered skills by department |
| Fields | Add, remove, or redefine custom extraction fields |
| Settings | API key and model selection |

## Skill intensity levels

| Level | Meaning |
|---|---|
| **Primary** | Core to the course; multiple graded assessments directly measure this skill |
| **Secondary** | At least one graded component develops this skill |
| **Incidental** | Skill appears but is not a graded focus |

## Bloom's Taxonomy levels

The AI identifies which of the six cognitive levels are present based on learning outcomes and assessments: Remember, Understand, Apply, Analyze, Evaluate, Create. The Dashboard shows the distribution across the full curriculum.

## Default extraction fields

| Field | What it captures |
|---|---|
| Learning Outcomes | Stated goals and objectives |
| Topics Covered | Subject-matter topics from the schedule |
| Assessment Methods | Graded components with weights |
| Engagement Activities | Hands-on student activities beyond lecture and reading |
| Reading Materials | Textbook (Yes/No) and Articles (Yes/No) |
| Additional Required Materials | Subscriptions, software, and other required purchases |
| Attendance Policy | Tracking method, absence limits, grade impact |

Core items extracted on every syllabus: course number, department, course level, course title, section, semester, year, instructor, meeting pattern, location, Essential Skills with intensity, NACE Competencies with intensity, Bloom's Taxonomy levels, and AI confidence flags.

## Technical notes

Single HTML file with no build step, no framework, and no backend. Two CDN dependencies: [mammoth.js](https://github.com/mwilliamson/mammoth.js) for Word files and [SheetJS](https://sheetjs.com) for Excel files. PDFs are sent directly to the Anthropic API using native document support. Data is auto-saved to browser localStorage; the JSON database file is the permanent record.
