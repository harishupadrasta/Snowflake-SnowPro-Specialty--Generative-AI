<h1 align="center">✅ Domain 2 Quiz: Snowflake Gen AI Functions</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Questions-60-blue?style=for-the-badge" alt="60 Questions"/>
  <img src="https://img.shields.io/badge/Domain_Weight-38%25-red?style=for-the-badge" alt="38%"/>
  <img src="https://img.shields.io/badge/⚠️_LARGEST_DOMAIN-critical-yellow?style=for-the-badge" alt="Largest"/>
</p>

> **Covers:** AI_COMPLETE, structured output, task-specific functions, vectors, helpers, pipelines, fine-tuning, REST API

---

## 📋 Section A: AI_COMPLETE Fundamentals (Q1-Q15)

### Q1.
What is the default temperature for AI_COMPLETE?

A) 0.5  B) 0.7  C) 0  D) 1  

**Answer: C** — Default temperature is 0 (deterministic). Higher values = more creative/random.

---

### Q2.
Which parameter enables Cortex Guard to filter unsafe content?

A) `model_parameters => {'safety': TRUE}`  
B) `model_parameters => {'guardrails': TRUE}`  
C) `response_format => {'safe': true}`  
D) `model_parameters => {'content_filter': 'on'}`  

**Answer: B** — `guardrails: TRUE` in model_parameters activates Cortex Guard (built on Llama Guard 3).

---

### Q3.
What does `show_details => TRUE` add to AI_COMPLETE output?

A) Internal reasoning chain  
B) Token usage statistics (prompt_tokens, completion_tokens, total_tokens)  
C) Alternative responses  
D) Model confidence score  

**Answer: B** — Returns JSON with `usage` object containing token counts + model name + timestamp.

---

### Q4.
A developer uses AI_COMPLETE with `response_format` to guarantee JSON. For OpenAI (GPT) models, what additional requirement applies?

A) Nothing extra needed  
B) `additionalProperties: false` must be set in every schema object + all properties in `required`  
C) The prompt must contain "respond in JSON"  
D) Only `type: 'json'` is needed without a schema  

**Answer: B** — OpenAI models specifically require `additionalProperties: false` and comprehensive `required` arrays.

---

### Q5.
What is the maximum default output tokens for AI_COMPLETE?

A) 1024  B) 2048  C) 4096  D) 8192  

**Answer: C** — Default max_tokens is 4096. Can be increased but model-specific limits apply.

---

### Q6.
Which AI_COMPLETE input variant supports passing multiple images in one call?

A) Single string  B) Single image  C) Prompt object (PROMPT function)  D) JSON array  

**Answer: C** — The PROMPT() function with `{0}`, `{1}` placeholders supports multiple files.

---

### Q7.
A batch query processing 100K rows fails on row 50,000 due to a malformed input. Which approach prevents this?

A) Use AI_COMPLETE with try-catch  
B) Use TRY_COMPLETE (returns NULL on error, query continues)  
C) Set max_tokens higher  
D) Use a larger warehouse  

**Answer: B** — TRY_COMPLETE returns NULL for failed rows instead of failing the entire query.

---

### Q8.
What does `return_error_details => TRUE` return for a failed row?

A) Just NULL  
B) An OBJECT with `{value: null, error: "error message"}`  
C) The error is logged but not returned  
D) The original input text  

**Answer: B** — Returns object with `value` (NULL if error) and `error` (message or NULL if success).

---

### Q9.
A developer uses `response_format => TYPE OBJECT(name STRING, age NUMBER)`. What is this syntax called?

A) JSON Schema  B) TYPE literal  C) Pydantic schema  D) SQL DDL format  

**Answer: B** — TYPE literals use SQL types (STRING, NUMBER, ARRAY, OBJECT) to define structured output.

---

### Q10.
Which model supports audio and video input with AI_COMPLETE?

A) claude-sonnet-4-6  B) llama3.1-70b  C) gemini-3.1-pro  D) mistral-large2  

