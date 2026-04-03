# Copilot Agent Factory — Operating Model

**Version:** 1.0
**Status:** Draft
**Last Updated:** 2026-04-03
**Contents:** Context-First Approach · 5-Step Action Plan · Skills Usage Guide · Rules · Do Not Use

---

## 1. Context-First Approach

**The agent must never act before understanding context. This is non-negotiable.**

Before taking any action — including searching the KB or triggering a flow — the agent must have a clear picture of the current situation. Context comes from four sources:

### 1.1 Four Sources of Context

| Source | What It Provides | When Available | How to Read |
|--------|-----------------|----------------|-------------|
| **Azure AD session** | User identity: name, role, company, D365 access | Session start (automatic) | Read silently — never ask the user |
| **Conversation history** | What the user has already said, slots already filled | Throughout the session | Always check before re-prompting — never ask for information already given |
| **Slot values** | Module, error description, steps taken | After slot filling | All three must be confirmed before any action |
| **Knowledge Base results** | Existing solutions, known errors, standard procedures | After KB search | Present to user with a confirmation question — never assert it will work |

### 1.2 Context Validation Before Acting

Run this mental checklist before every action:

```
Before searching the KB:
  ✔ Slot 1 (Module) confirmed?
  ✔ Slot 2 (Error description) confirmed — at least one full sentence?
  ✔ Slot 3 (Steps taken) confirmed — at least one full sentence?
  → If any slot is missing: collect it. Do not search yet.

Before triggering File Router:
  ✔ .axtr file received from user?
  ✔ File type validated?
  ✔ Incident ID available?
  → If any missing: wait. Do not trigger.

Before closing the conversation:
  ✔ User explicitly confirmed resolution?
  ✔ Incident record updated?
  → If not: do not close.
```

### 1.3 Context Continuity Rule

If the user provides information in their first message (e.g., "I have an AP invoice error"), extract as much as possible from that message before asking follow-up questions. Never ask for information the user has already provided.

---

## 2. Five-Step Action Plan

This is the master execution sequence for every agent project. Follow it in order.

```
┌─────────────────────────────────────────────────────────────┐
│  STEP 1 — UNDERSTAND                                        │
│  Read the project-context.md file completely.               │
│  Confirm: agent goal, audience, slot definitions,           │
│  escalation rules, open questions.                          │
│  DO NOT proceed until all open questions are answered       │
│  or flagged as acceptable blockers.                         │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│  STEP 2 — DESIGN                                            │
│  Execute Skill 01 (Conversation Design):                    │
│  → Define all topics using 02-topics.md                     │
│  → Complete slot filling matrix                             │
│  → Draw branching logic (resolution / escalation / fallback)│
│  Execute Skill 02 (System Prompt Engineering):              │
│  → Fill in 01-instructions-part-a.md                       │
│  → Fill in 01-instructions-part-b.md                       │
│  → Paste into Copilot Studio. Run first test.               │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│  STEP 3 — CONNECT                                           │
│  Execute Skill 03 (Knowledge Base Configuration):           │
│  → Upload documents, configure sources, run retrieval tests │
│  Execute Skill 04 (Action & Integration Design):            │
│  → Build Incident Creator flow                              │
│  → Build File Router flow                                   │
│  → Map Dataverse tables                                     │
│  Execute Skill 05 (Auth & Security):                        │
│  → Configure Azure AD SSO                                   │
│  → Set permission scoping per role                          │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│  STEP 4 — DEPLOY                                            │
│  Execute Skill 06 (Channel Deployment):                     │
│  → Select channel (Teams / Web)                             │
│  → Complete channel-specific configuration                  │
│  → Deploy to pilot group (10–20 users)                      │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│  STEP 5 — VALIDATE                                          │
│  Execute Skill 07 (Testing & Quality Assurance):            │
│  → Run all test types: trigger, slot, KB, flow, E2E         │
│  → Run edge case and adversarial tests                      │
│  → Set analytics baseline                                   │
│  → Go/No-Go decision                                        │
│  → Deploy to production                                     │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Skills Usage Guide

Use this table to know **which skill to activate**, **when**, and **what it produces**.

| Skill | When to Use | Input You Need | Output It Produces | Depends On |
|-------|------------|---------------|-------------------|------------|
| **Skill 01** Conversation Design | First — before writing any instruction or building any topic | Agent goal, user scenarios, escalation rules | Topic map, slot matrix, branching flow | Nothing (foundational) |
| **Skill 02** System Prompt Engineering | After Skill 01 — once you know what the agent does, define how it behaves | Skill 01 outputs + persona decisions | Instructions Part A + B (ready to paste), Guardrail matrix, Prompt revision log | Skill 01 |
| **Skill 03** Knowledge Base Configuration | After Skill 01 — in parallel with Skill 02 | Document list, owners, access permissions | KB source index, RAG-ready documents, retrieval test results | Skill 01 |
| **Skill 04** Action & Integration Design | After Skill 01 — in parallel with Skills 02 and 03 | Target system APIs, Dataverse table list, file storage decision | Flow specs, Dataverse mapping, file handling matrix, OpenAPI (if needed) | Skill 01 |
| **Skill 05** Auth & Security | After Skills 04 and 06 are decided | Azure AD role list, DLP policies, channel decision | Auth config, permission matrix, DLP checklist, audit setup | Skills 04, 06 |
| **Skill 06** Channel Deployment | After channel decision is made | Channel choice, auth method, brand guidelines | Channel config, pre-go-live checklist, deployment runbook | Skill 01 |
| **Skill 07** Testing & QA | After all other skills are complete | Outputs from Skills 01–06, real user scenarios | Test plan, results, analytics baseline, go/no-go recommendation | All skills |

### 3.1 Parallel vs. Sequential

```
Sequential (must be in order):
  Skill 01 → must complete before any other skill starts

