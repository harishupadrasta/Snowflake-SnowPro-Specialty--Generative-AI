<h1 align="center">✅ Domain 1 Quiz: Snowflake for Gen AI Overview</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Questions-50-blue?style=for-the-badge" alt="50 Questions"/>
  <img src="https://img.shields.io/badge/Domain_Weight-18%25-green?style=for-the-badge" alt="18%"/>
  <img src="https://img.shields.io/badge/Sections-5-orange?style=for-the-badge" alt="5 Sections"/>
</p>

> **Covers:** Cortex AI overview, Search, Analyst, Agents, Intelligence/CoWork, Code, Cross-Region, SPCS/Registry, MCP/CKE, Skills, Plugins  

---

## 📋 Section A: Cortex AI Architecture & Models (Q1-Q10)

### Q1.
A developer calls `AI_COMPLETE('llama3.1-70b', 'Hello')` but gets "unknown model" error. The account is in AWS EU Frankfurt. What is the most likely cause?

A) llama3.1-70b is deprecated  
B) CORTEX_ENABLED_CROSS_REGION is set to DISABLED and the model isn't available locally  
C) The user lacks USE AI FUNCTIONS privilege  
D) The warehouse is too small  

**Answer: B**  
**Explanation:** Not all models are available in all regions natively. If cross-region is DISABLED, only models deployed in your account's home region work. Frankfurt may not have llama3.1-70b natively. Enable cross-region to route to a region that has it.

---

### Q2.
Which statement about Snowflake Cortex AI billing is FALSE?

A) Both input and output tokens are billed for AI_COMPLETE  
B) Only input tokens are billed for AI_EMBED  
C) AI_COUNT_TOKENS incurs token-based compute costs  
D) Audio transcription bills at 50 tokens per second  

**Answer: C**  
**Explanation:** AI_COUNT_TOKENS incurs only standard compute cost (warehouse). It does NOT incur token-based charges — it's specifically designed for cost estimation without billing overhead.

---

### Q3.
A Snowflake account has CORTEX_ENABLED_CROSS_REGION = 'AWS_US'. Which model access behavior is correct?

A) Only models physically deployed in the account's home region can be used  
B) Models can be processed in any AWS US region, but not Azure or GCP  
C) All models globally are accessible  
D) Only Snowflake-native models (Arctic, Llama) are accessible  

**Answer: B**  
**Explanation:** AWS_US routes requests to any AWS US region. This gives access to models deployed across AWS US West 2, US East 1, etc., but NOT to Azure or GCP regions.

---

### Q4.
When cross-region inference processes a request in a different region, which statement is TRUE?

A) Customer data is replicated to the processing region for caching  
B) The inference payload transits transiently and is NOT persisted at the processing region  
C) Data egress charges apply for cross-region traffic  
D) Credits are consumed in the processing region, not the requesting region  

**Answer: B**  
**Explanation:** Data transits transiently only. No persistence, no egress charges, credits charged in YOUR region (requesting).

---

### Q5.
Who can set the CORTEX_ENABLED_CROSS_REGION parameter? (Select two)

A) ACCOUNTADMIN  
B) ORGADMIN  
C) SYSADMIN  
D) SECURITYADMIN  
E) No one — it's set at the organization level only  

**Answer: A only (trick question — only ACCOUNTADMIN)**  
**Explanation:** Only ACCOUNTADMIN can set this parameter. ORGADMIN explicitly CANNOT. It's account-level only (not session, not user, not org).

---

### Q6.
A new Snowflake account was created on April 1, 2026 in a commercial region. What is the default value of CORTEX_ENABLED_CROSS_REGION?

A) DISABLED  
B) AWS_US  
C) ANY_REGION  
D) It depends on the cloud provider  

**Answer: C**  
**Explanation:** For new accounts created in new organizations within commercial regions after March 9, 2026, ANY_REGION is the default.

---

### Q7.
Which namespace is NOT a valid way to call the text completion function?

A) `SNOWFLAKE.CORTEX.COMPLETE('llama3.1-70b', 'Hello')`  
B) `AI_COMPLETE('llama3.1-70b', 'Hello')`  
C) `CORTEX.COMPLETE('llama3.1-70b', 'Hello')`  
D) Both A and B are valid  