**Answer: C** — gemini-3.1-pro (and gemini-3.5-flash) are the only models supporting audio/video. Claude/OpenAI support images only.

---

### Q11.
How are audio files billed when processed by AI_COMPLETE?

A) Per file  B) Per megabyte  C) 50 tokens per second of audio  D) Same as text input  

**Answer: C** — Audio is billed at 50 tokens/second. A 30-min call = 90,000 tokens.

---

### Q12.
What is the PROMPT() function used for?

A) Generating prompts automatically  
B) Building multi-file prompt objects with `{0}`, `{1}` placeholders for TO_FILE references  
C) Validating prompts for safety  
D) Counting tokens in a prompt  

**Answer: B** — PROMPT('text {0} more text {1}', TO_FILE(...), TO_FILE(...)) builds multi-file prompts.

---

### Q13.
Which statement about TO_FILE is FALSE?

A) Stage must have server-side encryption (SNOWFLAKE_SSE or AWS_SSE_S3)  
B) Filenames are case-sensitive  
C) Works with user stages and table stages  
D) Requires directory table enabled for DIRECTORY() queries  

**Answer: C** — TO_FILE does NOT work with user stages or table stages. Must be named internal/external stages.

---

### Q14.
The Cortex REST API supports two specifications. Which is correct?

A) Chat Completions (OpenAI compatible, all models) + Messages (Anthropic, Claude only)  
B) Chat Completions (Claude only) + Messages (all models)  
C) Both support all models  
D) REST API only supports one specification  

**Answer: A** — Chat Completions = OpenAI format, all models. Messages = Anthropic format, Claude only.

---

### Q15.
Why would you choose the Cortex REST API over AI_COMPLETE in SQL?

A) REST API is cheaper  
B) REST API has lower latency for real-time applications  
C) REST API supports more models  
D) REST API doesn't require authentication  

**Answer: B** — REST API has lower latency (direct HTTP) vs SQL (query parsing overhead). Ideal for real-time chatbots.

---

## ?? Section B: Task-Specific Functions (Q16-Q30)

### Q16.
AI_CLASSIFY is called with 4 category labels, each with a description. How are the labels billed?

A) Once per query  
B) Once per AI_CLASSIFY call  
C) As input tokens FOR EACH ROW processed  
D) Labels are free; only the input text is billed  

**Answer: C** — Labels + descriptions count as input tokens PER ROW. With 10K rows, each row's bill includes all label tokens.

---

### Q17.
What does AI_FILTER return?

A) A category label  B) A float score  C) A boolean (TRUE/FALSE)  D) A JSON object  

**Answer: C** — AI_FILTER returns BOOLEAN, perfect for WHERE clauses.

---

### Q18.
AI_AGG processes 1 million rows. How does it handle the context window limit?

A) It truncates to the first 100 rows  
B) It processes in batches automatically — no context window limit  
C) It fails with an error  
D) It uses a special extended-context model  

**Answer: B** — AI_AGG (and AI_SUMMARIZE_AGG) process in batches. No context window limit. Can handle millions of rows.

---

### Q19.
AI_SENTIMENT is called WITHOUT an entities array. What does it return?

A) An OBJECT with categories  
B) A FLOAT between -1 and 1  
C) A STRING ('positive', 'negative', 'neutral')  
D) An ARRAY of scores  

**Answer: B** — Without entities: returns FLOAT (-1 to 1). With entities: returns OBJECT with categories.

---

### Q20.
AI_SENTIMENT is called WITH `['quality', 'price', 'service']` entities. What does it return?

A) A FLOAT  
B) An OBJECT with a `categories` field containing per-entity sentiment  
C) Three separate FLOAT values  
D) A STRING  

**Answer: B** — Returns `{"categories": [{"name":"overall","sentiment":"mixed"}, {"name":"quality","sentiment":"positive"}, ...]}`.

---

### Q21.
What is the legacy function name for AI_SENTIMENT with entities?

