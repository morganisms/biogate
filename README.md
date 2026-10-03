# BioGate

A single-file web app for checking stage-gate readiness on diagnostic product development projects run under ISO 13485. For each gate it lists the required deliverables and shows which ones have evidence in place, which are still missing, and whether the gate is ready for review.

A companion to [BioPortfolio](https://github.com/morganisms/bioportfolio) and [BioCapacity](https://github.com/morganisms/biocapacity): BioPortfolio decides which projects matter most, BioCapacity shows whether the team can staff them, and BioGate shows whether each project is ready to pass its next gate.

**Run it:** download `index.html` and open it in any modern browser. There's no install, no server and no external dependencies.

> [!CAUTION]
> **JSON backups are unencrypted and contain all of your project data in plain text.**
> Every project name, evidence reference (document numbers, file paths, links), justification and owner name is readable by anyone who opens the file. Treat a backup as a controlled quality record:
>
> - **Store it only where your company allows design history and quality records**, such as your QMS, document control system or an access-controlled company drive.
> - **Never commit a backup to this repository or any public repository**, and don't attach it to public issues or pull requests. The `.gitignore` in this repo blocks `biogate-backup-*.json` and `biogate-gaps-*.csv` as a safeguard, but don't rely on it alone.
> - **Don't send backups through personal email or consumer file-sharing services.**
> - **Only import files you exported yourself or received from someone you trust.** Imports are validated and malformed records are dropped, but an imported file replaces every project in your browser.
> - **Shared or kiosk computers:** anyone who uses the same browser profile can open the app and see your data. Use **Reset to samples** before you leave, or use a private window.

## Features

- **Gate readiness**: a stage-gate track shows every gate as a diamond. Green means ready, amber means every non-negotiable is in place but recommended items are still open, and red means at least one non-negotiable is missing. Select a gate to see its deliverables, blocking items and readiness score.
- **Evidence log**: for each deliverable, record a status (Missing, In progress, Evidence in place or N/A), an evidence reference such as a document number or link, and an owner. Readiness updates as you type.
- **Research Use Only or IVD**: switch a project between RUO and IVD at any time. Requirement levels change to match, and evidence you've entered is kept.
- **Target markets**: IVD projects can target the US (FDA), the EU (IVDR) or both. Each market adds its own submission, registration and post-market deliverables.
- **Software scope**: turn on "Includes software" to add the IEC 62304 deliverables.
- **Deliverable matrix**: every deliverable side by side for RUO, IVD in the US and IVD in the EU, so you can see exactly what is non-negotiable for each.
- **Filtering**: show all deliverables, gaps only, or non-negotiables only.
- **Multiple projects**: create, rename, duplicate and delete projects, each with its own product type, scope, current gate and evidence log.
- **Gap report**: export the open deliverables for a project to CSV, with gate, requirement level, status, evidence, owner and reference.
- **Print**: print the current gate's checklist for a gate review meeting.
- **Backup**: export all projects to JSON and restore them later. Read the caution above first.
- **Light and dark mode**: follows your system setting until you choose one with the toggle in the header.

## Gates

| Gate | Name | Exit question |
|------|------|---------------|
| G0 | Concept Approval | Is the opportunity worth pursuing, and is the product's regulatory identity defined? |
| G1 | Feasibility Exit / Design Inputs | Does the technology work, and are design inputs approved so design control can begin? |
| G2 | Design Freeze | Are design outputs complete and V&V protocols approved? |
| G3 | V&V Complete / Design Transfer | Is the design proven, transferable to manufacturing, and submitted? |
| G4 | Commercial Release | Is the product authorized, manufactured under control, and supported post-launch? |

The model has 46 deliverables. References cite ISO 13485:2016 clauses, ISO 14971:2019, IEC 62304, IEC 62366-1, the FDA QMSR (21 CFR 820, which incorporates ISO 13485 by reference), 21 CFR 809 and EU IVDR 2017/746.

## Requirement levels

| Level | Meaning |
|-------|---------|
| Non-negotiable | The gate can't pass until evidence is in place, or the item is marked N/A with a written justification |
| Recommended | Good practice. An open recommended item makes the gate amber, not red |
| Not applicable | Hidden for this project's configuration and listed in the matrix |

How many deliverables are non-negotiable depends on the configuration:

| Configuration | Non-negotiable | Recommended |
|---------------|----------------|-------------|
| Research Use Only | 15 | 19 (22 with software) |
| IVD, US | 32 (35 with software) | 7 |
| IVD, EU | 34 (37 with software) | 6 |
| IVD, US and EU, with software | 39 | 7 |

**Why RUO is lighter.** RUO products aren't medical devices, so full design control is shown as recommended. The RUO non-negotiables protect the RUO status itself: correct labeling ("For Research Use Only. Not for use in diagnostic procedures."), a documented classification rationale, no clinical or diagnostic claims, data behind every datasheet claim, stability data for expiry dating, controlled manufacturing and QC release, and complaint handling. Many teams run RUO products under design control anyway to keep a path to IVD open.

## How readiness is calculated

Readiness = non-negotiables satisfied ÷ non-negotiables that apply at that gate.

A deliverable counts as satisfied when its status is **Evidence in place**, or when it is **N/A** and has a written justification. N/A without a justification doesn't count and is flagged on the row.

| Color | Status |
|-------|--------|
| Green | Every non-negotiable and recommended item is satisfied |
| Amber | Every non-negotiable is satisfied, but recommended items are open |
| Red | At least one non-negotiable is missing |

## Data

Changes save automatically to your browser's local storage, so they stay on your machine and survive a reload. Clear browser data and they're gone, so export a JSON backup regularly and store it as described in the caution above.

Exports are named with the date, for example `biogate-backup-2026-10-03.json` and `biogate-gaps-respiratory-panel-rt-pcr-2026-10-03.csv`. The CSV opens in Excel with accented names intact.

Two sample projects load the first time you open the app: an IVD respiratory panel and an RUO library prep kit. Use **Reset to samples** to restore them, or delete them once you've added your own.

If the app is open in more than one tab, each tab picks up changes made in the others.

All saved and imported data is validated on load. Only known deliverables and statuses are kept, text is length-limited, and malformed records are dropped. If the saved data can't be read at all, the app loads the sample projects and keeps the unreadable copy in browser storage under `biogate_v1_unreadable`, so nothing is overwritten.

Evidence references are shown as plain text, not clickable links. Text in the CSV gap report that starts with `=`, `+`, `-` or `@` is prefixed with `'` so spreadsheets don't run it as a formula.

## Limits

- 200 projects
- Project and owner names up to 120 characters
- Evidence and justifications up to 500 characters
- JSON imports up to 5 MB

## Disclaimer

BioGate is a planning tool, not a regulatory determination. Deliverables, requirement levels and references are a general model for diagnostic product development. Confirm the requirements for each product with your Regulatory Affairs and Quality teams and align them with your own design control SOPs.
