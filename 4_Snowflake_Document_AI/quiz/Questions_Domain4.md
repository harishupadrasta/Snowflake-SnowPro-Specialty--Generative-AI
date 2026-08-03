<h1 align="center">✅ Domain 4 Quiz: Snowflake Document Processing</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Questions-40-blue?style=for-the-badge" alt="40 Questions"/>
  <img src="https://img.shields.io/badge/Domain_Weight-15%25-teal?style=for-the-badge" alt="15%"/>
  <img src="https://img.shields.io/badge/Focus-Parse_&_Extract-orange?style=for-the-badge" alt="Parse & Extract"/>
</p>

> **Covers:** AI_PARSE_DOCUMENT, AI_EXTRACT, Document AI pipelines, arctic-extract, stages, troubleshooting

---

## 📋 Section A: AI_PARSE_DOCUMENT (Q1-Q12)

### Q1.
AI_PARSE_DOCUMENT supports which two modes?

A) TEXT and BINARY  B) OCR and LAYOUT  C) FAST and DETAILED  D) SIMPLE and STRUCTURED  

**Answer: B** — OCR (text only, faster) and LAYOUT (preserves tables/headers as markdown).

---

### Q2.
LAYOUT mode returns content in what format?

A) Plain text  B) Structured markdown (preserving tables, headers, lists)  C) HTML  D) JSON with coordinates  

**Answer: B** — LAYOUT returns markdown-like content that preserves document structure.

---

### Q3.
How is AI_PARSE_DOCUMENT billed?

A) Per character  B) Per megabyte  C) 970 tokens per page  D) Per file  

**Answer: C** — Billed at 970 tokens per page regardless of mode.

---

### Q4.
What does `page_split: TRUE` do?

A) Splits the document into separate files  
B) Returns an ARRAY of per-page results (each with page_number and content)  
C) Processes pages in parallel  
D) Splits at paragraph boundaries  

**Answer: B** — Returns array: `[{page_number: 1, content: "..."}, {page_number: 2, content: "..."}, ...]`

---

### Q5.
What does `page_limit: 5` achieve?

A) Limits output to 5 lines  B) Processes only the first 5 pages (cost control)  C) Sets timeout to 5 seconds  D) Returns only pages with >5 tokens  

**Answer: B** — Only processes first N pages. Use for cost control on large documents.

---

### Q6.
Which is faster and cheaper for simple text extraction?

A) LAYOUT mode  B) OCR mode  C) They're the same speed  D) Depends on document size  

**Answer: B** — OCR is faster (text only, no structural analysis). Same token cost per page, but faster processing.

---

### Q7.
A technical report has complex tables. Which mode should you use?

A) OCR — tables are just text  B) LAYOUT — preserves table structure  C) Either works equally well  D) AI_EXTRACT is required  

**Answer: B** — LAYOUT preserves table structure in the output. OCR would lose table formatting.

---

### Q8.
Which file formats does AI_PARSE_DOCUMENT support? (Select all)

A) PDF  B) JPEG/PNG images  C) DOCX/PPTX/XLSX  D) MP4 video  E) CSV  

**Answer: A, B, C, E** — Supports PDF, images, Office files, and text formats. NOT video.

---

### Q9.
The function `AI_PARSE_DOCUMENT(TO_FILE('@stage', 'doc.pdf'), {'mode': 'LAYOUT'}):content` — what does `:content` do?

A) Filters to only content pages  
B) Extracts the content field from the returned JSON object  
C) Specifies content-type header  
D) Enables content-based filtering  

**Answer: B** — AI_PARSE_DOCUMENT returns a JSON object. `:content` extracts the text/markdown field.

---

### Q10.
A document is 200 pages. Processing all pages would cost too many tokens. Best approach?

A) Use a larger warehouse  
B) Set `page_limit` to process only needed pages  
C) Use OCR mode (it processes fewer pages automatically)  
D) Split the PDF externally first  

**Answer: B** — `page_limit` controls max pages processed. 200 pages × 970 tokens = 194,000 tokens. Limit to what you need.

---

### Q11.
AI_PARSE_DOCUMENT returns NULL for a file. What is the MOST likely cause?

