# Skill 04 — Action & Integration Design

**Skill Owner:** Integration Designer
**Status:** Not Started
**Dependencies:** Skill 01 (Conversation Design — defines when actions are triggered)

---

## 1. Purpose

Connect the agent to external systems so it can take action, not just answer questions. This skill covers Power Automate flows, custom connectors, Dataverse operations, file handling, and HTTP API calls.

---

## 2. Scope & Activities

### 2.1 Integration Inventory

Map every integration the agent requires:

| Integration ID | System | Action Type | Trigger | Direction | Auth Method | Status |
|----------------|--------|-------------|---------|-----------|-------------|--------|
| INT-001 | *(target system)* | *(Power Automate / HTTP / Dataverse)* | *(which topic triggers it)* | *(Read / Write / Both)* | *(OAuth / API Key / Service Account)* | *(Not Started / In Progress / Done)* |

### 2.2 Power Automate Flow Design

Design pattern for agent-triggered flows:

```
Topic (Copilot Studio)
  └─► "Call an action" node
        └─► Input: structured data from slot filling
              └─► Power Automate Cloud Flow
                    ├─► Business logic / API calls
                    ├─► Error handling (try-catch)
                    └─► Output: structured JSON response
                          └─► Topic renders response to user
```

**Flow specification template:**

| Field | Value |
|-------|-------|
| **Flow Name** | *(consistent naming: `Agent-[Action]-[Target]`)* |
| **Trigger** | Copilot Studio (When an agent calls a flow) |
| **Inputs** | *(list each input: name, type, required, description)* |
| **Logic** | *(step-by-step: what the flow does)* |
| **Outputs** | *(list each output: name, type, description)* |
| **Error Handling** | *(what happens on failure — return error message to agent)* |
| **Timeout** | *(max execution time)* |
| **Connections Used** | *(list connectors: Dataverse, SharePoint, HTTP, etc.)* |

### 2.3 Custom Connectors (OpenAPI / Swagger)

When Power Automate standard connectors are insufficient:

| Field | Value |
|-------|-------|
| **Connector Name** | *(descriptive name)* |
| **Base URL** | *(API endpoint)* |
| **Auth Type** | *(OAuth 2.0 / API Key / Basic)* |
| **OpenAPI Spec** | *(file reference or inline)* |
| **Operations** | *(list each operation: method, path, description)* |

### 2.4 Dataverse Operations

Define reads and writes to Dataverse tables:

| Operation ID | Table | Operation | Fields | Filter / Condition | Purpose |
|--------------|-------|-----------|--------|---------------------|---------|
| DV-001 | *(table name)* | *(Read / Create / Update)* | *(which fields)* | *(filter expression)* | *(why)* |

**Best Practice:**
- Keep flows stateless — use Dataverse for persistence, not flow variables
- Always define explicit input/output schemas (no untyped dynamic content)
- Handle flow errors with a fallback message node in the topic
- Log every flow execution for audit and debugging

### 2.5 File Handling

Define how the agent manages file inputs and outputs:

| Scenario | Channel | Method | Storage | Post-Processing |
|----------|---------|--------|---------|-----------------|
| *(e.g., .axtr upload)* | *(Teams/Web)* | *(native upload / Adaptive Card)* | *(SharePoint / Azure Blob / DevOps)* | *(route to team / parse / archive)* |

### 2.6 HTTP Actions (Direct API Calls)

For lightweight integrations that don't justify a full Power Automate flow:

| Action ID | Method | Endpoint | Headers | Body | Expected Response |
|-----------|--------|----------|---------|------|-------------------|
| HTTP-001 | *(GET/POST)* | *(URL)* | *(auth headers)* | *(JSON body)* | *(expected shape)* |

---

## 3. Key Decision Points

| # | Decision | Impact | Status |
|---|----------|--------|--------|
| 1 | What can the agent do autonomously vs. what must be routed to a human? | Defines automation boundary | Open |
| 2 | Power Automate vs. direct HTTP — threshold? | Architecture simplicity vs. control | Open |
| 3 | Where are files stored after upload? | SharePoint vs. Azure DevOps vs. Blob | Open |
| 4 | What Dataverse tables does the agent need access to? | Security and data scope | Open |
| 5 | Error handling strategy: silent retry or inform user? | User experience | Open |

---

## 4. Inputs Required

- [ ] List of actions the agent must perform (from Skill 01)
- [ ] Target system APIs and documentation
- [ ] Dataverse table/field metadata (from D365-KB MCP)
- [ ] Object dependencies (from D365-xref MCP)
- [ ] Authentication credentials and connection setup
- [ ] File storage decision

---

## 5. Outputs / Deliverables

| Deliverable | Format |
|-------------|--------|
| Integration Inventory | Markdown table |
| Power Automate Flow Specs | Markdown per flow |
| OpenAPI Definition (if applicable) | JSON file |
| Dataverse Field Mapping | Markdown table |
| File Handling Matrix | Markdown table |

---

## 6. Progress Tracker

- [ ] Complete Integration Inventory
- [ ] Design Power Automate flow specs (one per action)
- [ ] Define Dataverse read/write operations
- [ ] Define file handling flows
- [ ] Build custom connectors (if needed)
- [ ] Build and test each Power Automate flow
- [ ] End-to-end test: topic → flow → response
- [ ] Error handling validation
- [ ] Stakeholder sign-off
