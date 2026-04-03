# Copilot Agent — Topic Definitions

**Version:** 1.0
**Status:** Draft
**Applies to:** L1 D365 Support Agent (first instance)
**Related skill:** `skills/01-conversation-design.md`

---

## 1. Topic Architecture Overview

```
TRIGGERED TOPICS (user-initiated)
  ├── T-01  Report an Issue or Error
  ├── T-02  Request How-To Guidance
  └── T-03  General Inquiry / Unknown

SYSTEM TOPICS (built-in behaviors)
  ├── SYS-01  Greeting
  ├── SYS-02  End of Conversation
  ├── SYS-03  Escalation Handoff
  └── SYS-04  Fallback (unrecognized input)

REDIRECT SUB-FLOWS (reusable, called from triggered topics)
  ├── SUB-01  Slot Filling — Module
  ├── SUB-02  Slot Filling — Error Description
  ├── SUB-03  Slot Filling — Steps Taken
  ├── SUB-04  Task Recorder Instructions
  └── SUB-05  File Upload & L2 Routing
```

---

## 2. Triggered Topics

### T-01 — Report an Issue or Error

| Attribute | Value |
|-----------|-------|
| **Purpose** | User is experiencing a D365 error, unexpected behavior, or process failure |
| **Entry point** | User mentions a problem, error, or "not working" |
| **Slots required** | Module (SUB-01) → Error description (SUB-02) → Steps taken (SUB-03) |
| **Post-slot action** | Search Knowledge Base |
| **Resolution path** | KB answer found → present → confirm → close |
| **Escalation path** | No KB answer OR user says "No" → SUB-04 + SUB-05 |
| **Redirect to** | SUB-01, SUB-02, SUB-03, SUB-04 (if needed), SUB-05 (if needed) |

**Trigger Phrases (minimum 5–10):**
```
"I have an error"
"Something is not working"
"I'm getting a message"
"D365 is showing an error"
"I can't post"
"The system is blocking me"
"Error when I try to [action]"
"I'm stuck in [module]"
"There's a problem with my invoice"
"D365 won't let me complete"
```

**Conversation Flow:**
```
User triggers T-01
  └─► SYS-01 Greeting (if first message)
  └─► SUB-01 Collect Module
  └─► SUB-02 Collect Error Description
  └─► SUB-03 Collect Steps Taken
  └─► Action: Search Knowledge Base (query = module + error + steps)
        ├─► KB Result Found
        │     └─► Present answer
        │     └─► Ask: "Did this resolve your issue?"
        │           ├─► YES → log resolution → SYS-02 End of Conversation
        │           └─► NO  → SUB-04 Task Recorder → SUB-05 File Upload
        └─► No KB Result
              └─► SUB-04 Task Recorder → SUB-05 File Upload
```

---

### T-02 — Request How-To Guidance

| Attribute | Value |
|-----------|-------|
| **Purpose** | User needs step-by-step guidance on how to perform a D365 process |
| **Entry point** | User asks "how do I", "where do I find", "what is the process for" |
| **Slots required** | Module (SUB-01) → Task description (free text) |
| **Post-slot action** | Search Knowledge Base |
| **Resolution path** | KB answer found → present procedure → confirm |
| **Escalation path** | No KB answer → escalate with guidance request context |

**Trigger Phrases:**
```
"How do I [action] in D365"
"Where do I find [feature]"
"What is the process for [task]"
"Can you walk me through"
"I don't know how to"
"What steps do I follow"
"How does [feature] work"
```

**Conversation Flow:**
```
User triggers T-02
  └─► SUB-01 Collect Module
  └─► Ask: "What task or process do you need help with?"
  └─► Action: Search Knowledge Base (query = module + task)
        ├─► KB Result Found → Present procedure → Ask: "Was this helpful?"
        │     ├─► YES → SYS-02 End
        │     └─► NO  → SYS-03 Escalation Handoff (no Task Recorder needed for how-to)
        └─► No KB Result → SYS-03 Escalation Handoff
```

---

### T-03 — General Inquiry / Unknown

| Attribute | Value |
|-----------|-------|
| **Purpose** | Catch-all for vague or multi-intent messages not matching T-01/T-02 |
| **Entry point** | Message is unclear, combines multiple issues, or intent is ambiguous |
| **Action** | Clarify intent, then redirect to T-01 or T-02 |

**Trigger Phrases:**
```
"I need help"
"Can you help me with D365"
"I have a question"
"I need support"
"Something is wrong"
```

**Conversation Flow:**
```
User triggers T-03
  └─► Ask: "Are you experiencing an error, or do you need guidance on how to do something?"
        ├─► Error / problem → redirect to T-01
        └─► How-to / guidance → redirect to T-02
```

---

## 3. System Topics

### SYS-01 — Greeting

| Attribute | Value |
|-----------|-------|
| **Trigger** | Conversation start (first message in session) |
| **Action** | Greet user by name (from Azure AD), state agent purpose, invite the user to describe their issue |
| **Power Automate** | Trigger Incident Creator flow (log session start) |

**Message template:**
```
"Hello [USER FIRST NAME]. I'm the D365 Support Agent.
I'm here to help you with Dynamics 365 issues.

To get started — are you reporting an error, or do you need guidance on
how to complete a process?"
```

---

### SYS-02 — End of Conversation

| Attribute | Value |
|-----------|-------|
| **Trigger** | Resolution confirmed by user, or user says "thank you" / "bye" / "done" |
| **Action** | Summarize what was resolved, confirm incident closed, invite return |
| **Power Automate** | Update incident record: status = Resolved |

