# Skill 02 — System Prompt Engineering

**Skill Owner:** Conversation Designer
**Status:** Not Started
**Dependencies:** Skill 01 (Conversation Design — defines what the agent does; this skill defines how it behaves)

---

## 1. Purpose

Write the behavioral rules that define the agent's identity, constraints, and decision logic. The system prompt is the single most important configuration artifact — it governs tone, scope, guardrails, and reasoning behavior across all conversations.

---

## 2. Scope & Activities

### 2.1 Agent Persona Definition

Define who the agent is in a structured format:

| Attribute | Value |
|-----------|-------|
| **Role** | *(e.g., L1 Support Engineer for Dynamics 365)* |
| **Tone** | *(e.g., Professional, helpful, concise)* |
| **Scope** | *(e.g., D365 Finance & SCM support only)* |
| **Language** | *(e.g., English only)* |
| **Personality Traits** | *(e.g., Patient, methodical, never guesses)* |

### 2.2 Guardrails (Explicit Constraints)

Define what the agent must **never** do:

| Guardrail | Rationale |
|-----------|-----------|
| Never suggest a solution without confirming module + error + steps | Prevents misdiagnosis |
| Never make configuration changes or advise users to change settings | Out of L1 scope |
| Never answer questions outside D365 scope | Prevents scope creep |
| Never share internal system information (table names, field IDs) | Security |
| Never fabricate an answer if the KB returns no results | Accuracy over speed |

### 2.3 Decision Logic (Natural Language Rules)

Encode the agent's reasoning process as explicit instructions:

```
1. First, greet the user and ask which D365 module they are working in.
2. Then, ask what error message or unexpected behavior they are experiencing.
3. Then, ask what steps they took before the issue occurred.
4. Only after all three answers are collected, search the Knowledge Base.
5. If the KB returns a relevant result, present it and ask: "Did this resolve your issue?"
6. If the user confirms resolution, close the conversation.
7. If the user says No, or if the KB returned no result:
   a. Stop troubleshooting immediately.
   b. Provide Task Recorder instructions.
   c. Request the .axtr file upload.
   d. Confirm submission to the L2 team.
```

### 2.4 Prompting Principles

Apply these principles when drafting and refining the system prompt:

| Principle | Application |
|-----------|-------------|
| **Be specific** | Define exactly what structured input is required — no generic "How can I help?" |
| **Step-by-step reasoning** | Instruct the agent to think through the problem sequentially |
| **Role assignment** | The prompt defines who the agent IS, not just what it does |
| **Negative instructions** | Explicitly state what NOT to do — LLMs respond well to boundaries |
| **Iterative refinement** | Expect 3–5 prompt revisions; test each version against real scenarios |

### 2.5 Prompt Length & Structure Guidelines

- Keep the system prompt concise — verify against Copilot Studio's current character/token limit for the Instructions field (aim for under 2000 characters as a guideline)
- Structure with clear sections: Identity → Scope → Process → Guardrails → Escalation
- Reference Knowledge Sources by name rather than embedding facts inline
- Use numbered steps for sequential logic, bullet points for parallel rules

---

## 3. Key Decision Points

| # | Decision | Impact | Status |
|---|----------|--------|--------|
| 1 | What tone: formal, friendly, or neutral? | Sets user expectation | Open |
| 2 | Should the agent explain its reasoning to the user? | Transparency vs. speed | Open |
| 3 | How should the agent handle off-scope questions? | Redirect vs. hard refusal | Open |
| 4 | Should the agent support multiple languages? | Prompt complexity increases | Open |

---

## 4. Inputs Required

- [ ] Agent persona attributes (role, tone, scope)
- [ ] Complete list of guardrails from stakeholders
- [ ] Decision logic flow from Skill 01 (Conversation Design)
- [ ] Real user scenarios for testing prompt behavior
- [ ] Feedback from first round of testing

---

## 5. Outputs / Deliverables

| Deliverable | Format |
|-------------|--------|
| System Prompt (final version) | Markdown (`.md`) |
| Prompt Revision Log | Markdown table (version, date, change, test result, decision) |
| Guardrail Matrix | Markdown table |

### Prompt Revision Log Template

| Version | Date | Change Made | Test Result | Decision |
|---------|------|-------------|-------------|----------|
| v0.1 | *(date)* | Initial draft | *(pass/fail + notes)* | *(keep/revise)* |
| v0.2 | *(date)* | *(what changed)* | *(pass/fail + notes)* | *(keep/revise)* |

---

## 6. Progress Tracker

- [ ] Define agent persona attributes
- [ ] Draft guardrail matrix
- [ ] Write decision logic in natural language
- [ ] Draft system prompt v0.1
- [ ] Test against 5+ real user scenarios
- [ ] Revise prompt based on test results (target: v0.3 minimum)
- [ ] Stakeholder review and sign-off
- [ ] Final prompt committed to agent configuration
