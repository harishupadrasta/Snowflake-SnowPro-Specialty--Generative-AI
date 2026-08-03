# Domain 4: Scenarios and Decision Guide

> **Document processing scenarios** — When to parse, extract, or build pipelines

---

## Scenario 1: "Parse vs Extract vs Complete — Which to use?"

```
┌─────────────────────────────────────────────────────────────────┐
│  DOCUMENT PROCESSING DECISION TREE                               │
│                                                                   │
│  What do you need from the document?                              │
│  │                                                                │
│  ├── Full text content (for search/RAG)?                          │
│  │   └── AI_PARSE_DOCUMENT (OCR or LAYOUT mode)                   │
│  │                                                                │
│  ├── Specific structured fields (vendor, amount, date)?           │
│  │   └── AI_EXTRACT (with schema definition)                      │
│  │                                                                │
│  ├── Answer questions ABOUT the document?                         │
│  │   └── AI_COMPLETE with TO_FILE (vision/multimodal)             │
│  │                                                                │
│  ├── Classify document type?                                      │
│  │   └── AI_CLASSIFY with TO_FILE                                 │
│  │                                                                │
│  └── Visual pipeline (Snowsight UI) with training?                │
│      └── ⚠️ Document AI (!PREDICT) — DECOMMISSIONED March 2026  │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Scenario 2: "Process 10,000 invoices into a structured table"

**Step 1: Parse documents**
```sql
CREATE TABLE parsed_invoices AS
SELECT 
    RELATIVE_PATH,
    AI_PARSE_DOCUMENT(TO_FILE('@invoice_stage', RELATIVE_PATH), {'mode': 'LAYOUT'}):content AS text
FROM DIRECTORY(@invoice_stage)
WHERE RELATIVE_PATH LIKE '%.pdf';
```

**Step 2: Extract fields**
```sql
CREATE TABLE invoice_data AS
SELECT 
    RELATIVE_PATH,
    AI_EXTRACT(
        TO_FILE('@invoice_stage', RELATIVE_PATH),
        {'vendor': 'string', 'invoice_number': 'string', 'total': 'number', 'date': 'string'}
    ) AS fields
FROM DIRECTORY(@invoice_stage)
WHERE RELATIVE_PATH LIKE '%.pdf';
```

**Step 3: Flatten to relational**
```sql
CREATE TABLE invoices_final AS
SELECT 
    RELATIVE_PATH AS source_file,
    fields:vendor::STRING AS vendor,
    fields:invoice_number::STRING AS invoice_number,
    fields:total::NUMBER AS total_amount,
    fields:date::STRING AS invoice_date
FROM invoice_data;
```

---

## Scenario 3: "OCR mode vs LAYOUT mode — when?"

| Scenario | Mode | Why |
|----------|------|-----|
| Simple text extraction (letters, emails) | **OCR** | Faster, cheaper, text is enough |
| Documents with tables (financial reports) | **LAYOUT** | Preserves table structure |
| Forms with fields (applications, surveys) | **LAYOUT** | Maintains field-value relationships |
| Images of text (scanned documents) | **OCR** | Sufficient for plain text |
| Technical docs with headers/lists | **LAYOUT** | Preserves hierarchy |
| Quick bulk processing (cost-sensitive) | **OCR** | ~2x faster, same token cost |

**Cost:** Both modes bill at **970 tokens per page**. The difference is speed and output structure, not cost.

---

## Scenario 4: "Documents are failing — troubleshoot"

| Error | Cause | Fix |
|-------|-------|-----|
| `invalid image path` | File not found or unsupported format | Check path (CASE-SENSITIVE), verify extension |
| `Error in secure object` | Stage doesn't exist | Verify stage name, use `@` prefix |
| `unsupported image format` | Wrong format for model | Use JPEG/PNG/WEBP/GIF |
| `Image data exceeds limit` | File too large | Reduce to <10MB |
| Query hangs | Very large document | Use `page_limit` to cap pages |
| NULL returned | Error in processing | Use `return_error_details => TRUE` |

**Stage compatibility check:**
```sql
-- ✓ Works
CREATE STAGE good_stage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE');

-- ✗ Does NOT work
CREATE STAGE bad_stage ENCRYPTION = (TYPE = 'SNOWFLAKE_FULL');  -- client-side!
```

---

## Scenario 5: "Continuous document processing as files arrive"

**Pattern: Stream on Stage + Task**

```sql
-- 1. Enable directory auto-refresh for external stage
CREATE STAGE doc_stage URL='s3://...' DIRECTORY=(ENABLE=TRUE) AUTO_REFRESH=TRUE;

-- 2. Create stream detecting new files
CREATE STREAM new_docs_stream ON STAGE doc_stage;

-- 3. Create processing task
CREATE TASK process_docs
  WAREHOUSE = doc_wh
  SCHEDULE = '5 MINUTE'
  WHEN SYSTEM$STREAM_HAS_DATA('new_docs_stream')
AS
INSERT INTO processed_docs
SELECT
    RELATIVE_PATH,
    AI_PARSE_DOCUMENT(TO_FILE('@doc_stage', RELATIVE_PATH), {'mode': 'LAYOUT'}):content,
    AI_CLASSIFY(
        AI_PARSE_DOCUMENT(TO_FILE('@doc_stage', RELATIVE_PATH), {'mode': 'OCR'}):content,
        ['Invoice', 'Contract', 'Report']
    ),
    CURRENT_TIMESTAMP()
FROM new_docs_stream
WHERE METADATA$ACTION = 'INSERT';