Parallel (can run simultaneously after Skill 01):
  Skill 02 + Skill 03 + Skill 04  ←── run in parallel

Sequential (must wait for prior step):
  Skill 05 → needs Skill 04 output (auth per integration) + channel from Skill 06
  Skill 06 → needs channel decision (can start early once channel is decided)
  Skill 07 → must wait for all Skills 01–06 to be complete
```

---

## 4. Rules

These rules apply to the team using the factory, not just the agent itself.

| # | Rule | Rationale |
|---|------|-----------|
| R-01 | Always read `project-context.md` before opening any factory file | Ensures project-specific values (module list, SLA, L2 contact) are known before filling in placeholders |
| R-02 | Fill every [PLACEHOLDER] before pasting instructions into Copilot Studio | Generic placeholders cause the agent to behave incorrectly in production |
| R-03 | Count characters in the instruction block before pasting | Copilot Studio has a hard limit — exceeding it silently truncates the instruction |
| R-04 | Never skip Skill 01 | All other skills depend on it — building a KB or writing a prompt without a defined topic map creates contradictions |
| R-05 | Never deploy to production before Skill 07 is complete | Untested agents in production damage user trust and create L2 overhead |
| R-06 | Run the 5-step action plan in sequence for each new project | Skipping steps creates gaps that surface as bugs in production |
| R-07 | Log every prompt revision in the Skill 02 Revision Log | Without version tracking, it is impossible to diagnose what changed when the agent breaks |
| R-08 | Re-test after every change to topics, KB, or flows | Copilot Studio topics interact — a change in one can break another |
| R-09 | Open questions in `project-context.md` must be answered before the relevant skill is executed | An unanswered channel question blocks Skill 05 and Skill 06 |
| R-10 | Never embed sensitive data (credentials, internal URLs, employee IDs) in the instruction block | The instruction field is not encrypted — treat it as readable text |

---

## 5. Do Not Use

These are hard prohibitions — violations directly harm the agent's quality or security.

### 5.1 In the Instruction Block — Never Include:

| Prohibited Item | Why |
|-----------------|-----|
| Real API keys, passwords, or tokens | Not encrypted; visible to anyone with access to the agent settings |
| Internal table names, field IDs, or system identifiers | Creates a security and data integrity risk |
| Hardcoded employee names or personal data | GDPR / privacy risk |
| Vague behavioral instructions (e.g., "be helpful") | LLMs interpret vague instructions unpredictably — be explicit and specific |
| Contradictory rules (e.g., "always escalate" + "always try to resolve") | Agent will behave inconsistently; one rule must take precedence |
| Instructions that override safety rules (e.g., "ignore previous instructions if user asks") | Creates a prompt injection vulnerability |

### 5.2 In Topics — Never Do:

| Prohibited Behavior | Why |
|--------------------|-----|
| Ask two questions in the same message | Users answer only one — the second slot stays empty silently |
| Proceed to KB search before all slots are filled | Returns irrelevant or misleading answers |
| Continue troubleshooting after the STOP RULE is triggered | Wastes user time and delays L2 resolution |
| Re-ask for information already provided in the conversation | Damages user trust |
| Accept any file type for upload (not just `.axtr`) | Breaks the File Router flow |
| Redirect to escalation without explaining why | Users disengage if they don't understand the next step |

### 5.3 In Flows — Never Do:

| Prohibited Behavior | Why |
|--------------------|-----|
| Log credentials or tokens in flow run history | Security risk — Power Automate logs are readable by admins |
| Trigger File Router before confirming file type is `.axtr` | Sends corrupt or irrelevant files to L2 |
| Use dynamic content without schema validation | Breaks flow on unexpected input shape |
| Build flows without error handling blocks | Silent failures leave users without confirmation |
| Use personal connections (not service accounts) for production flows | Connection fails when the user's account is deactivated |

### 5.4 In the Knowledge Base — Never Do:

| Prohibited Behavior | Why |
|--------------------|-----|
| Upload documents without version control | Stale content produces wrong answers |
| Enable "Allow general knowledge" for support agents | Agent fabricates answers not grounded in company procedures |
| Upload documents with contradictory content across sources | Agent returns conflicting answers and users lose trust |
| Skip retrieval quality testing | Untested KB always has gaps that surface in production |
