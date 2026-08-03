---
status: approved
created: 2026-08-03
modified: 2026-08-03
author: Harish Kumar Upadrasta
project: SnowPro Specialty Gen AI (GES-C02) Study Guide
---

# Documentation Enrichment & Validation Specification

## Project Audit Summary

### Current State (August 3, 2026)

| Metric | Count |
|--------|-------|
| Total markdown files | 46 |
| Domain 1 files | 15 (12 topics + README + Scenarios + Quiz) |
| Domain 2 files | 13 (10 topics + README + Scenarios + Quiz) |
| Domain 3 files | 8 (5 topics + README + Scenarios + Quiz) |
| Domain 4 files | 7 (4 topics + README + Scenarios + Quiz) |
| Root files | 3 (README, GLOSSARY, CONTRIBUTING) |
| Quiz questions total | ~200 (50+60+50+40) |
| Scenarios total | ~38 across 4 guides |

### Files Under 100 Lines (Potentially Thin)

| File | Lines | Assessment |
|------|-------|-----------|
| `1.5_Snowflake_Intelligence.md` | 76 | ⚠️ Thin — needs CoWork features, limitations, comparison |
| `1.6_Cortex_Code.md` | 49 | ⚠️ Very thin — needs features, usage patterns, billing |
| `2.6_Chat_Interfaces_and_Streamlit.md` | 89 | ⚠️ Thin — needs Streamlit code patterns, session_state |
| `2.7_Cortex_in_Data_Pipelines.md` | 98 | ⚠️ Thin — needs Dynamic Tables + AI functions examples |
| `2.9_Performance_Considerations.md` | 91 | ⚠️ Thin — needs model comparison matrix, latency data |
| `3.1_Model_Access_Controls.md` | 81 | ⚠️ Thin — needs allowlist syntax, revocation patterns |
| `4.4_Troubleshooting_and_Optimization.md` | 88 | ⚠️ Thin — needs more error patterns, optimization tips |
| `2.8_Third_Party_Models_SPCS.md` | 104 | Borderline — could use more deployment examples |

---

## REQ-1: Accuracy Validation

### REQ-1.1: Cross-Reference All Quiz Questions Against Notes

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL ensure every quiz question answer can be found in the corresponding domain notes
SO THAT students never encounter exam content not covered in the study material.

**Acceptance Criteria**:
- [ ] Every Q in Questions_Domain1.md is answerable from Domain 1 notes
- [ ] Every Q in Questions_Domain2.md is answerable from Domain 2 notes
- [ ] Every Q in Questions_Domain3.md is answerable from Domain 3 notes
- [ ] Every Q in Questions_Domain4.md is answerable from Domain 4 notes
- [ ] No quiz answer references a concept not explained in notes

### REQ-1.2: Validate Against Official Documentation

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL verify all technical claims against official Snowflake documentation (docs.snowflake.com)
SO THAT no outdated or incorrect information misleads exam candidates.

**Acceptance Criteria**:
- [ ] All function signatures match current docs (AI_COMPLETE, AI_EXTRACT, AI_EMBED, etc.)
- [ ] All model names are current (no deprecated: reka-core, llama2-*, gpt-4o, snowflake-arctic for generation)
- [ ] Document AI decommission (March 2026) is reflected everywhere
- [ ] "Snowflake Intelligence" replaced with "CoWork" in all product descriptions (SQL DDL names unchanged)
- [ ] Billing rates and methods match Snowflake Service Consumption Table
- [ ] RBAC roles are accurately described (CORTEX_USER, CORTEX_AGENT_USER, COPILOT_USER, etc.)
- [ ] Limits are current (AI_PARSE_DOCUMENT: 2000 pages/100MB, AI_EXTRACT: 125 pages/100MB)

### REQ-1.3: Remove Orphaned/Duplicate Content

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL contain no orphaned files (not in TOC), no duplicate files covering the same topic, and no dead internal links
SO THAT students have a single source of truth for each concept.