A) The file doesn't exist (or path is wrong — filenames are case-sensitive)  
B) The warehouse is too small  
C) Cross-region is disabled  
D) The file is too new  

**Answer: A** — Most common cause. Filenames are CASE-SENSITIVE. Also check stage exists and has correct encryption.

---

### Q12.
Which encryption type makes a stage INCOMPATIBLE with AI functions?

A) SNOWFLAKE_SSE  B) AWS_SSE_S3  C) SNOWFLAKE_FULL  D) AWS_SSE_KMS  

**Answer: C** — SNOWFLAKE_FULL is client-side encryption. AI functions require server-side (SSE).

---

## ?? Section B: AI_EXTRACT and Structured Extraction (Q13-Q20)

### Q13.
What does AI_EXTRACT return?

A) Plain text  B) A JSON object matching the provided schema  C) An ARRAY  D) A FLOAT  

**Answer: B** — Returns JSON object with fields matching your schema definition.

---

### Q14.
AI_EXTRACT is called with a document file. How are pages billed?

A) Per character extracted  B) 970 tokens per page (same as AI_PARSE_DOCUMENT)  C) Only extracted fields are billed  D) Flat per-file fee  

**Answer: B** — Document pages = 970 tokens per page as input tokens. Plus output tokens for the JSON result.

---

### Q15.
The `responseFormat` schema in AI_EXTRACT is billed as:

A) Not billed  B) Input tokens  C) Output tokens  D) Separate fee  

**Answer: B** — The schema/responseFormat definition counts as input tokens.

---

### Q16.
A company processes thousands of identical invoice forms. They want higher extraction accuracy. Best approach?

A) Use AI_COMPLETE with detailed prompts  
B) Fine-tune arctic-extract on sample invoices  
C) Use LAYOUT mode with page_split  
D) Increase temperature for creativity  

**Answer: B** — Fine-tuning arctic-extract on your specific document type significantly improves extraction accuracy for repeated formats.

---

### Q17.
What is the correct syntax to use a fine-tuned arctic-extract model?

A) `AI_EXTRACT('my_model', TO_FILE(...))`  
B) `AI_EXTRACT(model => 'db.schema.my_model', file => TO_FILE(...))`  
C) `SNOWFLAKE.CORTEX.FINETUNE('PREDICT', 'my_model', ...)`  
D) `AI_COMPLETE('my_model', TO_FILE(...))`  

**Answer: B** — Use AI_EXTRACT with the `model =>` parameter pointing to your fine-tuned model.

---

### Q18.
Fine-tuning arctic-extract requires training data in what format?

A) prompt + completion columns  
B) Dataset object with FILE, Prompt, Response columns  
C) JSON file with question-answer pairs  
D) YAML configuration  

**Answer: B** — arctic-extract uses Snowflake Dataset objects with FILE (path), Prompt (extraction schema), Response (expected JSON).

---

### Q19.
What is the minimum recommended number of documents for fine-tuning arctic-extract?

A) 5  B) 20  C) 100  D) 1000  

**Answer: B** — Snowflake recommends at least 20 documents for fine-tuning.

---

### Q20.
AI_EXTRACT vs AI_COMPLETE with JSON schema — when to prefer AI_EXTRACT?

A) AI_EXTRACT is always better  
B) AI_EXTRACT for known schemas from documents; AI_COMPLETE for flexible text analysis  
C) AI_COMPLETE is always better  
D) They are identical  

**Answer: B** — AI_EXTRACT is purpose-built for extracting predefined fields from documents. AI_COMPLETE + schema is more flexible for arbitrary text analysis.

---

## ?? Section C: Stage Management and File Handling (Q21-Q28)

### Q21.
Which stage types are NOT supported for AI function file access? (Select two)

A) Named internal stages  B) User stages  C) Named external stages  D) Table stages  

**Answer: B, D** — User stages and table stages are NOT supported. Must use named internal or external stages.

---

### Q22.
What is required on a stage to use `DIRECTORY(@stage)` queries?

A) AUTO_REFRESH = TRUE  B) DIRECTORY = (ENABLE = TRUE)  C) ENCRYPTION = NONE  D) SIZE > 0  

**Answer: B** — DIRECTORY must be enabled to query staged files programmatically.

---

