# Skill 01 — Conversation Design

**Skill Owner:** Conversation Designer
**Status:** Not Started
**Dependencies:** None — this is the foundational skill (upstream of Skills 02, 03, 04, 06, 07)

---

## 1. Purpose

Design the full dialogue architecture of the agent: every topic, every branching path, every data collection point. This skill defines **what the agent says and when**, ensuring structured, predictable conversations that collect the right information before acting.

---

## 2. Scope & Activities

### 2.1 Topic Definition

Define all topics the agent will handle:

| Topic Type | Description | Example |
|------------|-------------|---------|
| **Triggered Topics** | Fired by user intent (trigger phrases) | "I have an error in AP" |
| **System Topics** | Built-in behaviors (greeting, escalation, fallback) | Greeting, End of Conversation |
| **Fallback Topic** | Catches unrecognized input | "I'm not sure I understand..." |

**Best Practice:**
- Define 5–10 trigger phrases per topic, covering natural language variations
- Keep each topic focused on a single intent — use **topic redirects** for shared sub-flows
- Name topics with a consistent convention: `[Module] - [Action]` (e.g., `Support - Qualify Issue`)
- Configure the **topic trigger confidence threshold** — too low triggers false positives, too high causes unnecessary fallbacks
- Define a **consecutive fallback limit** — after N unrecognized inputs, auto-escalate to a human rather than looping

### 2.2 Slot Filling

Define the mandatory data points the agent must collect before any action.

| Slot | Entity Type | Required | Prompt |
|------|-------------|----------|--------|
| *(defined per project)* | *(closed list / regex / prebuilt)* | *(Yes/No)* | *(question text)* |

**Best Practice:**
- Use entity-bound "Ask a question" nodes so the runtime auto-fills slots from the initial utterance without re-prompting
- Define validation rules per slot (e.g., module must be from a closed list)
- Never proceed to action or KB search until all required slots are filled

### 2.3 Branching Logic

Define the decision tree after data collection:

```
User Input
  └─► Slot Filling (collect all required data)
        └─► Knowledge Base Search
              ├─► Solution Found → Present to User → Confirm Resolution
              │     ├─► Resolved → End
              │     └─► Not Resolved → Escalation Path
              └─► No Solution → Escalation Path
                    └─► Collect additional context → Route to human team
```

**Best Practice:**
- Always include a user confirmation step after presenting a KB answer ("Did this solve your issue?")
- Dead-end handling: never leave the user without a next step
- Cap conversation depth — if after N turns the issue is unresolved, escalate

### 2.4 Adaptive Cards & Rich Messages

Design rich message formats where the channel supports them:

| Card Type | Use Case | Channel Support |
|-----------|----------|-----------------|
| Adaptive Card | Multi-field input forms, structured summaries | Teams, Web |
| Hero Card | Single action with image | Teams, Web |
| Quick Replies | Predefined response buttons | Teams, Web |

**Best Practice:**
- Use Adaptive Cards for slot filling where multiple inputs can be collected in one step
- Always provide a plain-text fallback for channels that don't support cards
- Test card rendering in the target channel before go-live

---

## 3. Key Decision Points

| # | Decision | Impact | Status |
|---|----------|--------|--------|
| 1 | What are the minimum data points before the agent acts? | Defines slot filling requirements | Open |
| 2 | How many turns before forced escalation? | Prevents infinite loops | Open |
| 3 | Should Adaptive Cards be used for data collection? | Depends on channel choice | Open |
| 4 | Topic naming convention | Consistency across the project | Open |

---

## 4. Inputs Required

- [ ] List of user intents / scenarios the agent must handle
- [ ] Closed-list values for entity slots (e.g., D365 module list)
- [ ] Escalation rules and human team routing logic
- [ ] Channel decision (affects card support)

---

## 5. Outputs / Deliverables

| Deliverable | Format |
|-------------|--------|
| Topic Map (all topics with trigger phrases) | Diagram or Copilot Studio export |
| Slot Filling Matrix | Markdown table |
| Conversation Flow Diagram | Mermaid or Visio |
| Adaptive Card Designs | JSON templates |

---

## 6. Progress Tracker

- [ ] Define triggered topics and trigger phrases
- [ ] Define system and fallback topics
- [ ] Define slot filling matrix with entities and validation
- [ ] Design branching logic (resolution, escalation, dead-end)
- [ ] Design Adaptive Cards (if applicable)
- [ ] Peer review of conversation flows
- [ ] Sign-off from project lead