**Message template:**
```
"Thank you for reaching out. Here's a summary of what we covered:
- Module: [MODULE]
- Issue: [BRIEF SUMMARY]
- Resolution: [SOLUTION OR ESCALATED TO L2]

Your incident has been logged. If the issue returns, start a new conversation
and reference incident ID [INCIDENT ID]."
```

---

### SYS-03 — Escalation Handoff

| Attribute | Value |
|-----------|-------|
| **Trigger** | Called from T-01/T-02 after SUB-05 file upload confirmed, or if KB fails for T-02 |
| **Action** | Confirm submission, communicate next steps, close with SLA |
| **Power Automate** | File Router flow already triggered in SUB-05 — this topic only delivers the confirmation message |

**Message template:**
```
"Your case has been escalated to the L2 team with all the context collected.
- Incident ID: [INCIDENT ID]
- Module: [MODULE]
- Recording: Submitted

The L2 team will review your recording and respond within [SLA TIMEFRAME].
You will receive a notification at [USER EMAIL / TEAMS CHANNEL]."
```

---

### SYS-04 — Fallback (Unrecognized Input)

| Attribute | Value |
|-----------|-------|
| **Trigger** | No topic matches the user's message (below confidence threshold) |
| **Action** | Apologize briefly, redirect to supported scope, offer options |
| **Consecutive fallback limit** | After 3 consecutive fallbacks → SYS-03 Escalation Handoff |

**Message template:**
```
"I didn't quite understand that. I can help with:
- Reporting a D365 error
- Getting guidance on a D365 process

Which of these best describes what you need?"
```

---

## 4. Redirect Sub-Flows

### SUB-01 — Slot Filling: Module

| Attribute | Value |
|-----------|-------|
| **Entity type** | Closed list |
| **Valid values** | GL, AP, AR, FA, CB, SCM, SALES, C2P, PROD, WM, OTHER |
| **Validation** | Must match list exactly. If "OTHER": skip KB, go to SUB-04. |
| **Re-prompt limit** | 1 re-prompt, then fallback to "OTHER" path |

**Prompt:** `"Which D365 module are you working in?"`
**Re-prompt:** `"Please choose from: GL, AP, AR, FA, CB, SCM, SALES, C2P, PROD, WM, or Other."`

---

### SUB-02 — Slot Filling: Error Description

| Attribute | Value |
|-----------|-------|
| **Entity type** | Free text |
| **Validation** | Minimum one full sentence. If too brief: re-prompt once. |
| **Re-prompt limit** | 1 |

**Prompt:** `"What error message or unexpected behavior are you seeing?"`
**Re-prompt:** `"Can you describe the error in a bit more detail? For example, include the exact message text if there is one."`

---

### SUB-03 — Slot Filling: Steps Taken

| Attribute | Value |
|-----------|-------|
| **Entity type** | Free text |
| **Validation** | Minimum one full sentence. If too brief: re-prompt once. |
| **Re-prompt limit** | 1 |

**Prompt:** `"What steps did you take in D365 before the issue occurred?"`
**Re-prompt:** `"Can you walk me through each step? For example: 'I went to AP > Invoices > then clicked Post.'"`

---

### SUB-04 — Task Recorder Instructions

| Attribute | Value |
|-----------|-------|
| **Trigger** | Called from T-01 escalation path, or when module = OTHER |
| **Action** | Deliver step-by-step Task Recorder instructions. Do not deviate from the script. |

**Script (deliver verbatim):**
```
"I'll guide you through creating a Task Recorder so the L2 team can see exactly
what's happening.

Here are the steps:
1. In D365, go to Settings (gear icon) > Task recorder.
2. Click Create recording.
3. Name it: '[MODULE] Issue - [your name]' (e.g., 'AP Issue - John Smith').
4. Click Start.
5. Reproduce the exact steps that lead to the error.
6. When the error appears, click Stop in the Task Recorder panel.
7. Click Save to this PC and choose .axtr format.
8. Upload the saved .axtr file here in this chat.

Let me know when you're ready to upload."
```

---

### SUB-05 — File Upload & L2 Routing

| Attribute | Value |
|-----------|-------|
| **Trigger** | Called from SUB-04 after instructions delivered |
| **Action** | Wait for file upload. Validate file type (.axtr). Trigger File Router flow. |
| **File validation** | Extension must be `.axtr`. Reject all other types with a clear message. |
| **Power Automate** | Trigger File Router: pass file, user ID, incident ID, module |
| **On success** | Redirect to SYS-03 Escalation Handoff |
| **On failure** | Provide manual fallback (email L2 directly) |

---

## 5. Topic Status Tracker

| Topic ID | Name | Trigger Phrases Defined | Flow Built | Tested | Status |
|----------|------|------------------------|------------|--------|--------|
| T-01 | Report an Issue | [ ] | [ ] | [ ] | Not Started |
| T-02 | How-To Guidance | [ ] | [ ] | [ ] | Not Started |
| T-03 | General Inquiry | [ ] | [ ] | [ ] | Not Started |
| SYS-01 | Greeting | N/A | [ ] | [ ] | Not Started |
| SYS-02 | End of Conversation | N/A | [ ] | [ ] | Not Started |
| SYS-03 | Escalation Handoff | N/A | [ ] | [ ] | Not Started |
| SYS-04 | Fallback | N/A | [ ] | [ ] | Not Started |
| SUB-01 | Slot — Module | N/A | [ ] | [ ] | Not Started |
| SUB-02 | Slot — Error | N/A | [ ] | [ ] | Not Started |
| SUB-03 | Slot — Steps | N/A | [ ] | [ ] | Not Started |
| SUB-04 | Task Recorder | N/A | [ ] | [ ] | Not Started |
| SUB-05 | File Upload | N/A | [ ] | [ ] | Not Started |
