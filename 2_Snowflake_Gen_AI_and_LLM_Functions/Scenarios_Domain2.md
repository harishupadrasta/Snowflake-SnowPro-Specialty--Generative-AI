# Domain 2: Scenarios and Decision Guide

> **Function selection scenarios** — When to use which AI function and how to configure it

---

## Scenario 1: "Classify 10,000 support tickets into categories"

**Requirements:** Each ticket → one of [Bug, Feature Request, Billing, General]

**Best function:** `AI_CLASSIFY`

```sql
SELECT 
    ticket_id,
    AI_CLASSIFY(description, ['Bug', 'Feature Request', 'Billing', 'General']) AS category
FROM support_tickets;
```

**Why not AI_COMPLETE?** AI_CLASSIFY is purpose-built for single-label classification — cheaper and returns exactly one label. AI_COMPLETE might return explanations or multiple labels.

**Cost trap:** Labels + descriptions count as input tokens **PER ROW**. With 10K rows and 4 labels with descriptions, that's 10K × (label tokens). Keep labels concise.

---

## Scenario 2: "Extract invoice data into a structured table"

**Requirements:** From PDFs, extract vendor, amount, date, line items as JSON.

**Best function:** `AI_EXTRACT`

```sql
SELECT AI_EXTRACT(
    TO_FILE('@invoices', filename),
    {'vendor_name': 'string', 'total_amount': 'number', 'date': 'string', 
     'line_items': 'array'}
) AS extracted
FROM invoice_files;
```

**Why not AI_PARSE_DOCUMENT?** PARSE gives you raw text/markdown. EXTRACT gives you structured JSON fields. Use PARSE first if you need the full text, EXTRACT if you need specific fields.

**Fine-tuning option:** For domain-specific documents, fine-tune `arctic-extract`:
```sql
SELECT SNOWFLAKE.CORTEX.FINETUNE(
    'CREATE', '@db.schema.my_model', 'arctic-extract',
    'snow://dataset/training_ds/versions/v1'
);
-- Then: AI_EXTRACT(model => 'db.schema.my_model', file => ...)
```

---

## Scenario 3: "Determine if reviews mention shipping problems"

**Requirements:** Filter a table to only rows where shipping is mentioned negatively.

**Best function:** `AI_FILTER` (returns boolean — perfect for WHERE)

```sql
SELECT * FROM customer_reviews
WHERE AI_FILTER(review_text, 'Mentions a negative experience with shipping or delivery');
```

**Why not AI_SENTIMENT?** Sentiment gives a score (-1 to 1) for overall tone. It doesn't tell you IF a specific topic (shipping) was mentioned. AI_FILTER answers "does this match the condition?" with TRUE/FALSE.

---

## Scenario 4: "Summarize themes across 50,000 complaints"

**Requirements:** One summary of recurring themes across ALL complaints (not per-row).

**Best function:** `AI_AGG` (aggregate — no context window limit)

```sql
SELECT AI_AGG(
    complaint_text,
    'What are the top 5 recurring complaint themes? Rank by frequency.'
) AS themes
FROM customer_complaints
WHERE created_date > DATEADD(month, -3, CURRENT_DATE());
```

**Why AI_AGG over AI_COMPLETE?** AI_COMPLETE has a context window limit. With 50K rows, you can't fit them all in one prompt. AI_AGG processes in batches automatically — handles millions of rows.

**Per-group analysis:**
```sql
SELECT product_category, AI_AGG(complaint_text, 'Top 3 issues') AS issues
FROM complaints
GROUP BY product_category;
```

---

## Scenario 5: "Need sentiment BY CATEGORY (food quality, service, price)"

**Requirements:** For each review, get sentiment for specific aspects, not just overall.

**Best function:** `AI_SENTIMENT` with entities array

```sql
SELECT AI_SENTIMENT(
    review_text,
    ['food quality', 'service speed', 'price', 'ambiance']
) AS aspect_sentiment
FROM restaurant_reviews;
```

**Returns:**
```json
{
  "categories": [
    {"name": "overall", "sentiment": "mixed"},
    {"name": "food quality", "sentiment": "positive"},
    {"name": "service speed", "sentiment": "negative"},
    {"name": "price", "sentiment": "neutral"},
    {"name": "ambiance", "sentiment": "unknown"}
  ]
}
```

**Key:** Without entities → returns FLOAT (-1 to 1). With entities → returns OBJECT with categories.

