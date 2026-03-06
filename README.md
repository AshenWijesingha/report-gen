# 🛡️ Security Report — Professional Vulnerability Report Platform

> Transform SecurityScorecard CSV exports into polished, executive-grade vulnerability assessment reports — instantly, in your browser, with no backend required.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Usage Guide](#usage-guide)
- [Input Format](#input-format)
- [Output Format](#output-format)
- [Configuration](#configuration)
- [Sample Data](#sample-data)
- [Security](#security)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Security Report** is a fully client-side, browser-based tool that parses [SecurityScorecard](https://securityscorecard.com/) Full Issues Report CSV exports and generates interactive, multi-page vulnerability reports. Reports can be viewed in-browser with full interactivity (filtering, pagination, dark/light theme) and exported as professional A4 PDFs with clickable CVE and URL links.

No server, no installation, no data leaves your browser.

---

## Features

### ✅ Must-Have (Core)

| Feature | Description |
|---|---|
| **CSV Upload** | Drag-and-drop or browse for SecurityScorecard Full Issues Report `.csv` files |
| **Multi-Entity Support** | Upload CSVs for multiple companies/entities; compare them side-by-side in tabbed views |
| **Interactive Report** | Fully rendered, multi-section HTML report viewable directly in the browser |
| **Severity Filtering** | Filter findings by severity: ALL / HIGH / MEDIUM / LOW / INFO |
| **PDF Export** | One-click export to A4 PDF via jsPDF with clickable CVE and URL links |
| **Credential Breach Integration** | Upload a breach intelligence CSV to append a Credential Breach Intelligence page |
| **Logo Upload** | Add your company logo (PNG, JPG, SVG, WebP, max 2 MB); appears on report cover and PDF |
| **Report Options** | Set analyst name, report title, and assessment period before generating |
| **Sample Data** | Load demo data from `/data/` folder to preview the tool without real files |
| **Dark / Light Theme** | Toggle between dark (default) and light themes; preference saved in `localStorage` |

### 🌟 Nice-to-Have (Quality of Life)

| Feature | Description |
|---|---|
| **Finding Detail Modal** | Click any finding row to open a full-detail modal with all field values |
| **CVE Links** | Every CVE identifier links directly to the [NVD database](https://nvd.nist.gov/) |
| **Clickable URLs** | Initial/Final URLs in findings are clickable in both the report and exported PDF |
| **Pagination** | Detailed findings paginate at 10 items per page to keep reports readable |
| **Entity Name Editing** | Auto-guessed entity names from filenames can be edited before generating |
| **Combined View** | A "Combined" tab aggregates findings across all uploaded entities |
| **KPI Cards** | Cover page shows key metrics: total findings, high/medium/low/info counts |
| **Remediation Roadmap** | Automatically generated P1/P2/P3 remediation timeline with action items |
| **Severity Donut Chart** | SVG donut chart showing severity distribution on the executive summary page |
| **Factor Breakdown** | Bar chart of findings grouped by SecurityScorecard factor (e.g., Application Security, Network Security) |
| **Domain Analysis** | Table of affected domains with per-domain finding counts and severity breakdown |

---

## Technology Stack

| Layer | Technology |
|---|---|
| **Language** | Vanilla JavaScript (ES2020+), HTML5, CSS3 |
| **CSV Parsing** | [PapaParse 5.4.1](https://www.papaparse.com/) |
| **PDF Generation** | [jsPDF 2.5.1](https://github.com/parallax/jsPDF) + [jspdf-autotable 3.8.1](https://github.com/simonbengtsson/jsPDF-AutoTable) |
| **Fonts** | [Inter](https://fonts.google.com/specimen/Inter) (UI), [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) (data/code) via Google Fonts |
| **Hosting** | Any static file server or local `file://` — no backend required |

---

## Getting Started

### Prerequisites

- A modern web browser (Chrome, Edge, Firefox, Safari)
- SecurityScorecard Full Issues Report CSV export(s)
- *(Optional)* A static HTTP server for the sample data feature (see below)

### Running Locally

#### Option 1 — Open Directly (no sample data)

Simply open `index.html` in your browser:

```bash
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

> **Note:** When opened via `file://`, the "Load Sample Data" feature will not work due to browser fetch restrictions. Upload your own CSV files instead.

#### Option 2 — Local HTTP Server (enables sample data)

```bash
# Python 3
python -m http.server 8080

# Node.js (npx)
npx serve .

# Node.js (http-server)
npx http-server -p 8080
```

Then navigate to [http://localhost:8080](http://localhost:8080).

---

## Usage Guide

### Step 1 — Upload CSV Files

- Drag and drop one or more SecurityScorecard **Full Issues Report** `.csv` files onto the drop zone, or click **Browse Files**.
- Each file represents one entity (company/domain group).
- Entity names are automatically guessed from the filename and can be edited in the input fields that appear below the file list.

### Step 2 — (Optional) Upload Logo

- Click **Choose Image** under "Company Logo".
- Accepted formats: PNG, JPG, SVG, WebP (max 2 MB).
- The logo appears on the PDF cover page and the HTML report header.

### Step 3 — (Optional) Upload Credential Breach Data

- Click **Choose File** under "Credential Breach Intelligence".
- Upload a SecurityScorecard `historical_compromised_credentials_found` CSV.
- A dedicated Credential Breach Intelligence page will be appended to the report.

### Step 4 — Set Report Options

Fill in the optional fields before generating:

| Field | Description |
|---|---|
| **Analyst Name** | Your name / team name — appears in the report footer |
| **Report Title** | Custom title for the report cover page |
| **Assessment Period** | Date range of the assessment (e.g., `Q1 2026`) |

### Step 5 — Generate Report

Click **Generate Professional Report**. The report renders instantly in the browser.

### Step 6 — Explore the Report

- Use the **entity tabs** at the top to switch between individual entities or the "Combined" view.
- Use the **severity filter buttons** (ALL / HIGH / MEDIUM / LOW / INFO) to filter the findings tables.
- Click any finding row to open the **Finding Detail Modal** with all available field data.
- Use the **← / →** page navigation buttons to browse paginated sections.

### Step 7 — Export to PDF

- Click **Export PDF** in the toolbar, or press `Ctrl+P` / `Cmd+P`.
- The PDF is generated client-side and downloaded automatically.
- CVE identifiers and URLs are clickable in the exported PDF.

> **PDF Limits:** To keep file size manageable, the PDF includes up to **25 findings** in the summary tables and **10 findings per page** in the detailed section.

---

## Input Format

### SecurityScorecard Full Issues Report CSV

The primary input file must be a SecurityScorecard "Full Issues Report" export. Expected columns include:

| Column | Description |
|---|---|
| `ISSUE ID` | Unique identifier for the finding |
| `FACTOR NAME` | SecurityScorecard factor (e.g., `Application Security`) |
| `ISSUE TYPE TITLE` | Human-readable issue name |
| `ISSUE TYPE CODE` | Machine-readable issue code |
| `ISSUE TYPE SEVERITY` | `HIGH`, `MEDIUM`, `LOW`, or `INFO` |
| `ISSUE RECOMMENDATION` | Remediation guidance text |
| `FIRST SEEN` / `LAST SEEN` | ISO 8601 timestamps |
| `IP ADDRESSES` | Associated IP address(es) |
| `HOSTNAME` | Affected hostname |
| `SUBDOMAIN` | Affected subdomain |
| `PORTS` | Associated port(s) |
| `STATUS` | Finding status |
| `CVE` | CVE identifier(s), if applicable |
| `DESCRIPTION` | Detailed finding description |
| `INITIAL URL` / `FINAL URL` | URLs associated with the finding |
| `ANALYSIS` | Additional analysis text |
| `PRODUCT` / `VERSION` / `PROVIDER` | Affected software details |
| `ISSUE TYPE SCORE IMPACT` | Score impact value |

### Credential Breach Intelligence CSV

Optional supplementary file. Expected columns include:

| Column | Description |
|---|---|
| `Domain` | Affected domain |
| `User Name` | Compromised username |
| `Leaked Password` | Masked/hashed password indicator |
| `Infection Date` | Date of compromise |
| `Stealer Name` | Malware family name |
| `Country` | Country of infection |
| `Operating System` | OS of infected machine |
| `First Observed` | First observation date |

---

## Output Format

The generated report consists of up to **9 pages/sections**:

| Page | Section | Description |
|---|---|---|
| 1 | **Cover Page** | Title, entity name, logo, KPI summary cards, assessment period |
| 2 | **Table of Contents** | Linked index of all report sections |
| 3 | **Executive Summary** | Severity distribution donut chart, risk narrative, key metrics |
| 4 | **Factor Analysis** | Findings grouped by SecurityScorecard factor with bar chart |
| 5 | **Domain Analysis** | Per-domain finding counts and severity breakdown table |
| 6 | **Findings Summary** | HIGH / MEDIUM / LOW / INFO findings tables with severity tags |
| 7 | **CVE Registry** | All associated CVEs with CVSS scores and NVD links |
| 8 | **Detailed Findings** | Paginated cards with full field data and remediation guidance |
| 9 | **Credential Breach** *(optional)* | Breach intelligence table (shown only when breach CSV is uploaded) |
| — | **Remediation Roadmap** | P1 (24–72h) / P2 (30 days) / P3 (90 days) action timeline |

**PDF output:** A4 portrait, embedded fonts, clickable hyperlinks.

---

## Configuration

The following constants control PDF output behavior and can be modified in `index.html`:

```javascript
const CONFIG = {
  MAX_FINDINGS_IN_PDF: 25,   // Maximum findings rows in summary PDF tables
  FINDINGS_PER_PAGE: 10,     // Findings per page in the detailed section
};
```

### Theme

The default theme is **dark**. Toggle using the 🌙 button in the toolbar or upload screen. The preference is persisted in `localStorage` under the key `theme`.

---

## Sample Data

The `/data/` directory contains representative sample files for demonstration purposes:

| File | Description |
|---|---|
| `ABC-FullIssuesReport-2026-03-05.csv` | Full Issues Report for "ABC Company" |
| `DEF-FullIssuesReport-2026-03-05.csv` | Full Issues Report for "DEF Company" |
| `SecurityScorecard - ABC - historical_compromised_credentials_found - findings - March 6, 2026.csv` | Credential breach intelligence sample |

Click **Load Sample Data from /data folder** on the upload screen (requires a local HTTP server) to auto-load all three files and generate a demo report.

---

## Security

- **No data transmission:** All CSV parsing, analysis, and PDF generation happens entirely in the browser. No data is sent to any server.
- **XSS prevention:** All user-supplied and CSV-derived values are sanitized via `escapeHtml()` before being inserted into the DOM.
- **Safe event handling:** Finding detail modals use an ID-based registry lookup instead of inline `onclick` JSON to prevent injection attacks.
- **No credentials stored:** Logo and CSV data are held in memory only for the duration of the session and cleared when returning to the upload screen.

---

## Future Enhancements

The following improvements are planned or under consideration:

### 📊 Reporting & Analytics
- [ ] **Trend analysis** — Compare two reports from different time periods and highlight new / resolved / regressed findings
- [ ] **Score simulation** — Estimate projected SecurityScorecard score improvement after remediating selected findings
- [ ] **Custom risk scoring** — Allow analysts to override severity ratings with internal risk scores
- [ ] **CVSS v3.1 enrichment** — Automatically fetch CVSS base scores and vectors from the NVD API for all CVEs
- [ ] **Executive summary narrative generator** — AI-assisted plain-English summary of the security posture

### 🖥️ User Interface
- [ ] **Report templates** — Multiple cover page / colour scheme options (e.g., branded, minimal, regulatory)
- [ ] **Drag-to-reorder pages** — Let analysts choose which sections to include and their order in the PDF
- [ ] **Inline annotation** — Allow analysts to add notes and comments directly to finding cards before export
- [ ] **Print-optimised CSS** — Media queries for direct browser printing without jsPDF
- [ ] **Responsive mobile layout** — Improved layout for tablet viewing of generated reports

### 📁 Data & Integration
- [ ] **Additional data sources** — Support for other vulnerability scanner exports (Qualys, Tenable, Rapid7)
- [ ] **JIRA / ServiceNow export** — Generate ticket-ready JSON/CSV for findings to feed into ticketing systems
- [ ] **Persistent projects** — Save report configurations (entity names, logo, options) to `localStorage` or `IndexedDB`
- [ ] **Shareable report URLs** — Encode report state into a URL hash or file for easy sharing
- [ ] **Bulk CSV merge** — Automatically detect and deduplicate overlapping findings across entity files

### ⚙️ Developer Experience
- [ ] **Build pipeline** — Introduce a bundler (Vite/esbuild) to split the monolithic `index.html` into modular source files
- [ ] **Automated tests** — Unit tests for CSV parsing, data analysis, and PDF generation logic
- [ ] **CI/CD pipeline** — GitHub Actions workflow for linting, testing, and deploying to GitHub Pages
- [ ] **TypeScript migration** — Add static typing to improve maintainability and developer tooling support
- [ ] **Accessibility audit** — WCAG 2.1 AA compliance review and remediation (keyboard navigation, ARIA labels, colour contrast)

---

## Contributing

Contributions, issues, and feature requests are welcome.

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'feat: add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

Please keep changes focused and test with both the sample data and your own SecurityScorecard exports before submitting.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
  <sub>Built with ❤️ for security professionals who deserve better reports.</sub>
</div>