**Answer: C**  
**Explanation:** Valid namespaces are `SNOWFLAKE.CORTEX.COMPLETE()` and `AI_COMPLETE()`. Just `CORTEX.COMPLETE()` without the SNOWFLAKE prefix is invalid.

---

### Q8.
For Snowflake-hosted models (Meta, Mistral, DeepSeek), which security guarantee applies?

A) Data is encrypted at rest in the processing region  
B) Data never leaves the Snowflake infrastructure boundary  
C) Data is sent to model developers for quality improvement  
D) Data is stored in a separate secure vault for 30 days  

**Answer: B**  
**Explanation:** For Snowflake-hosted models, your data NEVER leaves Snowflake's boundary. No data sent to model developers or third parties.

---

### Q9.
An organization uses CORTEX_MODELS_ALLOWLIST = 'mistral-large2' and has also granted CORTEX-MODEL-ROLE-LLAMA3.1-70B to the PUBLIC role. A user with the PUBLIC role calls `AI_COMPLETE('llama3.1-70b', 'test')`. What happens?

A) The call fails because llama3.1-70b is not in the allowlist  
B) The call succeeds because RBAC grant permits it (OR relationship)  
C) The call fails because both mechanisms must permit access  
D) The call succeeds only if ACCOUNTADMIN executes it  

**Answer: B**  
**Explanation:** Access is granted if EITHER the allowlist OR RBAC permits it (OR relationship). The RBAC grant allows llama3.1-70b regardless of the allowlist.

---

### Q10.
What is the recommended maximum warehouse size for executing AI function queries?

A) SMALL  
B) MEDIUM  
C) LARGE  
D) X-LARGE  

**Answer: B**  
**Explanation:** Snowflake recommends MEDIUM or smaller. Larger warehouses don't improve AI function performance but increase costs.

---

## ?? Section B: Cortex Search (Q11-Q20)

### Q11.
A Cortex Search service combines which three retrieval techniques?

A) Vector search, full-text search, and collaborative filtering  
B) Vector search, keyword search (BM25), and semantic reranking  
C) Graph search, vector search, and neural ranking  
D) Keyword search, fuzzy matching, and vector search  

**Answer: B**  
**Explanation:** Cortex Search is a hybrid engine: vector (semantic) + BM25 (keyword/lexical) + semantic reranking.

---

### Q12.
In the CREATE CORTEX SEARCH SERVICE statement, what does the ON clause specify?

A) The warehouse used for queries  
B) The primary text column that queries search against  
C) The table the service is based on  
D) The embedding model to use  

**Answer: B**  
**Explanation:** `ON <column>` specifies the primary text column that user queries are matched against.

---

### Q13.
What is the minimum value for AUTO_SUSPEND on a Cortex Search service?

A) 60 seconds  
B) 300 seconds  
C) 1800 seconds (30 minutes)  
D) 3600 seconds (1 hour)  

**Answer: C**  
**Explanation:** Minimum AUTO_SUSPEND is 1800 seconds (30 minutes).

---

### Q14.
A developer queries a Cortex Search service using SEARCH_PREVIEW in a production application. What is wrong with this approach?

A) Nothing — SEARCH_PREVIEW is the recommended production method  
B) SEARCH_PREVIEW is for testing only — it has higher latency and 300KB response limit  
C) SEARCH_PREVIEW requires ACCOUNTADMIN role  
D) SEARCH_PREVIEW doesn't support attribute filtering  

**Answer: B**  
**Explanation:** SEARCH_PREVIEW is strictly for testing/validation. Production should use Python SDK or REST API (10MB limit, lower latency).

---

### Q15.
A Cortex Search service is created with `ATTRIBUTES region, category`. A user without SELECT on the underlying table queries the service. What happens?

A) The query fails with insufficient privileges  
B) The query succeeds because Cortex Search uses owner's rights  
C) The query returns results without the attribute columns  
D) The query is redirected to the underlying table  

**Answer: B**  
**Explanation:** Cortex Search uses **owner's rights** — queries execute with the service owner's privileges. Users with USAGE on the service can see whatever the owner can read.

---

### Q16.
What is the default embedding model for Cortex Search when EMBEDDING_MODEL is not specified?

A) snowflake-arctic-embed-l-v2.0  
B) snowflake-arctic-embed-m-v1.5  
C) e5-base-v2  
D) voyage-multilingual-2  

