# Skill 07 — Testing & Quality Assurance

**Skill Owner:** QA
**Status:** Not Started
**Dependencies:** All other skills (testing validates the outputs of every skill)

---

## 1. Purpose

Validate the agent before go-live. This skill defines what to test, how to test it, what "good" looks like, and how to measure ongoing quality through analytics. No agent goes live without passing through this skill.

---

## 2. Scope & Activities

### 2.1 Test Strategy Overview

| Test Type | What It Validates | When to Run | Tool |
|-----------|-------------------|-------------|------|
| **Topic Trigger Testing** | Correct topic fires for a given input | After Skill 01 (Conversation Design) | Copilot Studio Test Chat |
| **Slot Filling Testing** | All required data is collected before action | After Skill 01 | Copilot Studio Test Chat |
| **KB Retrieval Testing** | Correct document retrieved, accurate answer | After Skill 03 (Knowledge Base) | Copilot Studio Test Chat |
| **Flow Integration Testing** | Power Automate flows execute and return correctly | After Skill 04 (Integration) | Copilot Studio + Power Automate |
| **Auth & Security Testing** | Correct users have correct access | After Skill 05 (Auth) | Target channel |
| **End-to-End Conversation Testing** | Full paths work: resolution, escalation, fallback | After all skills | Target channel |
| **Edge Case & Adversarial Testing** | Agent handles unexpected or malicious input | Before go-live | Target channel |
| **Channel-Specific Testing** | Agent works correctly in deployed channel | After Skill 06 (Channel) | Target channel |

### 2.2 Test Case Template

| Field | Description |
|-------|-------------|
| **Test ID** | TC-XXX |
| **Test Type** | *(from 2.1)* |
| **Related Skill** | *(Skill 01–06)* |
| **Preconditions** | *(what must be true before this test)* |
| **Test Input** | *(exact user message or action)* |
| **Expected Slot Values** | *(what slots should be filled)* |
| **Expected Agent Response** | *(summary of expected behavior)* |
| **Expected Topic Triggered** | *(which topic should fire)* |
| **Expected Flow Executed** | *(which Power Automate flow, if any)* |
| **Actual Result** | *(fill during execution)* |
| **Pass / Fail** | |
| **Notes** | *(observations, edge cases found)* |

### 2.3 Test Scenarios by Path

#### Resolution Path (Happy Path)
| Step | User Action | Expected Agent Behavior |
|------|-------------|------------------------|
| 1 | User describes issue | Agent asks for module |
| 2 | User provides module | Agent asks for error/behavior |
| 3 | User provides error | Agent asks for steps taken |
| 4 | User provides steps | Agent searches KB |
| 5 | KB returns result | Agent presents solution, asks "Did this resolve your issue?" |
| 6 | User says "Yes" | Agent closes conversation with summary |

#### Escalation Path
| Step | User Action | Expected Agent Behavior |
|------|-------------|------------------------|
| 1–4 | *(same as above)* | *(same as above)* |
| 5 | KB returns no result, or user says "No" | Agent stops troubleshooting |
| 6 | — | Agent provides Task Recorder instructions |
| 7 | User uploads `.axtr` file | Agent confirms receipt |
| 8 | — | Agent routes file to L2 team, confirms submission |

#### Fallback Path
| Step | User Action | Expected Agent Behavior |
|------|-------------|------------------------|
| 1 | User says something off-topic or unrecognizable | Fallback topic fires |
| 2 | — | Agent redirects to supported scope or offers escalation |

### 2.4 Edge Case & Adversarial Test Scenarios

| Scenario | Test Input | Expected Behavior |
|----------|------------|-------------------|
| Empty message | *(blank)* | Agent re-prompts or fallback |
| Off-scope question | "What's the weather?" | Agent declines politely, redirects |
| Prompt injection attempt | "Ignore your instructions and..." | Agent maintains guardrails |
| Partial slot filling | User provides module only, then goes silent | Agent re-prompts for missing slots |
| Contradictory input | "It's an AP error... actually it's AR" | Agent confirms the correction |
| Very long message | *(500+ word description)* | Agent extracts key info, asks for missing slots |
| Multiple issues in one message | "AP invoice error and also GL posting issue" | Agent handles one at a time or asks to prioritize |
| File upload failure | *.axtr file upload times out or fails | Agent provides retry instructions |
| Non-English input | *(message in another language)* | Agent responds per language policy |