**Acceptance Criteria**:
- [x] Removed: 1.2_Snowflake_Cortex_AI_Features.md (duplicate of 2.2)
- [x] Removed: 1.3_Vector_Data_Types_Operations.md (duplicate of 2.3)
- [x] Removed: 2.1_Cortex_LLM_Functions.md (old version of 2.1)
- [x] Removed: 2.2_Cortex_LLM_Complete_Functions.md (old version of 2.2)
- [x] Removed: 4.1_Document_AI_Overview.md (deprecated API patterns)
- [ ] All README TOC links verified against actual files on disk
- [ ] No broken relative links in navigation footers

---

## REQ-2: Content Enrichment

### REQ-2.1: Expand Thin Files to Minimum 150 Lines

**Type**: Event-Driven

**Statement**:
WHEN a topic file has fewer than 100 lines
THE SYSTEM SHALL expand it to at least 150 lines with additional explanations, SQL examples, exam tips, and diagrams
SO THAT every topic provides sufficient depth for exam preparation.

**Acceptance Criteria**:
- [ ] `1.5_Snowflake_Intelligence.md` expanded: add CoWork limitations, comparison with Cortex Code, supported queries, custom instructions
- [ ] `1.6_Cortex_Code.md` expanded: add all deployment surfaces, keybindings, AI suggestions, @auto-complete, billing types
- [ ] `2.6_Chat_Interfaces_and_Streamlit.md` expanded: add session_state for chat history, RAG chatbot pattern, privilege requirements
- [ ] `2.7_Cortex_in_Data_Pipelines.md` expanded: add Dynamic Table + AI function pattern, Streams+Tasks pattern, error handling
- [ ] `2.9_Performance_Considerations.md` expanded: add model comparison matrix (speed/quality/cost), batching strategies, warehouse sizing
- [ ] `3.1_Model_Access_Controls.md` expanded: add CORTEX_MODELS_ALLOWLIST syntax, revocation examples, interaction with cross-region
- [ ] `4.4_Troubleshooting_and_Optimization.md` expanded: add common error codes, retry patterns, batch processing strategies

### REQ-2.2: Add Missing Exam Topics

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL cover 100% of the official exam objectives from the Snowflake certification guide
SO THAT no exam topic is left unaddressed.

**Acceptance Criteria**:
- [ ] Cortex Fine-Tuning covered in depth (supported models, training format, CORTEX_FINE_TUNING_USAGE_HISTORY)
- [ ] Snowflake Notebooks + AI functions integration documented
- [ ] AI_EXTRACT table extraction format (column_ordering, nested schemas) documented
- [ ] AI_EXTRACT scores parameter documented
- [ ] AI_PARSE_DOCUMENT image extraction (extract_images: true) documented
- [ ] AI_PARSE_DOCUMENT page_filter option documented
- [ ] Cortex Search multi-index feature documented
- [ ] Cortex Search batch search documented
- [ ] Cortex Agent evaluations (GPA framework) documented with examples
- [ ] CoWork Deep Research feature documented
- [ ] Provisioned throughput vs serverless documented

### REQ-2.3: Increase Quiz Questions to 250+

**Type**: Optional

**Statement**:
WHEN the exam has 60 questions across 4 domains
THE SYSTEM SHALL provide at least 250 practice questions (4x coverage)
SO THAT students have sufficient practice material for all topic variations.

**Acceptance Criteria**:
- [ ] Domain 1: Increase from 50 to 60 questions (add CoWork, MCP, CKE questions)
- [ ] Domain 2: Increase from 60 to 75 questions (add AI_EXTRACT table format, fine-tuning, REST API)
- [ ] Domain 3: Increase from 50 to 60 questions (add cost management, per-user limits, observability)
- [ ] Domain 4: Increase from 40 to 55 questions (add Document AI decommission, AI_EXTRACT migration, pipeline patterns)
- [ ] Total: 250 questions minimum

---

## REQ-3: Structural Quality

### REQ-3.1: Consistent File Structure

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL maintain consistent structure across all topic files
SO THAT students can navigate predictably and find information quickly.

**Required Structure per Topic File**:
1. Title with exam domain/objective reference
2. Key Terms table (blockquote format with mini-table)
3. Concept explanation with "What is X?"
4. SQL examples (copy-paste ready)
5. Architecture diagram (Mermaid where applicable)
6. Exam Tips section (numbered, specific)
7. References section (links to official docs)
8. Navigation footer (← Previous | 🏠 Home | Next →)