A) SNOWFLAKE.CORTEX.SENTIMENT  
B) SNOWFLAKE.CORTEX.ENTITY_SENTIMENT  
C) SNOWFLAKE.CORTEX.CLASSIFY_SENTIMENT  
D) SNOWFLAKE.CORTEX.ASPECT_SENTIMENT  

**Answer: B** — ENTITY_SENTIMENT is the legacy function (being deprecated by end of 2026). AI_SENTIMENT with entities replaces it.

---

### Q22.
Which function removes PII from text?

A) AI_FILTER  B) AI_EXTRACT  C) AI_REDACT  D) AI_CLASSIFY  

**Answer: C** — AI_REDACT replaces PII with bracketed placeholders: "John" → "[PERSON_NAME]"

---

### Q23.
AI_TRANSCRIBE requires which model family?

A) Any model with multimodal support  
B) Only gemini-3.1-pro or gemini-3.5-flash  
C) Only claude-sonnet-4-6  
D) It uses a dedicated transcription model (no selection needed)  

**Answer: B** — AI_TRANSCRIBE currently requires Gemini models for audio/video processing.

---

### Q24.
What does AI_SIMILARITY return?

A) Boolean (similar/not similar)  B) FLOAT (0 to 1)  C) VECTOR  D) Integer distance  

**Answer: B** — Returns a float similarity score. Works with text-to-text, text-to-image, image-to-image.

---

### Q25.
Which function is best for "Does this review mention a delivery problem?"

A) AI_SENTIMENT  B) AI_CLASSIFY  C) AI_FILTER  D) AI_EXTRACT  

**Answer: C** — AI_FILTER returns TRUE/FALSE for a natural language condition. Perfect for "does this mention X?" questions.

---

### Q26.
SUMMARIZE vs AI_SUMMARIZE_AGG — what's the key difference?

A) They are identical  
B) SUMMARIZE works on single text; AI_SUMMARIZE_AGG is aggregate (GROUP BY, no context limit)  
C) AI_SUMMARIZE_AGG is newer and replaces SUMMARIZE  
D) SUMMARIZE uses RAG; AI_SUMMARIZE_AGG doesn't  

**Answer: B** — SUMMARIZE = single row. AI_SUMMARIZE_AGG = aggregate function across multiple rows with no context limit.

---

### Q27.
AI_TRANSLATE takes which parameters?

A) (text, target_language)  
B) (text, source_language, target_language)  
C) (model, text, target_language)  
D) (text, language_pair)  

**Answer: B** — `AI_TRANSLATE(text, source_lang, target_lang)` using ISO 639-1 codes. Source can be 'auto'.

---

### Q28.
A developer needs per-product complaint themes from 100K reviews. Which function and syntax?

```sql
SELECT product_name, ??? 
FROM reviews 
GROUP BY product_name;
```

A) `AI_COMPLETE('llama3.1-70b', CONCAT(reviews))`  
B) `AI_AGG(review_text, 'What are the top 3 complaint themes?')`  
C) `SUMMARIZE(review_text)`  
D) `AI_CLASSIFY(review_text, ['theme1','theme2'])`  

**Answer: B** — AI_AGG is the aggregate function for multi-row analysis with GROUP BY.

---

### Q29.
AI_EXTRACT's responseFormat (schema definition) is billed how?

A) Not billed  
B) Counted as input tokens  
C) Counted as output tokens  
D) Fixed per-call fee  

**Answer: B** — The responseFormat/schema definition counts as input tokens for billing.

---

### Q30.
For documents processed by AI_EXTRACT, how many tokens per page are billed?

A) 100  B) 500  C) 970  D) 2000  

**Answer: C** — Documents are billed at 970 tokens per page.

---

## ?? Section C: Vector Functions (Q31-Q40)

### Q31.
AI_EMBED returns which data type?

A) ARRAY  B) VARIANT  C) VECTOR  D) FLOAT  

**Answer: C** — Returns VECTOR type (e.g., VECTOR(FLOAT, 1024)).

---

### Q32.
Which embedding model supports IMAGE input?