**Answer: B**  
**Explanation:** Default is snowflake-arctic-embed-m-v1.5 (768 dimensions, 512 token context, English).

---

### Q17.
Which filter operator would you use to find documents where the `priority` field equals "high"?

A) `{"@match": {"priority": "high"}}`  
B) `{"@eq": {"priority": "high"}}`  
C) `{"@equals": {"priority": "high"}}`  
D) `{"priority": "high"}`  

**Answer: B**  
**Explanation:** `@eq` is the equality filter operator for Cortex Search queries.

---

### Q18.
For a multi-index Cortex Search query, which statement is TRUE?

A) All queried fields must use vector indexes only  
B) At least one vector index must be queried (text-only queries error)  
C) Text indexes and vector indexes cannot be mixed  
D) Multi-index queries don't support scoring weights  

**Answer: B**  
**Explanation:** At least one vector index must be queried. A text-only query across multiple indexes would fail.

---

### Q19.
A data engineer needs to perform entity resolution across 50,000 customer records using semantic matching. Which is the most efficient approach?

A) Loop through records calling SEARCH_PREVIEW for each  
B) Use the CORTEX_SEARCH_BATCH table function  
C) Create a Cortex Agent with a search tool  
D) Use VECTOR_COSINE_SIMILARITY manually on all pairs  

**Answer: B**  
**Explanation:** CORTEX_SEARCH_BATCH is designed for high-throughput offline workloads like entity resolution and deduplication. Use for 2,000+ queries. Much higher QPS than interactive APIs.

---

### Q20.
What happens when a Cortex Search service experiences 5 consecutive refresh failures?

A) The service is automatically dropped  
B) The service auto-suspends indexing  
C) An alert is sent to ACCOUNTADMIN  
D) The service continues with stale data indefinitely  

**Answer: B**  
**Explanation:** After 5 consecutive refresh failures, Cortex Search auto-suspends indexing. The existing index remains queryable but won't update until the issue is resolved.

---

## ?? Section C: Cortex Analyst & Semantic Views (Q21-Q30)

### Q21.
In a Semantic View, what distinguishes a METRIC from a FACT?

A) Metrics are optional; facts are required  
B) Metrics are aggregated (SUM, AVG, COUNT); facts are row-level values  
C) Metrics can only reference one table; facts can span tables  
D) Facts are dimensions; metrics are measures  

**Answer: B**  
**Explanation:** FACTS are row-level quantitative values (e.g., unit_price). METRICS are aggregated calculations (e.g., SUM(amount), COUNT(DISTINCT customer_id)).

---

### Q22.
A VQR (Verified Query Repository) entry contains SQL that references `__orders`. What does the `__` prefix indicate?

A) A temporary table  
B) A system table  
C) A logical table name defined in the semantic model  
D) An alias for the physical table  

**Answer: C**  
**Explanation:** VQR SQL must use logical table names prefixed with `__` (double underscore), not physical table names.

---

### Q23.
A semantic view has the instruction: `AI_QUESTION_CATEGORIZATION 'Reject questions about salary data'`. A user asks "What is the average salary?" What happens?

A) The query executes but returns NULL  
B) Cortex Analyst generates SQL that excludes salary columns  
C) The question is rejected/categorized as out-of-scope  
D) An error is thrown  

**Answer: C**  
**Explanation:** AI_QUESTION_CATEGORIZATION controls how questions are classified. It can reject topics, ask for clarification, or route questions. "Reject" means the question is declined.

---

### Q24.
Which role provides the LEAST privilege for a user who ONLY needs Cortex Analyst access?

A) SNOWFLAKE.CORTEX_USER  
B) SNOWFLAKE.CORTEX_ANALYST_USER  
C) SNOWFLAKE.AI_FUNCTIONS_USER  
D) SNOWFLAKE.CORTEX_AGENT_USER  

**Answer: B**  
**Explanation:** CORTEX_ANALYST_USER grants access to ONLY Cortex Analyst. CORTEX_USER grants everything. AI_FUNCTIONS_USER grants scalar functions but not services like Analyst.

---

### Q25.
A company has 50 tables and wants to build a Cortex Analyst solution. What is the recommended approach?

A) Include all 50 tables in one semantic view immediately  
B) Start with 5-10 most critical tables, expand after proving accuracy  
C) Create 50 separate semantic views, one per table  
D) Use AI_COMPLETE to auto-generate the semantic view from DDL  

