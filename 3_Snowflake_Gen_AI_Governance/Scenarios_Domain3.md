# Domain 3: Scenarios and Decision Guide

> **Governance scenarios** — Access control, cost management, and monitoring decisions

---

## Scenario 1: "Only the analytics team should use AI functions"

**Current state:** All users have access (CORTEX_USER on PUBLIC by default).

**Solution:**
```sql
USE ROLE ACCOUNTADMIN;

-- Step 1: Remove default access
REVOKE DATABASE ROLE SNOWFLAKE.CORTEX_USER FROM ROLE PUBLIC;
REVOKE USE AI FUNCTIONS ON ACCOUNT FROM ROLE PUBLIC;

-- Step 2: Grant to specific team
GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE analytics_role;
GRANT USE AI FUNCTIONS ON ACCOUNT TO ROLE analytics_role;

-- Step 3: Assign users
GRANT ROLE analytics_role TO USER analyst_1;
GRANT ROLE analytics_role TO USER analyst_2;
```

**Exam trap:** Users need BOTH Layer 1 (USE AI FUNCTIONS) AND Layer 2 (database role). Missing either = no access.

---

## Scenario 2: "Analyst needs only AI_CLASSIFY and AI_SENTIMENT — nothing else"

**Solution (per-function privileges):**
```sql
USE ROLE ACCOUNTADMIN;

-- Revoke blanket access
REVOKE USE AI FUNCTIONS ON ACCOUNT FROM ROLE PUBLIC;

-- Grant specific functions only
GRANT USE AI FUNCTION AI_CLASSIFY ON ACCOUNT TO ROLE analyst_role;
GRANT USE AI FUNCTION AI_SENTIMENT ON ACCOUNT TO ROLE analyst_role;
GRANT DATABASE ROLE SNOWFLAKE.AI_FUNCTIONS_USER TO ROLE analyst_role;
```

**Key:** Per-function and blanket have OR relationship. If role still has blanket USE AI FUNCTIONS from another grant, per-function restrictions don't help.

---

## Scenario 3: "Our AI spend is growing 30% month-over-month"

**Investigation queries:**
```sql
-- Where is the spend going?
SELECT FUNCTION_NAME, MODEL_NAME, 
    SUM(CREDITS_USED) AS credits,
    SUM(TOKENS_CONSUMED + TOKENS_PRODUCED) AS total_tokens
FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY
WHERE START_TIME > DATEADD(day, -30, CURRENT_TIMESTAMP())
GROUP BY 1, 2 ORDER BY credits DESC;

-- Who is spending the most?
SELECT USER_NAME, SUM(CREDITS_USED) AS credits
FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY
WHERE START_TIME > DATEADD(day, -30, CURRENT_TIMESTAMP())
GROUP BY 1 ORDER BY credits DESC LIMIT 10;

-- What specific queries cost the most?
SELECT QUERY_ID, MODEL_NAME, TOKENS_CONSUMED, TOKENS_PRODUCED, CREDITS_USED
FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY
ORDER BY CREDITS_USED DESC LIMIT 20;
```

**Cost reduction strategies:**
1. Switch expensive models (claude) to cheaper ones (llama3.1-8b) for simple tasks
2. Use AI_CLASSIFY instead of AI_COMPLETE for classification
3. Cache results in tables — don't reprocess unchanged data
4. Set temperature=0 (fewer retries needed)
5. Restrict expensive models via RBAC (only DS team gets claude)
6. Use MEDIUM warehouse max (larger wastes money)

---

## Scenario 4: "Need to ensure AI responses don't contain harmful content"

**Two levels of guardrails:**

**Level 1: Per-query (function parameter):**
```sql
SELECT AI_COMPLETE(
    'llama3.1-70b', user_input,
    model_parameters => {'guardrails': TRUE}
);
```

**Level 2: Account-wide default:**
```sql
ALTER ACCOUNT SET AI_SETTINGS = '{"guardrails": {"enabled": true}}';
```

**What gets filtered:** Violence, hate speech, sexual content, self-harm, weapons, regulated substances.

**Cost impact:** Additional input tokens charged (based on AI_COMPLETE output length). Budget ~10-20% overhead.

---

## Scenario 5: "Audit who used which AI models last week"

```sql
-- AI SQL Functions usage
SELECT USER_NAME, FUNCTION_NAME, MODEL_NAME, 
    COUNT(*) AS calls, SUM(CREDITS_USED) AS credits
FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY
WHERE START_TIME > DATEADD(day, -7, CURRENT_TIMESTAMP())
GROUP BY 1, 2, 3 ORDER BY credits DESC;

-- REST API usage
SELECT USER_ID, MODEL_NAME, COUNT(*) AS requests, SUM(TOKENS) AS tokens
FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_REST_API_USAGE_HISTORY
WHERE START_TIME > DATEADD(day, -7, CURRENT_TIMESTAMP())
GROUP BY 1, 2 ORDER BY tokens DESC;

-- Cortex Analyst usage
SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.CORTEX_ANALYST_USAGE_HISTORY
WHERE START_TIME > DATEADD(day, -7, CURRENT_TIMESTAMP());
```