A) snowflake-arctic-embed-m-v1.5  B) snowflake-arctic-embed-l-v2.0  C) voyage-multimodal-3  D) e5-base-v2  

**Answer: C** — voyage-multimodal-3 is the only model supporting image embeddings.

---

### Q33.
What does VECTOR_COSINE_SIMILARITY return when vectors are identical?

A) 0  B) 1  C) Infinity  D) -1  

**Answer: B** — Cosine similarity of 1.0 means identical direction (same meaning).

---

### Q34.
VECTOR_L2_DISTANCE returns 0 when:

A) Vectors are orthogonal  B) Vectors are identical  C) Vectors are opposite  D) Vectors are normalized  

**Answer: B** — L2 distance of 0 = identical vectors. Lower = more similar.

---

### Q35.
Which function reduces vector dimensions for storage optimization?

A) VECTOR_NORMALIZE  B) VECTOR_TRUNCATE  C) VECTOR_COMPRESS  D) VECTOR_REDUCE  

**Answer: B** — VECTOR_TRUNCATE(embedding, 256) reduces from 1024 to 256 dimensions.

---

### Q36.
For AI_EMBED, which billing model applies?

A) Input + output tokens  B) Input tokens only  C) Per embedding generated  D) Per dimension  

**Answer: B** — Embedding functions bill ONLY input tokens (no "output" since embeddings aren't text).

---

### Q37.
Which role provides LEAST privilege for embedding-only access?

A) SNOWFLAKE.CORTEX_USER  B) SNOWFLAKE.AI_FUNCTIONS_USER  C) SNOWFLAKE.CORTEX_EMBED_USER  D) PUBLIC  

**Answer: C** — CORTEX_EMBED_USER grants access to AI_EMBED + Cortex Search creation with managed embeddings only.

---

### Q38.
The recommended chunk size for text before embedding with the default model (arctic-embed-m-v1.5) is:

A) 128 tokens  B) 256 tokens  C) 512 tokens  D) 1024 tokens  

**Answer: C** — The default model has a 512-token context window. Text should be chunked to ≤512 tokens.

---

### Q39.
AI_MULTI_EMBED generates video embeddings. Each embedding includes which metadata?

A) Frame number and resolution  
B) start_sec, end_sec, and embedding_option (visual/audio/transcription)  
C) Video title and duration only  
D) No metadata — just the vector  

**Answer: B** — Returns per-segment embeddings with timestamps (start_sec, end_sec) and modality (visual, audio, transcription).

---

### Q40.
Which vector operation finds the average representation of a cluster of documents?

A) `VECTOR_SUM(embedding)`  
B) `VECTOR_AVG(embedding)` with GROUP BY  
C) `VECTOR_NORMALIZE(embedding)`  
D) `VECTOR_COSINE_SIMILARITY(embedding, centroid)`  

**Answer: B** — VECTOR_AVG computes the centroid (average vector) for a group.

---

## ?? Section D: Helpers, Pipelines & Performance (Q41-Q50)

### Q41.
AI_COUNT_TOKENS incurs which charges?

A) Token-based compute costs  B) Standard warehouse compute only  C) No cost at all  D) Per-call fee  

**Answer: B** — Only standard compute cost (warehouse). No token-based charges. Use it freely for cost estimation.

---

### Q42.
SPLIT_TEXT_RECURSIVE_CHARACTER accepts which parameters?

A) (text, max_chars)  
B) (text, model, {max_tokens, overlap})  
C) (text, separator_string)  
D) (text, num_chunks)  

**Answer: B** — Takes text, model name (for tokenization), and options object with max_tokens and overlap.

---

### Q43.
A dynamic table uses AI_CLASSIFY in its definition. How does refresh work?

A) All rows are reprocessed on every refresh  
B) Only new/changed rows are processed (incremental)  
C) Dynamic tables don't support AI functions  
D) Refresh must be triggered manually  

**Answer: B** — Dynamic tables refresh incrementally by default, only processing new/changed rows.

---

