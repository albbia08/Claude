# Skill 06 — Channel Deployment

**Skill Owner:** Project Lead
**Status:** Not Started
**Dependencies:** Skill 01 (Conversation Design — card support varies by channel)

---

## 1. Purpose

Publish the agent to the correct surface and ensure it works end-to-end in the target channel. Channel choice impacts authentication, UI capabilities, file handling, and user experience — this decision cascades through every other skill.

---

## 2. Scope & Activities

### 2.1 Channel Comparison Matrix

| Capability | Microsoft Teams | Web (Custom Canvas) | SharePoint | Mobile |
|------------|----------------|---------------------|------------|--------|
| **SSO** | Yes (automatic) | Requires config | Yes (inherited) | Depends on wrapper |
| **Adaptive Cards** | Full support | Full support | Limited | Limited |
| **File Upload** | Native | Requires custom config | Not supported | Depends on wrapper |
| **Proactive Messaging** | Supported | Not supported | Not supported | Not supported |
| **Rich Text / Markdown** | Supported | Supported | Limited | Limited |
| **Custom Branding** | Limited (Teams theme) | Full control | SharePoint theme | Full control |
| **Deployment Method** | Teams Admin Center | Embed (iframe / direct link) | Web Part | App wrapper |
| **User Discovery** | Teams App Store / Pinned | URL distribution | SharePoint page | App store |

### 2.2 Channel Selection Criteria

Score each channel against project requirements:

| Criterion | Weight | Teams | Web | SharePoint |
|-----------|--------|-------|-----|------------|
| User base already on this platform | High | *(score)* | *(score)* | *(score)* |
| Supports all required features (file upload, cards) | High | *(score)* | *(score)* | *(score)* |
| Auth complexity | Medium | *(score)* | *(score)* | *(score)* |
| Maintenance overhead | Medium | *(score)* | *(score)* | *(score)* |
| Go-live speed | Low | *(score)* | *(score)* | *(score)* |

### 2.3 Channel-Specific Configuration

#### Microsoft Teams
1. Publish agent from Copilot Studio to Teams
2. Configure Teams App Manifest (name, description, icon)
3. Submit to Teams Admin Center for org-wide deployment or specific group
4. Configure pinning policy (if the agent should appear in the left rail)
5. Test: SSO, Adaptive Cards, file upload, escalation handoff

#### Web (Custom Canvas)
1. Generate embed code from Copilot Studio
2. Configure custom canvas styling (colors, logo, position)
3. Set up OAuth 2.0 authentication
4. Deploy to target website (iframe or direct link)
5. Test: Auth flow, card rendering, file upload (if configured), mobile responsiveness

#### SharePoint
1. Add Copilot Studio Web Part to target SharePoint page
2. Configure web part settings
3. Test: inherited auth, conversation persistence, UI constraints

#### Mobile (Future Consideration)
Mobile deployment is typically achieved by wrapping the Web channel in a mobile app or using Teams mobile. Not a standalone channel in Copilot Studio.
1. Determine wrapper approach: Teams mobile app or custom mobile wrapper
2. Test all conversation flows on mobile form factor
3. Verify: touch-friendly card rendering, file upload capability, auth flow on mobile
4. **Note:** Mobile is out of scope for v1 unless explicitly required — document as a future phase

### 2.4 Pre-Go-Live Channel Checklist

| Check | Teams | Web | SharePoint |
|-------|-------|-----|------------|
| Agent responds to trigger phrases correctly | [ ] | [ ] | [ ] |
| Authentication works (SSO / OAuth) | [ ] | [ ] | [ ] |
| Adaptive Cards render correctly | [ ] | [ ] | [ ] |
| File upload works end-to-end | [ ] | [ ] | [ ] |
| Escalation handoff works | [ ] | [ ] | [ ] |
| Error messages display properly | [ ] | [ ] | [ ] |
| Conversation resets work correctly | [ ] | [ ] | [ ] |
| Analytics are capturing data | [ ] | [ ] | [ ] |

---

## 3. Key Decision Points

| # | Decision | Impact | Status |
|---|----------|--------|--------|
| 1 | Primary channel: Teams, Web, or SharePoint? | Cascades to auth, UI, file handling | Open |
| 2 | Multi-channel or single channel at launch? | Scope and testing effort | Open |
| 3 | Org-wide deployment or phased rollout (pilot group first)? | Risk management | Open |
| 4 | Who approves the Teams app in Admin Center? | Deployment dependency | Open |

---

## 4. Inputs Required

- [ ] User base analysis: where do target users spend their time?
- [ ] Feature requirements list (file upload, cards, proactive messaging)
- [ ] Auth decision from Skill 05
- [ ] Teams Admin Center access and approval process
- [ ] Brand guidelines (if Web channel with custom canvas)

---

## 5. Outputs / Deliverables

| Deliverable | Format |
|-------------|--------|
| Channel Selection Decision (with rationale) | Markdown |
| Channel-Specific Configuration Guide | Markdown |
| Pre-Go-Live Checklist (completed) | Markdown checklist |
| Deployment Runbook | Markdown (step-by-step) |

---

## 6. Progress Tracker

- [ ] Complete Channel Comparison Matrix
- [ ] Score channels against project requirements
- [ ] Select primary channel (stakeholder decision)
- [ ] Complete channel-specific configuration
- [ ] Run Pre-Go-Live Channel Checklist
- [ ] Deploy to pilot group
- [ ] Collect pilot feedback
- [ ] Deploy to full user base
- [ ] Post-deployment monitoring (first 48 hours)