---

## Scenario 6: "Evaluate quality of our RAG application"

**Use TruLens with Snowflake:**

```python
from trulens.core import TruSession
from trulens.connectors.snowflake import SnowflakeConnector
from trulens.providers.cortex import Cortex

# Connect (logs stored in Snowflake tables)
connector = SnowflakeConnector(snowpark_session=session)
tru = TruSession(connector=connector)

# Define quality metrics (RAG Triad)
provider = Cortex(snowpark_session=session, model="llama3.1-70b")

# 1. Context Relevance: Is retrieved context relevant to question?
f_context_relevance = provider.context_relevance

# 2. Groundedness: Is answer supported by context?
f_groundedness = provider.groundedness_measure_with_cot_reasons

# 3. Answer Relevance: Does answer address the question?
f_answer_relevance = provider.relevance
```

**RAG Triad interpretation:**
- Context Relevance LOW → Search service returning irrelevant chunks (fix: better chunking, tune scoring weights)
- Groundedness LOW → LLM hallucinating beyond context (fix: stronger prompt, lower temperature)
- Answer Relevance LOW → Response doesn't address question (fix: improve prompt engineering)

---

## Scenario 7: "Which usage view do I need?"

| Question | View | Notes |
|----------|------|-------|
| "How much did AI functions cost?" | `CORTEX_FUNCTIONS_USAGE_HISTORY` | Per-call, 1-hour aggregation |
| "Cost per specific query?" | `CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY` | Per-query, per-model breakdown |
| "REST API usage?" | `CORTEX_REST_API_USAGE_HISTORY` | Billed in dollars, not credits |
| "Cortex Analyst messages?" | `CORTEX_ANALYST_USAGE_HISTORY` | Per-message billing |
| "Search service daily usage?" | `CORTEX_SEARCH_DAILY_USAGE_HISTORY` | Queries + storage |
| "Fine-tuning training cost?" | `CORTEX_FINE_TUNING_USAGE_HISTORY` | Training tokens × epochs |
| "Provisioned throughput?" | `CORTEX_PROVISIONED_THROUGHPUT_USAGE_HISTORY` | Reserved capacity |
| "High-level ALL AI services?" | `METERING_DAILY_HISTORY` WHERE SERVICE_TYPE='AI_SERVICES' | Summary |
| "REST API rate limits?" | `CORTEX_REST_API_RATE_LIMIT_POLICIES` | RPM and TPM per model |

---

## Scenario 8: "Check which models are available in our account"

```sql
-- See all available models with lifecycle status
SHOW CORTEX BASE MODELS IN SCHEMA SNOWFLAKE.MODELS;

-- Check current allowlist
SHOW PARAMETERS LIKE 'CORTEX_MODELS_ALLOWLIST' IN ACCOUNT;

-- Check cross-region setting (affects which models accessible)
SHOW PARAMETERS LIKE 'CORTEX_ENABLED_CROSS_REGION' IN ACCOUNT;

-- See model RBAC roles
SHOW APPLICATION ROLES LIKE 'CORTEX-MODEL%' IN APPLICATION SNOWFLAKE;

-- Refresh model objects (picks up new models immediately)
CALL SNOWFLAKE.MODELS.CORTEX_BASE_MODELS_REFRESH();
```

---

## Governance Decision Tree

```
"Who should access what?"
│
├── ALL Cortex features → SNOWFLAKE.CORTEX_USER (default on PUBLIC)
├── Only AI functions (no services) → SNOWFLAKE.AI_FUNCTIONS_USER
├── Only specific functions → USE AI FUNCTION <name> per-function
├── Only Cortex Analyst → SNOWFLAKE.CORTEX_ANALYST_USER
├── Only Cortex Agents/CoWork → SNOWFLAKE.CORTEX_AGENT_USER
├── Only REST API → SNOWFLAKE.CORTEX_REST_API_USER
├── Only embeddings + Search → SNOWFLAKE.CORTEX_EMBED_USER
├── Only Cortex Code (Snowsight) → SNOWFLAKE.COPILOT_USER
├── Only specific models → Model RBAC (CORTEX-MODEL-ROLE-*)
└── Restrict to CoWork UI only → ALTER USER SET ALLOWED_INTERFACES=(SNOWFLAKE_INTELLIGENCE)
```

---

*Use this for governance-related exam scenarios.*