**Acceptance Criteria**:
- [ ] All 31 topic files have Key Terms box
- [ ] All 31 topic files have at least one SQL example
- [ ] All 31 topic files have Exam Tips section
- [ ] All 31 topic files have navigation footer
- [ ] At least 15 files have Mermaid diagrams

### REQ-3.2: Glossary Completeness

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL define every technical term used in the documentation within the GLOSSARY.md
SO THAT students can look up any unfamiliar keyword in one place.

**Acceptance Criteria**:
- [ ] Every term in Key Terms boxes is in GLOSSARY.md
- [ ] GLOSSARY.md covers all terms from quiz answer explanations
- [ ] No undefined acronyms in any file (first use should be expanded)

---

## REQ-4: Exam Readiness Validation

### REQ-4.1: Domain Weight Coverage

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL allocate content depth proportional to exam domain weights
SO THAT students spend appropriate time on high-weight domains.

**Expected Content Distribution**:
| Domain | Weight | Expected Depth | Current State | Gap? |
|--------|--------|----------------|---------------|------|
| D1 (Overview) | 18% | Breadth — know all products | 12 files, 2,670 lines | ✅ Good |
| D2 (Functions) | 38% | Deep — know every function | 10 files, 1,590 lines | ⚠️ Need more depth on thin files |
| D3 (Governance) | 29% | Medium — RBAC, cost, observability | 5 files, 666 lines | ⚠️ Thin — expand all files |
| D4 (Document) | 15% | Focused — parsing, pipelines | 4 files, 564 lines | ⚠️ Update for Document AI decommission |

### REQ-4.2: Scenario-Based Learning

**Type**: Ubiquitous

**Statement**:
THE SYSTEM SHALL include decision scenarios that mirror exam question patterns (multi-option, "which product/function/role")
SO THAT students develop the decision-making skill needed for the exam.

**Acceptance Criteria**:
- [ ] Each scenario file has at least 10 scenarios
- [ ] Scenarios cover "which function to use" decisions
- [ ] Scenarios cover "which role is needed" decisions
- [ ] Scenarios cover "what's the cost implication" decisions
- [ ] Scenarios cover "what's wrong with this approach" troubleshooting

---

## Implementation Priority

| Priority | Requirement | Impact | Effort |
|----------|-------------|--------|--------|
| 🔴 P0 | REQ-1.2 — Validate accuracy against docs | Critical — wrong info fails students | High |
| 🔴 P0 | REQ-2.1 — Expand thin files | High — thin files = knowledge gaps | High |
| 🟡 P1 | REQ-2.2 — Add missing topics | High — exam coverage | Medium |
| 🟡 P1 | REQ-1.1 — Cross-reference quiz vs notes | Medium — ensures consistency | Medium |
| 🟢 P2 | REQ-2.3 — Add more quiz questions | Medium — more practice | Medium |
| 🟢 P2 | REQ-3.1 — Structural consistency | Low — cosmetic but improves UX | Low |
| 🟢 P2 | REQ-3.2 — Glossary completeness | Low — reference quality | Low |

---

## Validation Checklist

Run these checks after implementation:

- [ ] `grep -r "Snowflake Intelligence" --include="*.md"` returns zero results (except SQL DDL names and historical references)
- [ ] `grep -r "CHAT()" --include="*.md"` returns zero results (non-existent function)
- [ ] `grep -r "gpt-4o" --include="*.md"` returns zero results (wrong model name)
- [ ] `grep -r "reka-core" --include="*.md"` returns zero results (removed model)
- [ ] `grep -r "llama2-" --include="*.md"` returns zero results (deprecated models)
- [ ] `grep -r "EXTRACT_ANSWER" --include="*.md"` returns zero or only marks it as deprecated
- [ ] `grep -r "EMBED_TEXT_768\|EMBED_TEXT_1024" --include="*.md"` returns zero or only marks as legacy
- [ ] `grep -r "!PREDICT" --include="*.md"` clearly marks as decommissioned
- [ ] All files > 100 lines (or have explicit justification for being shorter)
- [ ] Every quiz question has a 4-option multiple choice format with explanation
- [ ] Every file has a navigation footer