**Answer: B**  
**Explanation:** Best practice is to start with 5-10 tables for initial POC. Too many tables at once makes the model too large (>100K tokens) and harder to debug accuracy issues.

---

### Q26.
What is the purpose of `use_as_onboarding_question: true` in a VQR entry?

A) The question is used as training data for the model  
B) The question is always displayed to users as a suggested starter question  
C) The question is mandatory for new users to answer  
D) The question triggers a tutorial flow  

**Answer: B**  
**Explanation:** Onboarding questions are always shown to users as suggested starting points, regardless of their query or similarity matching.

---

### Q27.
Cortex Analyst evaluations measure quality using which metric?

A) BLEU score  
B) sql_correctness (LLM-as-judge)  
C) F1 score  
D) Precision/Recall  

**Answer: B**  
**Explanation:** Cortex Analyst evaluations use `sql_correctness`, where an LLM judges whether the generated SQL produces results equivalent to the VQR ground truth.

---

### Q28.
When Cortex Analyst is invoked directly via REST API vs through a Cortex Agent, how does billing differ?

A) Same billing model  
B) Direct API = per message; through Agent = per token  
C) Direct API = free; through Agent = per token  
D) Direct API = per query; through Agent = per message  

**Answer: B**  
**Explanation:** Direct Analyst API bills per message processed. When invoked as a tool within Cortex Agents, billing is per token (input + output).

---

### Q29.
A semantic view has CUSTOM_INSTRUCTIONS but also a highly relevant VQR for the user's question. Which takes priority?

A) Custom instructions always override VQR  
B) VQR always overrides custom instructions  
C) They work together — VQR guides SQL structure, instructions guide formatting/behavior  
D) Whichever was added more recently takes priority  

**Answer: C**  
**Explanation:** They are complementary. VQRs provide SQL templates for similar questions. Custom instructions guide formatting, filtering defaults, and question handling. Neither fully overrides the other.

---

### Q30.
Semantic View materializations (`MAX_STALENESS`) benefit which type of queries?

A) All Cortex Analyst queries  
B) All Cortex Agent queries  
C) Only queries via the SEMANTIC_VIEW() SQL function  
D) Both Analyst and direct SEMANTIC_VIEW() queries  

**Answer: C**  
**Explanation:** Materializations ONLY benefit queries made via the `SEMANTIC_VIEW()` function. Cortex Analyst and Agents execute physical SQL directly, bypassing materializations.

---

## ?? Section D: Cortex Agents & CoWork (Q31-Q40)

### Q31.
What is the correct reasoning loop for a Cortex Agent?

A) Query → Process → Return  
B) Plan → Use Tools → Reflect (repeat as needed)  
C) Parse → Generate → Validate  
D) Receive → Classify → Respond  

**Answer: B**  
**Explanation:** Agents follow: Plan (choose tools) → Use Tools (execute) → Reflect (evaluate results, decide next step or respond). This loop repeats until satisfied.

---

### Q32.
Which SQL function calls a named Cortex Agent object?

A) SNOWFLAKE.CORTEX.AGENT()  
B) SNOWFLAKE.CORTEX.DATA_AGENT_RUN()  
C) SNOWFLAKE.CORTEX.INVOKE_AGENT()  
D) SNOWFLAKE.CORTEX.COMPLETE() with agent parameter  

**Answer: B**  
**Explanation:** DATA_AGENT_RUN() calls named agent objects. AGENT_RUN() (without DATA_) is for inline agent specs without creating a persistent object.

---

### Q33.
A Cortex Agent specification sets `models: orchestration: auto`. What does this mean?

A) The agent uses the cheapest available model  
B) Snowflake automatically selects the highest-quality available model  
C) The model rotates randomly between requests  
D) The user must specify a model at query time  

**Answer: B**  
**Explanation:** "auto" is recommended — Snowflake picks the highest-quality model available for orchestration.

---

### Q34.
Which tool type in a Cortex Agent specification connects to Jira or Salesforce?

A) `cortex_search`  
B) `generic`  
C) `mcp_servers` (via EXTERNAL MCP SERVER)  
D) `web_search`  

**Answer: C**  
**Explanation:** MCP Connectors (CREATE EXTERNAL MCP SERVER) connect agents to external tools like Jira, GitHub, Salesforce.