### Q44.
Which pipeline pattern is MOST cost-efficient for enriching data as it arrives?

A) Scheduled stored procedure processing all rows hourly  
B) Stream + Task (processes only new rows when they arrive)  
C) View with AI functions (computes on every query)  
D) Manual batch processing weekly  

**Answer: B** — Streams capture only new/changed rows. Tasks process only when data exists (SYSTEM$STREAM_HAS_DATA). Most efficient.

---

### Q45.
The FINETUNE function requires training data with which column names?

A) input, output  B) prompt, completion  C) question, answer  D) text, label  

**Answer: B** — Must be exactly `prompt` and `completion` (case-insensitive). Other column names get ignored.

---

### Q46.
A fine-tuned model is created in AWS US West 2. Can it be used via cross-region inference from AWS EU Frankfurt?

A) Yes, cross-region works for fine-tuned models  
B) No, fine-tuned models do NOT support cross-region inference  
C) Only if the base model supports cross-region  
D) Only with Provisioned Throughput  

**Answer: B** — Cross-region inference does NOT support fine-tuned models. Must use in the same region where trained (or use database replication to copy).

---

### Q47.
Which FINETUNE operation returns the training progress and status?

A) FINETUNE('STATUS', job_id)  
B) FINETUNE('DESCRIBE', job_id)  
C) FINETUNE('SHOW', job_id)  
D) FINETUNE('PROGRESS', job_id)  

**Answer: B** — DESCRIBE returns status, progress, trained_tokens, training/validation loss.

---

### Q48.
After fine-tuning, how do you call the fine-tuned model?

A) A special FINETUNE_COMPLETE function  
B) `AI_COMPLETE('my_finetuned_model', 'prompt')` — same as any model  
C) Only via REST API  
D) Only through the Model Registry deploy function  

**Answer: B** — Fine-tuned models are used with AI_COMPLETE just like any other model name.

---

### Q49.
For fine-tuning arctic-extract (document extraction), what is the training data format?

A) prompt + completion columns  
B) FILE + Prompt + Response columns in a Dataset object  
C) image + label columns  
D) JSON with question-answer pairs  

**Answer: B** — arctic-extract fine-tuning uses Snowflake Dataset objects with FILE (path), Prompt (extraction schema), Response (expected JSON).

---

### Q50.
The Cortex REST API is billed in:

A) Snowflake credits per million tokens  
B) Dollars per million tokens  
C) Per request (flat fee)  
D) Same as AI_COMPLETE (credits)  

**Answer: B** — REST API specifically bills in dollars per million tokens (not credits). Tracked via CORTEX_REST_API_USAGE_HISTORY.

---

## ?? Section E: Advanced Scenarios (Q51-Q60)

### Q51.
A Streamlit app needs to call AI_COMPLETE AND AI_CLASSIFY. The user's role needs: (Select all)

A) SNOWFLAKE.CORTEX_USER database role  
B) USE AI FUNCTIONS privilege on account  
C) USAGE on database and schema  
D) ACCOUNTADMIN role  
E) CREATE COMPUTE POOL  

**Answer: A, B, C** — CORTEX_USER + USE AI FUNCTIONS + USAGE on objects. No admin or compute pool needed for SiS.

---

### Q52.
Which approach produces the MOST reliable structured JSON from AI_COMPLETE?

A) Adding "respond in JSON" to the prompt  
B) Using `response_format` with a JSON schema  
C) Post-processing the response with TRY_PARSE_JSON  
D) Setting temperature to 0  

**Answer: B** — response_format with schema GUARANTEES compliance. Every token is validated against the schema during generation.

---

### Q53.
A pipeline fails because some rows have text exceeding the model's context window. Best prevention?

A) Filter with `WHERE AI_COUNT_TOKENS(model, text) < context_limit`  
B) Use a larger warehouse  
C) Set max_tokens higher  
D) Use TRY_COMPLETE (this handles it automatically)  