---

## Scenario 6: "Generate structured JSON from unstructured text"

**Requirements:** Extract customer name, sentiment categories, and issue type as validated JSON.

**Best approach:** `AI_COMPLETE` with `response_format`

```sql
SELECT AI_COMPLETE(
    model => 'llama3.3-70b',
    prompt => CONCAT('Analyze this customer interaction: ', note_text),
    model_parameters => {'temperature': 0},
    response_format => {
        'type': 'json',
        'schema': {
            'type': 'object',
            'properties': {
                'customer_name': {'type': 'string'},
                'issue_type': {'type': 'string', 'enum': ['billing','technical','general']},
                'sentiment': {'type': 'string', 'enum': ['positive','negative','neutral']},
                'requires_escalation': {'type': 'boolean'}
            },
            'required': ['customer_name', 'issue_type', 'sentiment']
        }
    }
) FROM customer_notes;
```

**Why response_format over just prompting?** Without schema enforcement, the model might return invalid JSON, extra text, or missing fields. `response_format` GUARANTEES schema compliance — every token is validated.

**OpenAI model trap:** For GPT models, you MUST add `additionalProperties: false` to every object in the schema.

---

## Scenario 7: "Process audio call recordings for QA"

**Requirements:** Transcribe + analyze call center recordings.

**Step 1: Transcribe** with `AI_TRANSCRIBE`:
```sql
SELECT AI_TRANSCRIBE(TO_FILE('@calls', recording_path)) AS transcript
FROM call_recordings;
```

**Step 2: Analyze** with `AI_COMPLETE`:
```sql
SELECT AI_COMPLETE(
    'gemini-3.1-pro',
    'Evaluate agent professionalism and customer satisfaction from this call',
    TO_FILE('@calls', recording_path),
    {},
    {'type': 'json', 'schema': {...}}
);
```

**Key:** `gemini-3.1-pro` or `gemini-3.5-flash` are REQUIRED for audio/video. Other models don't support audio input.

**Billing:** Audio = 50 tokens/second. A 30-minute call = 90,000 tokens.

---

## Scenario 8: "Batch process fails on row 5,000 of 100,000"

**Problem:** One malformed row crashes the entire AI_COMPLETE query.

**Solution: Use TRY_COMPLETE or return_error_details:**

```sql
-- Option A: TRY_COMPLETE (returns NULL on error, query continues)
SELECT id, TRY_COMPLETE('llama3.1-70b', text_col) AS result
FROM large_table;

-- Option B: return_error_details (returns {value, error} per row)
SELECT id, 
    AI_COMPLETE('llama3.1-70b', text_col, return_error_details => TRUE) AS result
FROM large_table;
-- result:value = response text (NULL if error)
-- result:error = error message (NULL if success)
```

**Pre-filter long texts:**
```sql
-- Skip rows that would exceed context window
SELECT id, AI_COMPLETE('llama3.1-70b', text_col)
FROM large_table
WHERE AI_COUNT_TOKENS('llama3.1-70b', text_col) < 120000;
```

---

## Scenario 9: "Need embeddings for semantic search"

**Decision: Which embedding model?**

| Need | Model | Dimensions |
|------|-------|-----------|
| English text, standard quality | `snowflake-arctic-embed-m-v1.5` | 768 |
| Multilingual text | `snowflake-arctic-embed-l-v2.0` | 1024 |
| Very long documents (>512 tokens per chunk) | `snowflake-arctic-embed-l-v2.0-8k` | 1024 |
| Images + text (cross-modal) | `voyage-multimodal-3` | 1024 |
| Video search | `twelvelabs-marengo-embed-3-0` | 512 |

**Standard pattern:**
```sql
-- Generate and store embeddings
CREATE TABLE doc_embeddings AS
SELECT 
    doc_id, chunk_text,
    AI_EMBED('snowflake-arctic-embed-l-v2.0', chunk_text) AS embedding
FROM document_chunks;

-- Search by similarity
SELECT doc_id, chunk_text,
    VECTOR_COSINE_SIMILARITY(embedding, 
        AI_EMBED('snowflake-arctic-embed-l-v2.0', 'search query')) AS score
FROM doc_embeddings
ORDER BY score DESC LIMIT 10;
```

**Better approach for production:** Use Cortex Search Service (manages embeddings + index automatically).

---

## Scenario 10: "Remove PII before sharing data"