### Q23.
Which SQL creates a stage suitable for AI function file processing?

A) `CREATE STAGE s1;`  
B) `CREATE STAGE s1 DIRECTORY=(ENABLE=TRUE) ENCRYPTION=(TYPE='SNOWFLAKE_SSE');`  
C) `CREATE STAGE s1 ENCRYPTION=(TYPE='SNOWFLAKE_FULL');`  
D) `CREATE TABLE STAGE s1;`  

**Answer: B** — Needs directory enabled + server-side encryption (SNOWFLAKE_SSE or AWS_SSE_S3).

---

### Q24.
FL_GET_RELATIVE_PATH(file_ref) is used to:

A) Get the full absolute path  
B) Extract the filename/relative path from a FILE object  
C) Convert paths to URLs  
D) Validate file existence  

**Answer: B** — Extracts the relative path portion from a FILE type value.

---

### Q25.
TO_FILE('@my_stage', 'Invoice_2024.PDF') fails with "invalid image path". The file is named 'invoice_2024.pdf'. Why?

A) The stage doesn't exist  
B) Filenames are case-sensitive — 'Invoice_2024.PDF' ≠ 'invoice_2024.pdf'  
C) PDF files are not supported  
D) The function is deprecated  

**Answer: B** — Filenames are CASE-SENSITIVE. Must match exactly. This is the #1 cause of "file not found" errors.

---

### Q26.
For an external stage on S3, what encryption type supports AI functions?

A) SNOWFLAKE_FULL  B) AWS_CSE  C) AWS_SSE_S3  D) No encryption needed  

**Answer: C** — AWS_SSE_S3 (server-side encryption) works. AWS_CSE (client-side) does NOT.

---

### Q27.
What does AUTO_REFRESH = TRUE do on an external stage?

A) Refreshes AI function results  
B) Automatically refreshes the directory table when new files land in cloud storage  
C) Refreshes the Cortex Search index  
D) Auto-retries failed function calls  

**Answer: B** — Auto-refreshes the directory table metadata when new/updated files appear in the external storage.

---

### Q28.
A FILE data type column stores what?

A) The actual file binary content  
B) A reference/pointer to a staged file (metadata: path, stage, size, etc.)  
C) A URL to download the file  
D) The file's hash  

**Answer: B** — FILE type is a reference/pointer. Contains RELATIVE_PATH, STAGE, SIZE, CONTENT_TYPE, ETAG, LAST_MODIFIED.

---

## ?? Section D: Pipelines and Automation (Q29-Q35)

### Q29.
The standard RAG document pipeline order is:

A) Embed → Parse → Search  
B) Parse → Chunk → Create Cortex Search Service  
C) Search → Parse → Embed  
D) Chunk → Parse → Index  

**Answer: B** — Parse (AI_PARSE_DOCUMENT) → Chunk (SPLIT_TEXT_RECURSIVE_CHARACTER) → Index (CREATE CORTEX SEARCH SERVICE).

---

### Q30.
Which Snowflake feature provides DECLARATIVE, auto-incremental document processing?

A) Stored procedures  B) Dynamic tables  C) Views  D) Streams only  

**Answer: B** — Dynamic tables with TARGET_LAG = declarative + automatic incremental refresh.

---

### Q31.
A task processes new documents from a stream. What condition triggers it?

A) `WHEN SYSTEM$STREAM_HAS_DATA('stream_name')`  
B) `WHEN NEW_FILES_EXIST()`  
C) `WHEN CURRENT_TIMESTAMP() > LAST_RUN`  
D) Tasks always run on schedule  

**Answer: A** — SYSTEM$STREAM_HAS_DATA prevents the task from running when there's nothing new.

---

### Q32.
A Cortex Search service is created over a dynamic table of parsed/chunked documents. What keeps the search index fresh?

A) Manual REFRESH command  B) TARGET_LAG on both the dynamic table and search service  C) A separate task  D) It doesn't auto-refresh  

**Answer: B** — Both have TARGET_LAG. Dynamic table refreshes parsed content; Search service refreshes its index from that content.

---

### Q33.
Which combination processes documents MOST efficiently as they arrive?

