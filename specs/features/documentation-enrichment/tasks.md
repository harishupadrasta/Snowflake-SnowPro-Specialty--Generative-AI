---
status: draft
created: 2026-08-03
modified: 2026-08-03
---

# Implementation Tasks

## Status Legend
- [ ] Not started
- [~] In progress
- [x] Complete

---

## Phase 1: Accuracy Fixes (P0)

### Task 1.1: Fix Deprecated References
- [ ] `2.8a_REST_API_and_Fine_Tuning.md` — Check EXTRACT_ANSWER and EMBED_TEXT_768/1024 references, ensure marked as deprecated/legacy
- [ ] `4.1_Document_Parsing_Functions.md` — Ensure !PREDICT is clearly marked as decommissioned (March 2026)
- [ ] `4_Snowflake_Document_AI/README.md` — Update !PREDICT reference in decision tree
- [ ] `4_Snowflake_Document_AI/Scenarios_Domain4.md` — Update any !PREDICT scenarios to use AI_EXTRACT

### Task 1.2: Validate Remaining Model Names
- [ ] Search for any remaining deprecated model names across all files
- [ ] Ensure model lists reflect current availability (Aug 2026)
- [ ] Verify model context windows are current

---

## Phase 2: Content Enrichment (P0)

### Task 2.1: Expand `1.5_Snowflake_Intelligence.md` (76 → 150+ lines)
Add:
- [ ] CoWork vs Cortex Code comparison table
- [ ] Supported query types (structured data, unstructured, mixed)
- [ ] Custom instructions feature (2000 chars, session-persistent)
- [ ] @auto-complete for schema references
- [ ] Language support (11 languages)
- [ ] Limitations (schema metadata limit ~10 tables, no cross-database, no data value access)
- [ ] SNOWFLAKE_INTELLIGENCE_ADMIN role for creating agents in CoWork
- [ ] Deep Research feature details

### Task 2.2: Expand `1.6_Cortex_Code.md` (49 → 150+ lines)
Add:
- [ ] Three deployment surfaces (Snowsight, Desktop CLI, Workspaces)
- [ ] AI code suggestions (Tab to accept, GA July 2026)
- [ ] Skills system (SKILL.md files, bundled vs custom)
- [ ] Plugins system (MCP integration)
- [ ] Subagents (explore, generalPurpose, plan types)
- [ ] Hooks system (PreToolUse, PostToolUse)
- [ ] Billing types per surface (CORTEX_CODE_SNOWSIGHT, CORTEX_CODE_DESKTOP, CORTEX_CODE_WORKSPACE)
- [ ] Cross-region requirement for non-supported regions

### Task 2.3: Expand `2.6_Chat_Interfaces_and_Streamlit.md` (89 → 150+ lines)
Add:
- [ ] Complete Streamlit chat pattern with session_state
- [ ] RAG chatbot code example (Search + AI_COMPLETE + Streamlit)
- [ ] Privilege triad: USE AI FUNCTIONS + CORTEX_USER + USAGE on warehouse
- [ ] Streamlit in Snowflake vs local Streamlit differences
- [ ] Container-based Streamlit (SiS on SPCS)

### Task 2.4: Expand `2.7_Cortex_in_Data_Pipelines.md` (98 → 150+ lines)
Add:
- [ ] Dynamic Table + AI function pattern (with TARGET_LAG considerations)
- [ ] Stream + Task pattern for incremental AI processing
- [ ] Error handling pattern (TRY_COMPLETE, CASE on NULL)
- [ ] Cost optimization: batch processing vs row-by-row
- [ ] Pipeline monitoring via CORTEX_FUNCTIONS_USAGE_HISTORY

### Task 2.5: Expand `2.9_Performance_Considerations.md` (91 → 150+ lines)
Add:
- [ ] Model comparison matrix: model × speed × quality × cost × context window
- [ ] Warehouse sizing recommendation (MEDIUM max)
- [ ] Batching strategies (LIMIT, pagination, parallel queries)
- [ ] REST API vs SQL latency comparison
- [ ] Prompt optimization (shorter prompts = less cost)
- [ ] Cross-region latency implications

### Task 2.6: Expand `3.1_Model_Access_Controls.md` (81 → 150+ lines)
Add:
- [ ] CORTEX_MODELS_ALLOWLIST syntax and examples
- [ ] Interaction between allowlist and RBAC (OR relationship)
- [ ] Account-level vs session-level parameter settings
- [ ] How to revoke access (revoke database role from PUBLIC)
- [ ] Monitoring who used what (CORTEX_AI_FUNCTIONS_USAGE_HISTORY view)

