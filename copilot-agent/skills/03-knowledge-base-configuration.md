# Skill 03 — Knowledge Base Configuration

**Skill Owner:** Project Lead
**Status:** Not Started
**Dependencies:** Skill 01 (Conversation Design — defines when KB is searched), Skill 05 (Auth — defines access control to sources)

---

## 1. Purpose

Set up and manage the documents the agent uses for Generative Answers (RAG). The quality of the agent's responses is directly tied to the quality, structure, and relevance of its Knowledge Base. This skill covers source selection, document preparation, upload, and retrieval quality testing.

---

## 2. Scope & Activities

### 2.1 Knowledge Source Inventory

Document every source the agent will use:

| Source ID | Type | Location | Content Scope | Auth Required | Owner | Refresh Cadence | Status |
|-----------|------|----------|---------------|---------------|-------|-----------------|--------|
| KB-001 | *(PDF/DOCX/SharePoint/Website)* | *(URL or path)* | *(what it covers)* | *(Yes/No)* | *(name)* | *(weekly/monthly/ad hoc)* | *(Active/Draft/Pending)* |

### 2.2 Document Preparation for RAG

Structure documents to maximize retrieval quality:

| Principle | Why It Matters | Action |
|-----------|---------------|--------|
| **Clear headings** | RAG chunks by sections — headings define chunk boundaries | Use H1/H2/H3 hierarchy consistently |
| **Atomic procedures** | One procedure per section, not interleaved | Split multi-step processes into self-contained sections |
| **No ambiguity** | Vague text generates vague answers | Replace "it depends" with explicit conditions |
| **Tables over paragraphs** | Structured data retrieves more accurately | Convert narrative descriptions to tables where possible |
| **Remove duplicates** | Duplicate content causes conflicting answers | Deduplicate across all sources before upload |
| **Include keywords** | Matches user language to document language | Add common synonyms and user-facing terminology |

### 2.3 Copilot Studio Configuration

- Navigate to **Knowledge** section in Copilot Studio
- Add each source (upload file, link SharePoint, or add URL)
- Configure source-level settings:
  - Content moderation level
  - Citation behavior (show source / hide source)
  - Access scope (which topics can use this source)
- Configure **Generative Answers** settings:
  - **Content strictness:** How closely the agent must adhere to KB content vs. general knowledge (recommended: **High** for L1 support to prevent fabrication)
  - **Allow general knowledge:** On / Off — when Off, the agent only answers from KB content
  - **Classic vs. Generative mode:** Generative for natural responses grounded in KB; Classic for exact-match FAQ style

### 2.4 Retrieval Quality Testing

Test that the agent retrieves the right content for real user queries:

| Test ID | User Query | Expected Source | Expected Answer (summary) | Actual Source | Actual Answer | Pass/Fail | Notes |
|---------|------------|-----------------|---------------------------|---------------|---------------|-----------|-------|
| RT-001 | *(real user question)* | *(KB-xxx)* | *(expected)* | *(actual)* | *(actual)* | | |

**Testing protocol:**
1. Write 10–20 representative user queries covering each KB source
2. Run each query in the Test Chat pane
3. Check: Did the agent cite the correct source? Is the answer accurate?
4. For failures: is the issue in the document (bad structure) or in the retrieval (configuration)?
5. Refine documents or configuration and retest

---

## 3. Key Decision Points

| # | Decision | Impact | Status |
|---|----------|--------|--------|
| 1 | What documents does the agent have access to? | Defines answer scope | Open |
| 2 | Who owns each document and who keeps it updated? | Determines freshness risk | Open |
| 3 | Should the agent cite its source in the response? | User trust vs. simplicity | Open |
| 4 | How are documents versioned and refreshed? | Stale KB = wrong answers | Open |
| 5 | Should different topics have access to different KB sources? | Scoping vs. complexity | Open |

---

## 4. Inputs Required

- [ ] Complete list of candidate documents
- [ ] Document owners and update cadence
- [ ] Access permissions for each source
- [ ] Representative user queries for testing (minimum 10)
- [ ] Decision on citation behavior

---

## 5. Outputs / Deliverables

| Deliverable | Format |
|-------------|--------|
| Knowledge Base Index (source inventory) | Markdown table |
| RAG-ready documents | PDF / DOCX (structured per 2.2 guidelines) |
| Retrieval Quality Test Results | Markdown table |
| Refresh & Ownership Matrix | Markdown table |

---

## 6. Progress Tracker

- [ ] Identify all candidate documents
- [ ] Complete Knowledge Source Inventory table
- [ ] Assign document owners and refresh cadence
- [ ] Prepare documents per RAG-friendly guidelines (Section 2.2)
- [ ] Upload documents to Copilot Studio
- [ ] Configure Knowledge Sources
- [ ] Run retrieval quality tests (minimum 10 queries)
- [ ] Resolve retrieval failures
- [ ] Stakeholder sign-off on KB scope