A) Cron job polling every minute  
B) Stream on stage + Task with WHEN condition + MEDIUM warehouse  
C) Continuous query running SELECT with AI_PARSE_DOCUMENT  
D) Manual scheduled procedure  

**Answer: B** — Stream detects new files, task runs only when data exists, MEDIUM warehouse (larger doesn't help AI functions).

---

### Q34.
TRY_COMPLETE is used in a document processing pipeline. What benefit does it provide?

A) Faster processing  B) Lower cost  C) Individual file failures don't crash the entire batch  D) Better accuracy  

**Answer: C** — One malformed file returns NULL instead of failing the whole query. Pipeline continues.

---

### Q35.
What's the correct order for a full document-to-search pipeline using SQL?

```
1. CREATE STAGE with SSE + DIRECTORY
2. Upload/stage documents
3. AI_PARSE_DOCUMENT (LAYOUT mode)
4. SPLIT_TEXT_RECURSIVE_CHARACTER (512 tokens, 50 overlap)
5. CREATE CORTEX SEARCH SERVICE (ON chunk_text)
6. CREATE AGENT with cortex_search tool
```

Is this order correct? A) Yes  B) No — step 4 should come before step 3  C) No — step 6 is invalid  D) No — step 5 should use AI_EMBED  

**Answer: A** — This is the correct end-to-end pipeline order. Cortex Search handles embedding internally.

---

## ?? Section E: Document AI and Troubleshooting (Q36-Q40)

### Q36.
Document AI (Snowsight pipeline feature) requires which special role?

A) SNOWFLAKE.CORTEX_USER  
B) SNOWFLAKE.DOCUMENT_INTELLIGENCE_CREATOR  
C) SNOWFLAKE.ML_USER  
D) ACCOUNTADMIN  

**Answer: B** — DOCUMENT_INTELLIGENCE_CREATOR is required for Document AI. Even ACCOUNTADMIN needs it explicitly.

---

### Q37.
Document AI requires which schema privilege(s)? (Select two)

A) CREATE SNOWFLAKE.ML.DOCUMENT_INTELLIGENCE  
B) CREATE MODEL  
C) CREATE CORTEX SEARCH SERVICE  
D) CREATE STREAM  

**Answer: A, B** — Both required since March 2025. CREATE MODEL was added because Document AI now integrates with Model Registry.

---

### Q38.
Document AI's `!PREDICT` method vs AI_PARSE_DOCUMENT — key difference?

A) They are the same  
B) !PREDICT uses a trained model for custom field extraction; AI_PARSE_DOCUMENT returns raw text  
C) AI_PARSE_DOCUMENT is more accurate  
D) !PREDICT doesn't need a stage  

**Answer: B** — Document AI trains a model on your sample documents to extract YOUR specific fields. AI_PARSE_DOCUMENT is a general-purpose parser.

---

### Q39.
A developer gets "Error in secure object" when calling AI_PARSE_DOCUMENT. Most likely cause?

A) The model is not available  
B) The stage doesn't exist or is not accessible (check name, ensure @ prefix is used)  
C) The file is too large  
D) Cross-region is disabled  

**Answer: B** — "Error in secure object" typically means the stage doesn't exist or the user can't access it.

---

### Q40.
AI Functions are currently incompatible with:

A) Dynamic tables  B) Streams  C) Custom network policies  D) Views  

**Answer: C** — "AI Functions are currently incompatible with custom network policies" — stated in official docs.

---

## Answer Key

| Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|
| 1 | B | 11 | A | 21 | B,D | 31 | A |
| 2 | B | 12 | C | 22 | B | 32 | B |
| 3 | C | 13 | B | 23 | B | 33 | B |
| 4 | B | 14 | B | 24 | B | 34 | C |
| 5 | B | 15 | B | 25 | B | 35 | A |
| 6 | B | 16 | B | 26 | C | 36 | B |
| 7 | B | 17 | B | 27 | B | 37 | A,B |
| 8 | A,B,C,E | 18 | B | 28 | B | 38 | B |
| 9 | B | 19 | B | 29 | B | 39 | B |
| 10 | B | 20 | B | 30 | B | 40 | C |


---

<p align="center">
  <a href="./README.md">🏠 Domain Home</a> &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="../../README.md">📚 Main Home</a>
</p>