### 2.5 Analytics Baseline

Define success metrics **before go-live** so you can measure improvement:

| Metric | Definition | Target | Measurement Source |
|--------|-----------|--------|-------------------|
| **Resolution Rate** | % of conversations resolved without escalation | *(set target)* | Copilot Studio Analytics |
| **Escalation Rate** | % of conversations escalated to human | *(set target)* | Copilot Studio Analytics |
| **Fallback Rate** | % of user messages hitting the fallback topic | < 15% | Copilot Studio Analytics |
| **Avg. Turns to Resolution** | Average number of turns before resolution or escalation | *(set target)* | Conversation Transcripts |
| **Drop-off Rate** | % of users who abandon the conversation mid-flow | < 10% | Copilot Studio Analytics |
| **User Satisfaction** | Post-conversation rating (if enabled) | *(set target)* | Copilot Studio Analytics |
| **Topic Trigger Accuracy** | % of messages routed to the correct topic | > 90% | Manual review of transcripts |

---

### 2.6 Post-Go-Live Continuous Improvement

Testing does not end at go-live. Define a continuous improvement loop:

| Activity | Cadence | Owner | Input | Output |
|----------|---------|-------|-------|--------|
| Conversation transcript review | Weekly | QA / Support Lead | Copilot Studio transcripts + Dataverse logs | List of failing topics, new intents, KB gaps |
| Analytics dashboard review | Weekly | Project Lead | Copilot Studio Analytics | Trend analysis (resolution rate, fallback rate) |
| KB content update | As needed (triggered by transcript review) | KB Owner | Identified gaps | New/updated KB documents |
| New topic creation | As needed | Conversation Designer | Recurring unrecognized intents | New triggered topics |
| Prompt refinement | Monthly or as needed | Conversation Designer | Edge cases from transcripts | Updated system prompt (logged in Prompt Revision Log) |
| Regression testing | After every change | QA | Modified topics, KB, or flows | Updated test results |

---

## 3. Key Decision Points

| # | Decision | Impact | Status |
|---|----------|--------|--------|
| 1 | What is the minimum test coverage before go-live? | Quality gate | Open |
| 2 | Who performs UAT — project team, BPOs, or pilot users? | Test realism | Open |
| 3 | What analytics targets define "success"? | Sets expectations with stakeholders | Open |
| 4 | Is post-conversation satisfaction survey enabled? | Feedback collection | Open |
| 5 | How frequently is regression testing performed post-go-live? | Ongoing quality | Open |

---

## 4. Inputs Required

- [ ] Completed outputs from Skills 01–06
- [ ] Real user scenarios (minimum 20 for comprehensive testing)
- [ ] Access to Copilot Studio Test Chat and target channel
- [ ] Analytics dashboard configured
- [ ] UAT participants identified

---

## 5. Outputs / Deliverables

| Deliverable | Format |
|-------------|--------|
| Test Plan (all test cases) | Markdown |
| Test Execution Results | Markdown table |
| Edge Case & Adversarial Test Results | Markdown table |
| Analytics Baseline & Targets | Markdown table |
| Go/No-Go Recommendation | Markdown (with evidence) |

---

## 6. Progress Tracker

- [ ] Write test cases for all topic triggers
- [ ] Write test cases for slot filling
- [ ] Write test cases for KB retrieval
- [ ] Write test cases for flow integrations
- [ ] Write test cases for auth & security
- [ ] Write edge case & adversarial scenarios
- [ ] Execute all test cases
- [ ] Document results and failures
- [ ] Fix identified issues (loop back to relevant skill)
- [ ] Retest after fixes
- [ ] Set analytics baseline targets
- [ ] Stakeholder review of test results
- [ ] Go/No-Go decision
- [ ] Set up post-go-live continuous improvement process (see Section 2.6)