ALTER TASK process_docs RESUME;
```

---

## Scenario 6: "Document AI pipeline vs AI_PARSE_DOCUMENT"

| Feature | Document AI (!PREDICT) ⚠️ DECOMMISSIONED | AI_PARSE_DOCUMENT |
|---------|----------------------|-------------------|
| **Interface** | Snowsight visual UI | SQL function |
| **Training** | Upload samples, train model | No training needed |
| **Role required** | DOCUMENT_INTELLIGENCE_CREATOR | CORTEX_USER |
| **Schema privilege** | CREATE SNOWFLAKE.ML.DOCUMENT_INTELLIGENCE | None |
| **Output** | Custom fields you defined | Text/markdown |
| **Best for** | Repeated extraction of same doc type | Ad-hoc parsing |
| **Fine-tuning** | Built into the pipeline | Separate (arctic-extract) |
| **SQL privilege** | + CREATE MODEL (since March 2025) | Standard AI function access |

**Document AI privileges:**
```sql
GRANT DATABASE ROLE SNOWFLAKE.DOCUMENT_INTELLIGENCE_CREATOR TO ROLE doc_role;
GRANT CREATE SNOWFLAKE.ML.DOCUMENT_INTELLIGENCE ON SCHEMA db.schema TO ROLE doc_role;
GRANT CREATE MODEL ON SCHEMA db.schema TO ROLE doc_role;
```

---

## Scenario 7: "Build full RAG pipeline over company documents"

**Complete architecture:**
```
┌──────────────────────────────────────────────────────────────┐
│                 DOCUMENT RAG PIPELINE                          │
│                                                                │
│  @doc_stage (PDFs, DOCX, etc.)                                │
│       │                                                        │
│       ▼  AI_PARSE_DOCUMENT('LAYOUT')                          │
│  ┌──────────────┐                                             │
│  │ Full text    │                                             │
│  │ (markdown)   │                                             │
│  └──────┬───────┘                                             │
│         │                                                      │
│         ▼  SPLIT_TEXT_RECURSIVE_CHARACTER(512 tokens, 50 overlap)
│  ┌──────────────┐                                             │
│  │ Text chunks  │                                             │
│  │ (array)      │                                             │
│  └──────┬───────┘                                             │
│         │                                                      │
│         ▼  CREATE CORTEX SEARCH SERVICE                        │
│  ┌──────────────────────────────────────┐                     │
│  │ Cortex Search (auto-embeds,          │                     │
│  │ auto-indexes, auto-refreshes)        │                     │
│  └──────────────┬───────────────────────┘                     │
│                 │                                              │
│                 ▼  CREATE AGENT + cortex_search tool            │
│  ┌──────────────────────────────────────┐                     │
│  │ Cortex Agent (orchestrates)          │                     │
│  └──────────────┬───────────────────────┘                     │
│                 │                                              │
│                 ▼  Deploy to CoWork / REST API                  │
│  ┌──────────────────────────────────────┐                     │
│  │ Users ask questions in natural language│                    │
│  └──────────────────────────────────────┘                     │
└──────────────────────────────────────────────────────────────┘
```

**SQL implementation:**
```sql
-- Dynamic table for parsed + chunked content
CREATE DYNAMIC TABLE doc_chunks TARGET_LAG='1 hour' WAREHOUSE=doc_wh AS
WITH parsed AS (
    SELECT RELATIVE_PATH AS source,
        AI_PARSE_DOCUMENT(TO_FILE('@docs', RELATIVE_PATH), {'mode':'LAYOUT'}):content AS content
    FROM DIRECTORY(@docs) WHERE RELATIVE_PATH LIKE '%.pdf'
)
SELECT source, f.INDEX AS chunk_id, f.VALUE::VARCHAR AS chunk
FROM parsed, LATERAL FLATTEN(
    SPLIT_TEXT_RECURSIVE_CHARACTER(content, 'snowflake-arctic-embed-l-v2.0', {'max_tokens':512,'overlap':50})
) f;

-- Search service
CREATE CORTEX SEARCH SERVICE doc_search
  ON chunk ATTRIBUTES source WAREHOUSE=search_wh TARGET_LAG='1 hour'
AS (SELECT chunk, source FROM doc_chunks);

-- Agent
CREATE AGENT doc_assistant FROM SPECIFICATION $$
models:
  orchestration: auto
tools:
  - tool_spec: {type: "cortex_search", name: "docs"}
tool_resources:
  docs: {name: "db.schema.doc_search", max_results: "5"}
$$;
```

---

## Scenario 8: "page_limit and page_split — when to use?"

| Option | Use When |
|--------|----------|
| `page_limit: 5` | Only need first N pages (cover page, TOC, summary) |
| `page_limit: 1` | Processing individual page images |
| `page_split: TRUE` | Need per-page results (page-level citations, chunking) |
| Neither | Process entire document as one block |

```sql
-- Get each page separately for fine-grained chunking
SELECT AI_PARSE_DOCUMENT(
    TO_FILE('@docs', 'report.pdf'),
    {'mode': 'LAYOUT', 'page_split': TRUE}
);
-- Returns ARRAY: [{page_number:1, content:"..."}, {page_number:2, ...}]

-- Cost control: only first 3 pages
SELECT AI_PARSE_DOCUMENT(
    TO_FILE('@docs', 'long_report.pdf'),
    {'mode': 'OCR', 'page_limit': 3}
);
```

---

*Use this for document processing exam scenarios.*

---

<p align="center">
  <a href="./README.md">🏠 Domain Home</a> &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="../README.md">📚 Main Home</a>
</p>