### Task 2.7: Expand `4.4_Troubleshooting_and_Optimization.md` (88 → 150+ lines)
Add:
- [ ] Common AI_PARSE_DOCUMENT error messages and fixes
- [ ] Common AI_EXTRACT error messages (page limit, file size, format)
- [ ] scale_factor parameter for OCR quality improvement
- [ ] Batch processing strategies (DIRECTORY() + LATERAL)
- [ ] Cost optimization (page_filter to process only needed pages)
- [ ] Pipeline error handling (TRY/CATCH patterns)

---

## Phase 3: Missing Topics (P1)

### Task 3.1: Add AI_EXTRACT Advanced Features
- [ ] Table extraction with column_ordering and nested schemas
- [ ] List extraction format
- [ ] scores => TRUE parameter (confidence scoring)
- [ ] scale_factor config parameter
- [ ] Multi-format extraction in single call (entity + list + table)
- [ ] AI_EXTRACT with fine-tuned models (model => parameter)

### Task 3.2: Add AI_PARSE_DOCUMENT Advanced Features
- [ ] Image extraction (extract_images: true, LAYOUT mode required)
- [ ] page_filter option for selective page processing
- [ ] Multilingual support (15 languages)
- [ ] Integration with AI_COMPLETE for multimodal analysis
- [ ] Storing extracted images to stages

### Task 3.3: Add Cortex Search Advanced Features
- [ ] Multi-index Cortex Search (multiple vector columns)
- [ ] Batch search queries (CORTEX_SEARCH_BATCH_QUERY_USAGE_HISTORY)
- [ ] Primary keys for optimized refresh
- [ ] Auto-suspend serving (AUTO_SUSPEND parameter)

### Task 3.4: Add Cost Management Deep Dive
- [ ] CORTEX_AI_FUNCTIONS_USAGE_HISTORY view (5-min latency)
- [ ] Per-user spending limits (REVOKE from PUBLIC pattern)
- [ ] Runaway query detection and cancellation
- [ ] Account-level spending alerts (Alert + Email integration)
- [ ] Query tags for cost attribution

---

## Phase 4: Quiz & Scenarios Expansion (P2)

### Task 4.1: Add 10 New Questions to Domain 1
- [ ] CoWork Deep Research feature
- [ ] MCP transport types (stdio, http, sse)
- [ ] CKE from Marketplace
- [ ] SNOWFLAKE_INTELLIGENCE_ADMIN role
- [ ] Cortex Code billing types per surface

### Task 4.2: Add 15 New Questions to Domain 2
- [ ] AI_EXTRACT table extraction format
- [ ] AI_EXTRACT scores parameter
- [ ] AI_PARSE_DOCUMENT image extraction
- [ ] AI_PARSE_DOCUMENT page_filter
- [ ] Fine-tuning supported models (current list)
- [ ] REST API prompt caching differences (OpenAI vs Anthropic)
- [ ] Warehouse sizing for AI functions

### Task 4.3: Add 10 New Questions to Domain 3
- [ ] Per-user spending limits pattern
- [ ] CORTEX_AI_FUNCTIONS_USAGE_HISTORY vs CORTEX_FUNCTIONS_USAGE_HISTORY
- [ ] Runaway query cancellation
- [ ] Cost attribution with query tags
- [ ] Cortex Guard billing (additional to AI_COMPLETE)

### Task 4.4: Add 15 New Questions to Domain 4
- [ ] Document AI decommission date and migration path
- [ ] AI_EXTRACT vs old !PREDICT method
- [ ] scale_factor parameter effect on tokens/pages
- [ ] page_filter syntax
- [ ] Image extraction limitations (50 images max)
- [ ] Supported file formats (current list)

### Task 4.5: Add Navigation Footer to Quiz/Scenario Files
- [ ] Add footer to all 8 quiz/scenario files (Questions_Domain1-4, Scenarios_Domain1-4)

---

## Phase 5: Final Validation (P2)

### Task 5.1: Run Full Validation Checklist
- [ ] grep for all deprecated terms (from requirements.md checklist)
- [ ] Verify all files > 100 lines
- [ ] Verify all navigation footers present
- [ ] Cross-reference quiz answers against notes
- [ ] Verify GLOSSARY covers all Key Terms boxes

### Task 5.2: Consistency Review
- [ ] All files use same heading structure
- [ ] All SQL examples use consistent formatting
- [ ] All Mermaid diagrams render correctly
- [ ] No broken links between files