**Answer: A** — Pre-filter with AI_COUNT_TOKENS to skip rows that would exceed limits. TRY_COMPLETE would return NULL but doesn't prevent the attempt.

---

### Q54.
The legacy function SNOWFLAKE.CORTEX.EMBED_TEXT_768 is equivalent to:

A) AI_EMBED with any model  
B) AI_EMBED with a 768-dimension model (snowflake-arctic-embed-m-v1.5)  
C) VECTOR_COSINE_SIMILARITY  
D) AI_SIMILARITY  

**Answer: B** — EMBED_TEXT_768 is the legacy name. Use AI_EMBED with the 768-dim model now.

---

### Q55.
Which Cortex REST API role provides LEAST privilege for REST-only access?

A) SNOWFLAKE.CORTEX_USER  
B) SNOWFLAKE.CORTEX_REST_API_USER  
C) SNOWFLAKE.AI_FUNCTIONS_USER  
D) SNOWFLAKE.CORTEX_AGENT_USER  

**Answer: B** — CORTEX_REST_API_USER grants access to only the REST API, no other Cortex features.

---

### Q56.
The REST API uses the user's _____ role for determining permissions.

A) Session role  B) Primary role  C) Default role  D) ACCOUNTADMIN role  

**Answer: C** — REST API requests use the user's DEFAULT role (not session). Change with `ALTER USER SET DEFAULT_ROLE`.

---

### Q57.
For prompt caching in the REST API, what's the difference between OpenAI and Claude models?

A) Both require explicit cache_control  
B) OpenAI = implicit (automatic for 1024+ tokens); Claude = explicit (cache_control breakpoints)  
C) Neither supports caching  
D) OpenAI uses server-side; Claude uses client-side  

**Answer: B** — OpenAI caches automatically. Claude requires explicit `cache_control: {"type": "ephemeral"}` on content blocks (5-min TTL, max 4 breakpoints).

---

### Q58.
A company wants real-time streaming responses in their chatbot. Which approach achieves this?

A) AI_COMPLETE with `show_details => TRUE`  
B) Cortex REST API with `stream: true`  
C) TRY_COMPLETE in a loop  
D) AI_AGG with one row  

**Answer: B** — REST API `stream: true` enables server-sent events for real-time token streaming.

---

### Q59.
Tool calling in the REST API works how?

A) Model executes tools directly  
B) Model returns tool_calls with function name + arguments; you execute and send results back  
C) Tools are pre-registered in Snowflake  
D) Only available for Agents, not REST API  

**Answer: B** — Multi-step: (1) Send tools list → (2) Model returns tool_calls → (3) You execute locally → (4) Send results back → (5) Model generates final response.

---

### Q60.
A developer wants to use the same model in both AI_COMPLETE (SQL) and the REST API. The model is 'llama3.1-70b'. Is this possible?

A) No — different model names for each  
B) Yes — same model name works in both  
C) Only with fine-tuned models  
D) Only if cross-region is enabled  

**Answer: B** — Same model names work across SQL AI_COMPLETE and REST API. Consistency across interfaces.

---

## Answer Key

| Q | A | Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | C | 11 | C | 21 | B | 31 | C | 41 | B | 51 | A,B,C |
| 2 | B | 12 | B | 22 | C | 32 | C | 42 | B | 52 | B |
| 3 | C | 13 | C | 23 | B | 33 | B | 43 | B | 53 | A |
| 4 | B | 14 | A | 24 | B | 34 | B | 44 | B | 54 | B |
| 5 | C | 15 | B | 25 | C | 35 | B | 45 | B | 55 | B |
| 6 | C | 16 | C | 26 | B | 36 | B | 46 | B | 56 | C |
| 7 | B | 17 | C | 27 | B | 37 | C | 47 | B | 57 | B |
| 8 | B | 18 | B | 28 | B | 38 | C | 48 | B | 58 | B |
| 9 | B | 19 | B | 29 | B | 39 | B | 49 | B | 59 | B |
| 10 | C | 20 | B | 30 | C | 40 | B | 50 | B | 60 | B |

