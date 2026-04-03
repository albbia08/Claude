# Copilot Agent Factory — Overview

**Version:** 1.0
**Status:** Draft
**Last Updated:** 2026-04-03
**Applies to:** Any Copilot Studio agent project

---

## 1. What Is the Copilot Agent Factory?

The Copilot Agent Factory is the master toolkit for designing, configuring, and deploying a Copilot Studio agent from zero to production. It is a reusable framework — the L1 D365 Support Agent is the first instance.

The factory produces two categories of output:

| Category | What It Is | Where It Goes |
|----------|------------|---------------|
| **Agent Instructions** | The behavioral rules pasted into Copilot Studio's Instructions field | Copilot Studio > Settings > Instructions |
| **Design Artifacts** | Topics, skills, flows, KB index, test plan | Azure DevOps / SharePoint |

---

## 2. Factory Files Index

| # | File | Purpose | Character Limit |
|---|------|---------|-----------------|
| This file | `00-factory-overview.md` | Index, usage guide | — |
| 1A | `01-instructions-part-a.md` | Copilot Studio Instructions — Identity, Purpose, Scope, Audience, Tools, How to use tools | ~3,500 chars |
| 1B | `01-instructions-part-b.md` | Copilot Studio Instructions — Rules, Error Handling, Limitations, Format, Examples | ~4,200 chars |
| 2 | `02-topics.md` | All topics with trigger phrases, slot definitions, and flow logic | — |
| 3 | `03-operating-model.md` | Context-first approach, 5-step action plan, skills usage guide, rules, do not use | — |

**Note on the 8,000-character limit:**
Copilot Studio's Instructions field has a hard character limit. Parts A and B combined equal the full instruction set. If the platform accepts ~7,700 chars, paste both parts together. If you must split, paste Part A in the main Instructions field and embed Part B as a preamble inside the first triggered topic.

---

## 3. How to Use the Factory

Follow this sequence for every new agent project:

```
STEP 1 — Read project-context.md (in the project folder)
         Understand the agent goal, audience, slot filling, escalation logic,
         and open questions before touching any factory file.

STEP 2 — Fill in 01-instructions-part-a.md
         Replace all [PLACEHOLDER] values with project-specific data.
         Validate character count stays under 3,500.

STEP 3 — Fill in 01-instructions-part-b.md
         Replace [PLACEHOLDER] values (SLA, L2 contact, module list, etc.).
         Validate character count stays under 4,200.

STEP 4 — Paste both parts into Copilot Studio Instructions field.
         Test in the Test Chat pane with the 5 standard scenarios (see 03-operating-model.md).

STEP 5 — Build topics using 02-topics.md as the template.
         Link topics to the skills defined in the skills/ folder.

STEP 6 — Execute the 5-step action plan (03-operating-model.md) to complete all skills.
```

---

## 4. Dependencies Map

```
project-context.md
  └─► 01-instructions-part-a.md  (Identity, Tools)
  └─► 01-instructions-part-b.md  (Rules, Error Handling, Example)
  └─► 02-topics.md               (Topic names, trigger phrases, slot values)
  └─► 03-operating-model.md      (Action plan references all 7 skills)
        └─► skills/01-conversation-design.md
        └─► skills/02-system-prompt-engineering.md
        └─► skills/03-knowledge-base-configuration.md
        └─► skills/04-action-integration-design.md
        └─► skills/05-auth-security-configuration.md
        └─► skills/06-channel-deployment.md
        └─► skills/07-testing-quality-assurance.md
```

---

## 5. Quality Gates Before Go-Live

| Gate | Criteria | Owner |
|------|----------|-------|
| Instructions validated | Both parts fit within character limit and pass 5 standard test scenarios | Conversation Designer |
| Topics complete | All triggered, system, and fallback topics configured with trigger phrases | Conversation Designer |
| Skills 01–06 signed off | All deliverables produced and approved | Project Lead |
| Skill 07 passed | All test cases executed, go/no-go approved | QA |
| Environment promoted | Solution exported from Test, imported to Production | IT |
