# Skill 05 — Authentication & Security Configuration

**Skill Owner:** Security Lead
**Status:** Not Started
**Dependencies:** Skill 04 (Integration Design — defines what systems need auth), Skill 06 (Channel — auth method varies by channel)

---

## 1. Purpose

Control who can use the agent, what data it can access, and how user identity flows through conversations and integrations. This skill ensures the agent operates within the organization's security boundaries.

---

## 2. Scope & Activities

### 2.1 Authentication Method Selection

| Channel | Auth Method | SSO Available | User Identity Passed to Flows | Notes |
|---------|-------------|---------------|-------------------------------|-------|
| **Microsoft Teams** | Azure AD (Entra ID) | Yes (automatic) | Yes — via claims | Preferred for internal users |
| **Web (Custom Canvas)** | OAuth 2.0 / Azure AD | Requires config | Yes — via token exchange | Requires explicit setup |
| **SharePoint** | Azure AD | Yes (inherited) | Yes | Limited to embedded experience |
| **Anonymous / Public** | None | No | No | Not recommended for D365 data |

**Decision required:** Internal-only (authenticated) or public-facing (anonymous)?

### 2.2 Agent-Level Security Settings

Configure in Copilot Studio under **Settings > Security**:

| Setting | Options | Recommendation |
|---------|---------|----------------|
| **Require user sign-in** | On / Off | **On** for any agent accessing D365 data |
| **Authentication provider** | Azure AD / Manual | Azure AD for Microsoft 365 environments |
| **Token exchange URL** | *(if manual)* | Only needed for custom OAuth setups |
| **Session timeout** | *(minutes)* | Align with org security policy |

### 2.3 Permission Scoping

Define what the agent can access based on user role:

| User Role | D365 Modules Visible | Actions Permitted | Data Access Level | KB Sources Available |
|-----------|---------------------|-------------------|-------------------|---------------------|
| *(e.g., AP Clerk)* | *(e.g., AP, GL)* | *(e.g., Read, Create ticket)* | *(e.g., Own company only)* | *(e.g., AP procedures)* |
| *(e.g., Finance Controller)* | *(e.g., All Finance)* | *(e.g., Read, Escalate)* | *(e.g., Cross-company)* | *(e.g., All Finance KB)* |

**MCP Tool:** Use **D365sec** to map existing D365 security roles to agent permission profiles.

### 2.4 Role-Based Conversation Logic

When different user profiles require different conversation paths:

```
User authenticates
  └─► Agent reads user role from Azure AD claims
        ├─► Role = AP Clerk → Topic: AP Support (scoped KB + actions)
        ├─► Role = Finance Controller → Topic: Finance Support (broader scope)
        └─► Role = Unknown/No Access → Topic: Access Denied message
```

### 2.5 Data Loss Prevention (DLP) Policies

| Policy | Scope | Action |
|--------|-------|--------|
| No PII in agent responses | All conversations | Agent must not surface personal data beyond user's own profile |
| No credentials in logs | All flows | Ensure tokens and keys are not logged in conversation transcripts |
| Connector grouping | Power Platform DLP | Ensure agent connectors are in the same DLP group |

### 2.6 Audit & Compliance

| Audit Point | Where to Configure | What to Log |
|-------------|-------------------|-------------|
| Conversation transcripts | Copilot Studio Analytics + Dataverse | All conversations (for review and improvement) |
| Flow execution logs | Power Automate run history | Inputs, outputs, errors |
| Authentication events | Azure AD sign-in logs | Who accessed the agent and when |
| Data access events | Dataverse audit log | What records were read/written via the agent |

---

## 3. Key Decision Points

| # | Decision | Impact | Status |
|---|----------|--------|--------|
| 1 | Internal (authenticated) or public (anonymous) agent? | Determines entire auth architecture | Open |
| 2 | Which Azure AD groups / D365 roles map to agent access? | Scopes who can use the agent | Open |
| 3 | Should the agent behave differently based on user role? | Complexity vs. personalization | Open |
| 4 | Where are conversation transcripts stored and who can access them? | Privacy and compliance | Open |
| 5 | DLP policy group for agent connectors? | Must align with org Power Platform DLP | Open |

---

## 4. Inputs Required

- [ ] Organization's Azure AD structure (relevant groups and roles)
- [ ] D365 security role matrix (from D365sec MCP)
- [ ] DLP policy documentation from Power Platform admin
- [ ] Data classification of information the agent will handle
- [ ] Compliance requirements (GDPR, internal audit)

---

## 5. Outputs / Deliverables

| Deliverable | Format |
|-------------|--------|
| Security & Auth Configuration | Markdown |
| Permission Scoping Matrix | Markdown table |
| DLP Compliance Checklist | Markdown checklist |
| Audit Configuration Guide | Markdown |

---

## 6. Progress Tracker

- [ ] Decide: authenticated vs. anonymous
- [ ] Select authentication method per channel
- [ ] Map Azure AD roles to agent permission profiles
- [ ] Configure agent-level security settings
- [ ] Define role-based conversation logic (if applicable)
- [ ] Review DLP policies with Power Platform admin
- [ ] Configure audit logging
- [ ] Security review and sign-off