**Requirements:** Redact names, emails, phone numbers from customer notes before analytics team access.

```sql
CREATE VIEW safe_notes AS
SELECT 
    ticket_id,
    AI_REDACT(agent_notes, ['person_name', 'email_address', 'phone_number', 'ssn']) AS notes
FROM support_tickets;
```

**Result:** "Call John Smith at 555-1234" → "Call [PERSON_NAME] at [PHONE_NUMBER]"

---

## Scenario 11: "REST API vs SQL — which to use?"

| Scenario | Use | Why |
|----------|-----|-----|
| Real-time chatbot (React/mobile app) | **REST API** | Lower latency, streaming, WebSocket-like |
| Batch enrichment (enrich 1M rows) | **SQL (AI_COMPLETE)** | Integrates with tables, GROUP BY, pipelines |
| Streamlit app in Snowflake | **SQL** | get_active_session(), direct SQL execution |
| External Python service | **REST API** | OpenAI SDK compatible, standard HTTP |
| Dynamic table pipeline | **SQL** | Declarative, auto-incremental |
| Fine-tuned model inference | **Both work** | SQL: AI_COMPLETE('my_model',...); REST: model="my_model" |

---

## Scenario 12: "Fine-tune a model for our domain"

**Decision tree:**

```
Is your task domain-specific extraction from documents?
├── YES → Fine-tune arctic-extract (Dataset objects, FILE + Prompt + Response)
└── NO → Fine-tune LLM (prompt + completion columns)
    ├── Need cheapest/fastest? → mistral-7b or llama3.1-8b
    └── Need highest quality? → llama3.1-70b (limited to 4.5K rows @ 3 epochs)
```

**Training data format (LLM fine-tuning):**
```sql
-- MUST have columns named exactly 'prompt' and 'completion'
SELECT question AS prompt, ideal_answer AS completion FROM training_examples;
```

**After fine-tuning:**
```sql
-- Use exactly like any other model
SELECT AI_COMPLETE('my_finetuned_model', 'Your prompt here');
```

**Cross-region limitation:** Fine-tuned models do NOT support cross-region inference. Must use in the same region where trained.

---

## Function Selection Cheat Sheet

```
┌─────────────────────────────────────────────────────────────────┐
│  TASK                        │  FUNCTION              │  OUTPUT  │
├──────────────────────────────┼────────────────────────┼──────────┤
│  Single label classification │  AI_CLASSIFY           │  STRING  │
│  Boolean yes/no condition    │  AI_FILTER             │  BOOLEAN │
│  Sentiment score             │  AI_SENTIMENT (no ent) │  FLOAT   │
│  Aspect-based sentiment      │  AI_SENTIMENT (w/ ent) │  OBJECT  │
│  Extract specific fields     │  AI_EXTRACT            │  JSON    │
│  Multi-row insight           │  AI_AGG                │  STRING  │
│  Multi-row summary           │  AI_SUMMARIZE_AGG      │  STRING  │
│  Single-text summary         │  SUMMARIZE             │  STRING  │
│  Translation                 │  AI_TRANSLATE          │  STRING  │
│  PII removal                 │  AI_REDACT             │  STRING  │
│  Speech to text              │  AI_TRANSCRIBE         │  JSON    │
│  Embeddings (text/image)     │  AI_EMBED              │  VECTOR  │
│  Video embeddings            │  AI_MULTI_EMBED        │  OBJECT  │
│  Similarity score            │  AI_SIMILARITY         │  FLOAT   │
│  Parse document structure    │  AI_PARSE_DOCUMENT     │  JSON    │
│  General text generation     │  AI_COMPLETE           │  STRING  │
│  Structured JSON generation  │  AI_COMPLETE + schema  │  JSON    │
│  Error-safe batch processing │  TRY_COMPLETE          │  STRING  │
│  Token estimation            │  AI_COUNT_TOKENS       │  INT     │
│  Text chunking               │  SPLIT_TEXT_*          │  ARRAY   │
│  File reference              │  TO_FILE               │  FILE    │
│  Multi-file prompt           │  PROMPT                │  OBJECT  │
└──────────────────────────────┴────────────────────────┴──────────┘
```

---

*Use this alongside the function reference notes for exam scenario questions.*

---

<p align="center">
  <a href="./README.md">🏠 Domain Home</a> &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="../README.md">📚 Main Home</a>
</p>