---

### Q35.
What is the difference between CREATE MCP SERVER and CREATE EXTERNAL MCP SERVER?

A) They are the same command  
B) CREATE MCP SERVER exposes Snowflake TO external clients; CREATE EXTERNAL MCP SERVER connects FROM Snowflake TO external tools  
C) CREATE MCP SERVER is for SPCS; CREATE EXTERNAL MCP SERVER is for stages  
D) CREATE EXTERNAL MCP SERVER requires GPU  

**Answer: B**  
**Explanation:** MCP SERVER = inbound (expose Snowflake capabilities). EXTERNAL MCP SERVER = outbound (connect to external tools). Critical distinction.

---

### Q36.
A user accesses CoWork at `https://ai.snowflake.com` but cannot see any agents. What might be wrong? (Select two)

A) The user's role doesn't have USAGE on any agent objects  
B) A Snowflake CoWork object exists but the agents haven't been added to it  
C) Cross-region inference is disabled  
D) The user needs ACCOUNTADMIN  

**Answer: A, B**  
**Explanation:** Users need USAGE on agents to see them. Also, if a CoWork object exists (`CREATE SNOWFLAKE INTELLIGENCE`), only agents explicitly added to it are visible.

---

### Q37.
How do you restrict a user to ONLY access Snowflake CoWork (no Snowsight, no worksheets)?

A) Revoke all roles  
B) `ALTER USER SET ALLOWED_INTERFACES = (SNOWFLAKE_INTELLIGENCE);`  
C) Only grant CORTEX_AGENT_USER role  
D) Set a network policy blocking Snowsight  

**Answer: B**  
**Explanation:** ALLOWED_INTERFACES = (SNOWFLAKE_INTELLIGENCE) restricts the user to CoWork only, preventing access to any other Snowflake interface.

---

### Q38.
What is "Deep Research" in Snowflake CoWork?

A) A way to query research papers  
B) An investigation mode that decomposes complex questions into multi-step parallel analysis  
C) A feature to search the internet  
D) A model fine-tuning workflow  

**Answer: B**  
**Explanation:** Deep Research breaks complex questions into sub-investigations, runs them in parallel across structured and unstructured data, and synthesizes a report with citations. Can take up to 10 minutes.

---

### Q39.
Agent Skills are defined in which file?

A) agent.yaml  
B) SKILL.md  
C) config.json  
D) skill_definition.sql  

**Answer: B**  
**Explanation:** Each skill is defined by a SKILL.md file at the root of the skill folder. Contains: name, description, instructions, and optional script references.

---

### Q40.
Which agent evaluation framework does Snowflake use for Cortex Agents?

A) RAG Triad  
B) BLEU/ROUGE  
C) GPA (Goal-Plan-Action)  
D) Precision-Recall  

**Answer: C**  
**Explanation:** Agent evaluations use the GPA framework: Goal (what was asked), Plan (what tools were selected), Action (what was executed). Measures tool selection accuracy, execution accuracy, answer correctness, and logical consistency.

---

## ?? Section E: Cross-Region, SPCS & MCP (Q41-Q50)

### Q41.
A company needs to deploy a custom fine-tuned Hugging Face model for real-time inference in Snowflake. Which approach is correct?

A) Upload to a stage and call with AI_COMPLETE  
B) Use SNOWFLAKE.CORTEX.FINETUNE to customize it  
C) Deploy via SPCS (Docker container + GPU compute pool)  
D) Register in Model Registry and call as a UDF (no SPCS needed for any model)  

**Answer: C**  
**Explanation:** Custom Hugging Face models need Docker containers deployed to SPCS with GPU compute pools. FINETUNE only works with supported base models (llama, mistral). Model Registry UDF deployment works for simple sklearn/XGBoost models but complex LLMs need SPCS.

---

### Q42.
What is the correct Docker registry URL format for pushing images to Snowflake?

A) `snowflake.com/registry/<account>/images`  
B) `<orgname>-<account>.registry.snowflakecomputing.com`  
C) `registry.snowflake.com/<account>`  
D) `<account>.snowflake.registry.io`  

**Answer: B**  
**Explanation:** Format is `<orgname>-<account_name>.registry.snowflakecomputing.com`

---

### Q43.
When logging a model to the Snowflake Model Registry for SPCS deployment, which parameter is REQUIRED to infer the model's input/output signature?

