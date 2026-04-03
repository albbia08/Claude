# Project Context: L1 D365 Support Agent

**Project:** L1 Dynamics 365 Support Agent
**Framework Version:** 1.0
**Status:** In Definition
**Last Updated:** 2026-04-03

---

## 1. Agent Goal

Act as a first-level (L1) support agent for Dynamics 365. Qualify every request before suggesting any solution. If the Knowledge Base does not resolve the issue, guide the user through creating a Task Recorder file and route it to the L2 team.

---

## 2. Mandatory Scoping Questions (Slot Filling)

The agent must collect **all three** before any action:

| Slot # | Question | Entity Type | Validation | Required |
|--------|----------|-------------|------------|----------|
| 1 | Which D365 module are you working in? | Closed List | Must match approved module list | Yes |
| 2 | What error message or unexpected behavior are you seeing? | Free Text | Minimum 10 characters | Yes |
| 3 | What steps did you take before the issue occurred? | Free Text | Minimum 10 characters | Yes |

### Approved Module List (Slot 1)

| Module Code | Display Name |
|-------------|-------------|
| GL | General Ledger |
| AP | Accounts Payable |
| AR | Accounts Receivable |
| FA | Fixed Assets |
| CB | Cash & Bank Management |
| SCM | Supply Chain Management |
| SALES | Sales |
| C2P | Concept to Product |
| PROD | Production |
| WM | Warehouse Management |
| OTHER | Other / Not Sure |

---

## 3. Escalation Logic ("Stop" Rule)

If the Knowledge Base does not return a solution that resolves the issue (confirmed by the user):

1. **Stop** active troubleshooting immediately — do not attempt further suggestions
2. **Deliver** step-by-step Task Recorder instructions (see Section 3.1)
3. **Request** upload of the resulting `.axtr` file
4. **Route** the file via Power Automate to the L2 team
5. **Confirm** to the user that the file has been submitted

### 3.1 Task Recorder Instructions (Agent Delivers to User)

> 1. In D365, go to **Settings** (gear icon) > **Task recorder**
> 2. Click **Create recording**
> 3. Enter a name for the recording (e.g., "AP Invoice Error - [your name]")
> 4. Click **Start**
> 5. Reproduce the exact steps that lead to the error
> 6. When the error appears, click **Stop** in the Task Recorder panel
> 7. Click **Save to this PC** and select the `.axtr` format
> 8. Upload the saved `.axtr` file here in this chat

---

## 4. System Prompt (Draft v0.1)

> This is the initial draft. The system prompt will be iteratively refined per **Skill 02 (System Prompt Engineering)**. See the Prompt Revision Log in Skill 02 deliverables for version history.

```
You are a Level 1 Support Engineer for Dynamics 365. Your primary goal is accurate 
scoping — do not suggest solutions until you have confirmed the module, the error or 
behavior, and the steps the user took.

PROCESS:
1. Greet the user and ask which D365 module they are working in.
2. If the user provides a module not on the approved list (GL, AP, AR, FA, CB, SCM, 
   SALES, C2P, PROD, WM), ask them to choose from the list or select "Other."
3. Ask what error message or unexpected behavior they are experiencing.
   If the description is too brief (less than one sentence), ask for more detail.
4. Ask what steps they took before the issue occurred.
   If the description is too brief (less than one sentence), ask for more detail.
5. Only after collecting all three answers, search the Knowledge Base.
6. If the KB returns a relevant result, present it and ask: "Did this resolve your issue?"
7. If the user says Yes, summarize the resolution and close.
8. If the user says No, OR if the KB returned no result:
   - Stop troubleshooting immediately.
   - Provide Task Recorder instructions.
   - Request the .axtr file upload.
   - Confirm the file has been submitted to the L2 team.

MODULE "OTHER" HANDLING:
- If the user selects "Other / Not Sure," do NOT search the Knowledge Base (KB is 
  organized by module and results will be unreliable).
- Instead, proceed directly to the escalation path: deliver Task Recorder instructions 
  and route to the L2 team for manual triage.

GUARDRAILS:
- Never suggest a solution before all three scoping questions are answered.
- Never make configuration change recommendations — that is L2/L3 scope.
- Never answer questions outside of Dynamics 365.
- Never fabricate an answer. If you don't know, say so and escalate.
- Never share internal system details (table names, field IDs, technical metadata).
- Be professional, concise, and patient.
```

