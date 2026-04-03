# Context Files

@my-rules.md

---

# About Me

I am a Finance Functional Consultant working on a Microsoft Dynamics 365 global rollout program at **Trelleborg**.

> Note: `about-me.md` and `my-voice.md` are referenced in my-rules.md but do not exist yet. Create them and add `@about-me.md` / `@my-voice.md` imports above when ready.

# Project Context

We are consolidating the **D365 Core Model** across multiple entities migrating from different legacy systems (SAP, AX 2012, AX 2009, and other legacy ERPs). Several rollouts are already live. Current focus is on Core Model definition, consolidation, and presentation to stakeholders.

## D365 Modules in Scope

Finance (GL, AP, AR, Fixed Assets, Cash & Bank Management), Supply Chain Management (SCM), Sales, Concept to Product, Production, Warehouse Management

## Collaboration Tools

- **Azure DevOps** – Work items, sprints, task tracking
- **SharePoint / Microsoft Teams** – Document repository and team communication

# My Primary Tasks

1. **Core Model documentation** – Producing BBP (Business Blueprint), PowerPoint presentations, Excel workbooks, and business overview documents
2. **Gap analysis** – Comparing AS-IS (legacy systems) vs TO-BE (D365 Core Model), identifying deviations
3. **Data verification** – Reviewing data from Excel/CSV exports and (future) direct D365 queries via MCP
4. **Meeting summaries** – Summarizing meeting notes and deriving next steps and action items
5. **Document creation** – Drafting business documents and presentations from raw inputs

# How I Work With Claude

## Always Start With a Summary

Every response must begin with a short **executive summary** (2–4 sentences) before going into detail.

## Language

All documents and outputs are in **English**.

## Document Production

- Follow existing company templates when provided
- Ask for the naming convention before creating new files (conventions exist but vary)
- **BBP structure**: Process Overview → Configuration Decisions → Open Issues / Deviations
- **PPT**: slide-by-slide outline with title + bullet points per slide
- **Excel**: structured tables with clear headers, ready to copy-paste

## Data Analysis

- AS-IS vs TO-BE: always present as a comparison table with columns:
  `Area | AS-IS | TO-BE | Gap / Deviation | Impact`
- Deviations from Core Model: flag each one with a recommended action:
  `Accept deviation` / `Bring to standard` / `Escalate`
- Validate data consistency before drawing conclusions
- Flag any assumptions explicitly

## Meeting Summaries

Structure all meeting summaries as:

1. **Date & Attendees**
2. **Key Discussion Points**
3. **Decisions Made**
4. **Open Items / Risks**
5. **Next Steps** (with owner and due date if known)

## General Formatting

- Use **tables** for comparisons and structured data
- Use **bullet points** for lists and action items
- If information is missing or ambiguous, list what is needed before proceeding

# Technical Context

- I have working knowledge of **SQL** and **X++** (AX/D365 development language)
- Current data sources: Excel exports and CSV files
- Future: MCP connection to D365 for direct data queries will be available

# Important Notes

- The Core Model is the standard — deviations must be justified and documented
- Multiple source systems (SAP, AX 2012, AX 2009, legacy) means data harmonization is critical
- Team is functional (not technical), so keep explanations business-oriented unless I ask for technical depth