A) model_type  
B) sample_input_data  
C) gpu_requests  
D) service_name  

**Answer: B**  
**Explanation:** `sample_input_data` is required — the registry uses it to infer the model's signature (expected input/output schema). Without it, SPCS deployment fails.

---

### Q44.
What privilege is needed on an image repository for a role to deploy services from it?

A) USAGE  
B) READ  
C) SELECT  
D) CREATE IMAGE  

**Answer: B**  
**Explanation:** READ privilege on the image repository is needed to pull images for service creation.

---

### Q45.
Which SPCS instance family provides a single NVIDIA A10G GPU?

A) CPU_X64_L  
B) GPU_NV_S  
C) GPU_NV_M  
D) GPU_NV_L  

**Answer: B**  
**Explanation:** GPU_NV_S = A10G (24GB). GPU_NV_M = A100 (40GB). GPU_NV_L = 4×A100 (160GB).

---

### Q46.
What is the difference between a SERVICE and a JOB in SPCS?

A) Services use CPU; jobs use GPU  
B) Services are long-running (HTTP endpoints); jobs run to completion and stop  
C) Jobs are interactive; services are batch  
D) Services are free; jobs incur compute costs  

**Answer: B**  
**Explanation:** SERVICE = long-running (real-time inference, always available). JOB = finite task (training, batch processing) that runs to completion via `EXECUTE JOB SERVICE`.

---

### Q47.
What are Cortex Knowledge Extensions (CKE)?

A) Code extensions for Cortex Code  
B) Pre-built Cortex Search services shared via Marketplace  
C) GPU accelerators for compute pools  
D) REST API extensions for custom models  

**Answer: B**  
**Explanation:** CKEs are pre-built knowledge packages from the Marketplace containing ready-to-use Cortex Search services with curated, indexed content.

---

### Q48.
A Snowflake-managed MCP server uses which authentication method?

A) API keys  
B) OAuth 2.0  
C) Basic auth (username/password)  
D) SAML  

**Answer: B**  
**Explanation:** Snowflake-managed MCP servers authenticate via OAuth 2.0 (Snowflake OAuth or External OAuth).

---

### Q49.
Which SQL command adds an agent to the CoWork visibility list?

A) `GRANT USAGE ON AGENT TO ROLE PUBLIC`  
B) `ALTER SNOWFLAKE INTELLIGENCE SNOWFLAKE_INTELLIGENCE_OBJECT_DEFAULT ADD AGENT db.schema.my_agent`  
C) `ALTER AGENT SET VISIBLE = TRUE`  
D) `CREATE SNOWFLAKE INTELLIGENCE AGENT`  

**Answer: B**  
**Explanation:** Use ALTER SNOWFLAKE INTELLIGENCE ... ADD AGENT to make agents visible in CoWork (when a CoWork object has been created).

---

### Q50.
A developer is building an agentic AI application using the Cortex Code Agent SDK. Which built-in tools are available? (Select all that apply)

A) Read, Write, Edit  
B) Bash, Glob, Grep  
C) SQL (execute against Snowflake)  
D) GPU (direct GPU access)  
E) All of A, B, and C  

**Answer: E**  
**Explanation:** The Agent SDK provides: Read, Write, Edit, Bash, Glob, Grep, and SQL tools. No direct GPU tool — that requires SPCS.

---

## Answer Key

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | B | 11 | B | 21 | B | 31 | B | 41 | C |
| 2 | C | 12 | B | 22 | C | 32 | B | 42 | B |
| 3 | B | 13 | C | 23 | C | 33 | B | 43 | B |
| 4 | B | 14 | B | 24 | B | 34 | C | 44 | B |
| 5 | A | 15 | B | 25 | B | 35 | B | 45 | B |
| 6 | C | 16 | B | 26 | B | 36 | A,B | 46 | B |
| 7 | C | 17 | B | 27 | B | 37 | B | 47 | B |
| 8 | B | 18 | B | 28 | B | 38 | B | 48 | B |
| 9 | B | 19 | B | 29 | C | 39 | B | 49 | B |
| 10 | B | 20 | B | 30 | C | 40 | C | 50 | E |


---

<p align="center">
  <a href="./README.md">🏠 Domain Home</a> &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="../../README.md">📚 Main Home</a>
</p>
