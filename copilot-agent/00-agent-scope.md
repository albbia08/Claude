# Copilot Studio Agent — Scope Definition

**Version:** 1.0
**Status:** Draft
**Last Updated:** 2026-04-03

---

## 1. Purpose

This document is the master reference for designing, configuring, and deploying an intelligent agent in **Microsoft Copilot Studio**. It defines skills, tools, deliverables, and project-specific context. The framework is generic and reusable across multiple agent projects.

---

## 2. What We Are Building

A Copilot Studio agent that acts as an intelligent conversational interface between end users and one or more backend systems. The agent must:

- Collect structured information from the user before taking any action (Slot Filling)
- Search a Knowledge Base and attempt autonomous resolution (RAG)
- Execute actions via Power Automate or direct API calls when resolution requires it
- Escalate to a human team when the agent cannot resolve the request, with all context preserved

---

## 3. Skills Index

Each skill is documented in its own file under `skills/`.

| # | Skill | File | Status |
|---|-------|------|--------|
| 1 | Conversation Design | `skills/01-conversation-design.md` | Not Started |
| 2 | System Prompt Engineering | `skills/02-system-prompt-engineering.md` | Not Started |
| 3 | Knowledge Base Configuration | `skills/03-knowledge-base-configuration.md` | Not Started |
| 4 | Action & Integration Design | `skills/04-action-integration-design.md` | Not Started |
| 5 | Authentication & Security | `skills/05-auth-security-configuration.md` | Not Started |
| 6 | Channel Deployment | `skills/06-channel-deployment.md` | Not Started |
| 7 | Testing & Quality Assurance | `skills/07-testing-quality-assurance.md` | Not Started |

---

## 4. Available Tools (MCPs)

These are discovery and integration tools — they support the skills above, they are not skills themselves.

| MCP | Purpose |
|-----|---------|
| **D365-KB** (full database) | Query table and field metadata for scoping questions and Dataverse integrations |
| **D365-xref** (object references) | Understand object dependencies to scope what the agent can safely read or write |
| **D365sec** (security) | Map user roles to agent behavior — determine data/action access per role |

---

## 5. Deliverables

| Deliverable | Format | Owner | Status |
|-------------|--------|-------|--------|
| System Prompt | Markdown (`.md`) | Conversation Designer | Not Started |
| Topic Map | Diagram or Copilot Studio export | Conversation Designer | Not Started |
| Knowledge Base Index | Document list with source, owner, update frequency | Project Lead | Not Started |
| Power Automate Flow Specs | Markdown or Flow export | Integration Designer | Not Started |
| OpenAPI Definition | JSON (if custom endpoints needed) | Integration Designer | Not Started |
| Dataverse Field Mapping | Markdown table | Integration Designer | Not Started |
| Security & Auth Config | Markdown | Security Lead | Not Started |
| Test Plan & Results | Markdown | QA | Not Started |

---

## 6. Project Instances

| Project | Context File | Status |
|---------|-------------|--------|
| L1 D365 Support Agent | `L1-d365-support/project-context.md` | In Definition |

---

## 7. Environment Strategy

| Environment | Purpose | Promotion Path |
|-------------|---------|----------------|
| **Development** | Build and iterate on agent configuration, topics, flows | Dev → Test |
| **Test / UAT** | End-to-end validation with real users and test data | Test → Production |
| **Production** | Live agent serving end users | — |

- Agents are packaged as **Managed Solutions** in Power Platform for promotion between environments
- Each promotion must pass the quality gate defined in Skill 07 (Testing & QA)
- Solution exports should be stored in Azure DevOps for version control

---

## 8. ALM & Version Control

| Artifact | Version Control Method | Storage |
|----------|----------------------|---------|
| Copilot Studio agent (topics, settings) | Power Platform Solution export | Azure DevOps repo |
| Power Automate flows | Solution export + Flow documentation | Azure DevOps repo |
| Knowledge Base documents | Document versioning (SharePoint) | SharePoint |
| System Prompt | Markdown file with revision log | Azure DevOps repo |
| OpenAPI definitions | JSON files | Azure DevOps repo |

**Promotion process:** Export solution from Dev → Import to Test → Run QA → Export from Test → Import to Production

---

## 9. Open Questions (Framework-Level)

These are generic questions that apply to any Copilot Studio agent project. Project-specific open questions are tracked in each project's `project-context.md` file.

| # | Question | Owner | Status |
|---|----------|-------|--------|
| 1 | What is the standard naming convention for agent solutions in Power Platform? | Power Platform Admin | Open |
| 2 | What DLP policies apply to Copilot Studio connectors in the organization? | Power Platform Admin | Open |
| 3 | What is the approval process for promoting solutions from Test to Production? | IT Lead | Open |
| 4 | Is there a multi-language strategy for agents serving global entities? | Project Lead | Open |