---

## 5. Channel Decision

| Channel | Pros | Cons | Recommendation |
|---------|------|------|----------------|
| **Microsoft Teams** | SSO automatic, file upload native, users already there | Limited branding | **Preferred** |
| **Web** | Full branding, public access possible | File upload needs config, auth needs setup | Backup option |

**Decision status:** Open — must be finalized before Power Automate flow can be built.

---

## 6. Dataverse Tables to Map

| Table | Purpose | Confirmed | MCP to Validate |
|-------|---------|-----------|-----------------|
| Incident (or equivalent) | Store support request records | No | D365-KB |
| Error log tables | Contextual error data for scoping | No | D365-KB |
| User / Role tables | Identify user profile and permissions | No | D365sec |

---

## 7. Knowledge Base Sources (Initial List)

| Source ID | Type | Content Scope | Owner | Refresh Cadence | Status |
|-----------|------|---------------|-------|-----------------|--------|
| KB-L1-001 | PDF/DOCX | D365 module user manuals | Training Team | Quarterly | Pending |
| KB-L1-002 | SharePoint | Internal support procedures | Support Team | Monthly | Pending |
| KB-L1-003 | To be created | Known error catalog | L2 Team | As needed | Not Started |

---

## 8. Integration Requirements

| Integration | Type | Trigger | Description | Status |
|-------------|------|---------|-------------|--------|
| `.axtr` file routing | Power Automate | User uploads file after escalation | Route file to L2 team (SharePoint or DevOps — TBD) | Not Started |
| Incident creation | Power Automate / Dataverse | Every new support conversation | Create a support record in Dataverse | Not Started |
| User identification | Azure AD claims | Conversation start | Read user role for permission scoping | Not Started |

---

## 9. SLA & Response Time Expectations

| Metric | Target | Notes |
|--------|--------|-------|
| Agent response latency | < 5 seconds per message | Copilot Studio standard; monitor via Analytics |
| Agent availability | 24/7 (automated) | No human dependency for L1 |
| L2 team pickup after escalation | *(TBD — define SLA with L2 team)* | Starts when `.axtr` file is routed |
| User notification after L2 pickup | *(TBD)* | Email or Teams notification? |

---

## 10. User Adoption & Change Management

| Activity | Owner | Timing | Status |
|----------|-------|--------|--------|
| Pilot group selection (10–20 users) | Project Lead | Before go-live | Not Started |
| User communication (what the agent does, how to access it) | Project Lead | 1 week before go-live | Not Started |
| Quick-start guide / training material | Training Team | Before go-live | Not Started |
| Feedback collection mechanism (survey or Teams channel) | Project Lead | At go-live | Not Started |
| Post-pilot review and adjustments | Project Lead + QA | 2 weeks after go-live | Not Started |

---

## 11. Open Questions (L1-Specific)

| # | Question | Owner | Status |
|---|----------|-------|--------|
| 1 | Which channel: Teams or Web? | Project Lead | Open |
| 2 | Where does the `.axtr` file get stored: SharePoint or Azure DevOps? | L2 Team Lead | Open |
| 3 | Which Dataverse tables are in scope for agent context reads? | D365 Admin | Open |
| 4 | Who owns the Knowledge Base documents and at what cadence are they updated? | Business Owner | Open |
| 5 | Is user authentication required or is the agent open to all internal users? | Security Lead | Open |
| 6 | What is the L2 team SLA for picking up escalated tickets? | L2 Team Lead | Open |
| 7 | Is multi-language support required for non-English entities? | Project Lead | Open |

---

## 12. Next Steps

1. Answer open questions in Section 9
2. Draft the full Topic Map (Skill 01 deliverable)
3. Upload initial Knowledge Base documents and run retrieval quality test (Skill 03)
4. Build the Power Automate flow for `.axtr` routing — pending channel decision (Skill 04)
5. Run end-to-end test with a real D365 support scenario (Skill 07)
