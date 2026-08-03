<h1 align="center">✅ Domain 3 Quiz: Snowflake Gen AI Governance</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Questions-50-blue?style=for-the-badge" alt="50 Questions"/>
  <img src="https://img.shields.io/badge/Domain_Weight-29%25-purple?style=for-the-badge" alt="29%"/>
  <img src="https://img.shields.io/badge/Focus-RBAC_&_Cost-orange?style=for-the-badge" alt="RBAC & Cost"/>
</p>

> **Covers:** Model access controls, RBAC roles, per-function privileges, cost management, usage views, observability, TruLens, guardrails

---

## 📋 Section A: RBAC and Database Roles (Q1-Q15)

### Q1.
Which database role is granted to PUBLIC by default, giving all users access to ALL Cortex features?

A) SNOWFLAKE.AI_FUNCTIONS_USER  B) SNOWFLAKE.CORTEX_USER  C) SNOWFLAKE.CORTEX_ADMIN  D) SNOWFLAKE.COPILOT_USER  

**Answer: B** — CORTEX_USER is on PUBLIC by default. All others must be explicitly granted.

---

### Q2.
A user has USE AI FUNCTIONS privilege but NOT the CORTEX_USER database role. What can they use?

A) All AI functions  B) Only AI_AGG and AI_SUMMARIZE_AGG  C) Nothing — both layers required  D) Only AI_COMPLETE  

**Answer: C** — Users need BOTH the account privilege (USE AI FUNCTIONS) AND a database role (CORTEX_USER or AI_FUNCTIONS_USER). Missing either = no access.

**Exception:** AI_AGG and AI_SUMMARIZE_AGG can work with just USE AI FUNCTIONS and CORTEX_USER (even without the specific per-function grant).

---

### Q3.
What does SNOWFLAKE.AI_FUNCTIONS_USER provide that SNOWFLAKE.CORTEX_USER doesn't?

A) It provides MORE access  
B) It provides LESS access — scalar AI functions only, excludes AGG functions and services  
C) They are identical  
D) It includes Model Registry access  

**Answer: B** — AI_FUNCTIONS_USER = scalar AI functions only. Excludes: AI_AGG, AI_SUMMARIZE_AGG, Cortex Analyst, Agents, Search, Fine-tuning.

---

### Q4.
Per-function privileges (USE AI FUNCTION <name>) have what relationship with USE AI FUNCTIONS (blanket)?

A) AND — both are required  B) OR — either grants access  C) Per-function overrides blanket  D) They conflict  

**Answer: B** — OR relationship. Having EITHER the blanket OR the per-function grant is sufficient.

---

### Q5.
An admin revokes USE AI FUNCTIONS from PUBLIC and grants only USE AI FUNCTION AI_COMPLETE and USE AI FUNCTION AI_CLASSIFY to `analyst_role`. The analyst calls AI_SENTIMENT. What happens?

A) Succeeds — CORTEX_USER includes all functions  
B) Fails — analyst_role only has AI_COMPLETE and AI_CLASSIFY per-function grants  
C) Succeeds if temperature is 0  
D) Depends on the model used  

**Answer: B** — Without the blanket privilege, only specifically granted per-function privileges work. AI_SENTIMENT was not granted.

---

### Q6.
Which command correctly revokes the CORTEX_USER database role from all users?

A) `REVOKE ROLE SNOWFLAKE.CORTEX_USER FROM ROLE PUBLIC;`  
B) `REVOKE DATABASE ROLE SNOWFLAKE.CORTEX_USER FROM ROLE PUBLIC;`  
C) `DROP DATABASE ROLE SNOWFLAKE.CORTEX_USER;`  
D) `ALTER ACCOUNT SET CORTEX_USER = FALSE;`  

**Answer: B** — Use `REVOKE DATABASE ROLE` syntax. Also should revoke IMPORTED PRIVILEGES on SNOWFLAKE database.

---

### Q7.
SNOWFLAKE.CORTEX_EMBED_USER grants access to which capabilities? (Select two)

A) AI_EMBED function  
B) All AI functions  
C) Creating Cortex Search Services with managed embeddings  
D) Cortex Agents  
E) Fine-tuning  

**Answer: A, C** — CORTEX_EMBED_USER = AI_EMBED + creating Cortex Search with managed vector embeddings only.

---

### Q8.
Database roles in the SNOWFLAKE database cannot be granted directly to:

A) Account roles  B) Users  C) Other database roles  D) PUBLIC  

**Answer: B** — Snowflake database roles cannot be granted directly to users. Must grant to an account role, then assign role to user.

---

### Q9.
Which role is needed for Cortex Code in Snowsight specifically?

A) SNOWFLAKE.CORTEX_USER  B) SNOWFLAKE.COPILOT_USER  C) SNOWFLAKE.CORTEX_CODE_USER  D) ACCOUNTADMIN  

**Answer: B** — COPILOT_USER is specifically for Cortex Code in Snowsight.

---

### Q10.
AI Functions run inside a Native App. The app's role doesn't have USE AI FUNCTIONS. What happens?

A) The function call fails  
B) It succeeds — Native Apps bypass USE AI FUNCTIONS currently  
C) It prompts for ACCOUNTADMIN approval  
D) Only free models work  

**Answer: B** — Known exception: USE AI FUNCTIONS currently does NOT apply inside Snowflake Native Applications.

---

### Q11.
To use AI Functions inside a stored procedure with EXECUTE AS RESTRICTED CALLER, what additional grants are needed?

A) None — standard grants are sufficient  
B) INHERITED CALLER USAGE on SNOWFLAKE schemas and functions + CALLER USAGE on SNOWFLAKE database  
C) ACCOUNTADMIN role  
D) CREATE FUNCTION privilege  

**Answer: B** — RCR procedures need: `GRANT INHERITED CALLER USAGE ON ALL SCHEMAS/FUNCTIONS IN DATABASE snowflake` + `GRANT CALLER USAGE ON DATABASE snowflake`.

---

### Q12.
CORTEX_AGENT_USER provides access to what?

A) All Cortex features  B) Only the Cortex Agents API (and CoWork)  C) Only AI_COMPLETE  D) Only Cortex Search  

**Answer: B** — Least-privilege for agent-only access. Users can interact via CoWork but not call AI functions directly.

---

### Q13.
Which account parameter disables Cortex Analyst for the entire account?

A) CORTEX_MODELS_ALLOWLIST = 'None'  
B) ENABLE_CORTEX_ANALYST = FALSE  
C) CORTEX_ENABLED_CROSS_REGION = 'DISABLED'  
D) REVOKE DATABASE ROLE SNOWFLAKE.CORTEX_ANALYST_USER  

**Answer: B** — ALTER ACCOUNT SET ENABLE_CORTEX_ANALYST = FALSE disables Analyst globally.

---

### Q14.
For the Cortex REST API specifically, which is the least-privilege role?

A) SNOWFLAKE.CORTEX_USER  B) SNOWFLAKE.CORTEX_REST_API_USER  C) SNOWFLAKE.AI_FUNCTIONS_USER  D) PUBLIC  

**Answer: B** — CORTEX_REST_API_USER grants REST API access only, no other Cortex features.

---

### Q15.
A user's Cortex REST API requests are failing with 403. The user has CORTEX_USER role. What's likely wrong?

A) The user's DEFAULT ROLE doesn't have the required privileges  
B) The user needs ACCOUNTADMIN  
C) REST API doesn't support CORTEX_USER  
D) Cross-region is disabled  

**Answer: A** — REST API uses the user's DEFAULT role. Even if the user has CORTEX_USER, it won't help if it's not their default role.

---

## ?? Section B: Model Access Controls (Q16-Q25)

### Q16.
CORTEX_MODELS_ALLOWLIST is set to 'None'. Role X has RBAC grant for llama3.1-70b. User with Role X calls AI_COMPLETE('llama3.1-70b',...). Result?

A) Fails — allowlist blocks all  
B) Succeeds — RBAC grant permits (OR relationship)  
C) Fails — both must permit  
D) Depends on cross-region  

**Answer: B** — Even with allowlist='None', RBAC grants still work. Access = allowlist OR RBAC.

---

### Q17.
What is the status of CORTEX_MODELS_ALLOWLIST?

A) Recommended approach  B) Being deprecated (starting August 2026)  C) Already removed  D) Required for all accounts  

**Answer: B** — Being deprecated. Starting Aug 2026, only change allowed is setting to 'None'. Migrate to RBAC.

---

### Q18.
To grant access to ALL current AND future models via RBAC:

A) `GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" TO ROLE my_role;`  
B) `ALTER ACCOUNT SET CORTEX_MODELS_ALLOWLIST = 'All';`  
C) `GRANT ALL MODELS TO ROLE my_role;`  
D) Both A and B achieve this  

**Answer: A** — CORTEX-MODEL-ROLE-ALL via RBAC is the recommended approach. B works but is being deprecated.

---

### Q19.
Model names in CORTEX_MODELS_ALLOWLIST must be:

A) UPPERCASE  B) lowercase (case-sensitive)  C) Case-insensitive  D) Quoted with double quotes  

**Answer: B** — Must be lowercase and case-sensitive. 'mistral-large2' works; 'MISTRAL-LARGE2' does not.

---

### Q20.
ACCOUNTADMIN always has access to all models regardless of restrictions. How do you test restrictions properly?

A) Log in as a different user  
B) `USE SECONDARY ROLES NONE;` then test with a non-ACCOUNTADMIN role  
C) Disable ACCOUNTADMIN temporarily  
D) Use ORGADMIN instead  

**Answer: B** — Disable secondary roles to prevent ACCOUNTADMIN from being inherited. Then USE ROLE <test_role>.

---

### Q21.
Which command refreshes model objects in SNOWFLAKE.MODELS immediately?

A) `ALTER ACCOUNT REFRESH MODELS;`  
B) `CALL SNOWFLAKE.MODELS.CORTEX_BASE_MODELS_REFRESH();`  
C) `SHOW MODELS IN SNOWFLAKE.MODELS;` (auto-refreshes)  
D) Models auto-refresh every hour  

**Answer: B** — CORTEX_BASE_MODELS_REFRESH() refreshes on-demand. Otherwise, daily auto-refresh.

---

### Q22.
A role has the RBAC model grant but still gets "unknown model" errors. What else could be wrong?

A) The model isn't available in the account's region (cross-region needed)  
B) The model has been deprecated (end of life)  
C) Cross-region is disabled and model isn't local  
D) All of the above could be causes  

**Answer: D** — Model access ≠ model availability. Even with access, it must be regionally available and not deprecated.

---

### Q23.
Which command shows available models and their lifecycle status?

A) `SHOW MODELS;`  
B) `SHOW CORTEX BASE MODELS IN SCHEMA SNOWFLAKE.MODELS;`  
C) `SELECT * FROM INFORMATION_SCHEMA.AI_MODELS;`  
D) `DESCRIBE ACCOUNT MODELS;`  

**Answer: B** — SHOW CORTEX BASE MODELS shows models + lifecycle status (GA/Preview/Legacy/EOL).

---

### Q24.
GA models receive how much notice before deprecation?

A) 30 days  B) 60 days  C) 90 days  D) No guarantee  

**Answer: B** — GA models get at least 60 days deprecation notice. Preview models have NO guarantee.

---

### Q25.
Which command creates a model RBAC grant for a specific model?

A) `GRANT MODEL 'llama3.1-70b' TO ROLE my_role;`  
B) `GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-LLAMA3.1-70B" TO ROLE my_role;`  
C) `GRANT USE MODEL llama3.1-70b ON ACCOUNT TO ROLE my_role;`  
D) `ALTER ROLE my_role ADD MODEL 'llama3.1-70b';`  

**Answer: B** — Grant the APPLICATION ROLE with the quoted model name.

---

## ?? Section C: Cost Management (Q26-Q37)

### Q26.
Which view shows credit consumption aggregated in one-hour increments?

A) CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY  
B) CORTEX_FUNCTIONS_USAGE_HISTORY  
C) METERING_DAILY_HISTORY  
D) QUERY_HISTORY  

**Answer: B** — CORTEX_FUNCTIONS_USAGE_HISTORY provides per-call data aggregated hourly.

---

### Q27.
Which view shows per-QUERY token and credit breakdown (grouped by model)?

A) CORTEX_FUNCTIONS_USAGE_HISTORY  
B) CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY  
C) METERING_DAILY_HISTORY  
D) CORTEX_REST_API_USAGE_HISTORY  

**Answer: B** — QUERY_USAGE_HISTORY breaks down each query's cost by model used.

---

### Q28.
The Cortex REST API is tracked in which usage view?

A) CORTEX_FUNCTIONS_USAGE_HISTORY  B) CORTEX_REST_API_USAGE_HISTORY  C) METERING_DAILY_HISTORY  D) API_USAGE_HISTORY  

**Answer: B** — Separate view for REST API. Note: REST API bills in dollars, not credits.

---

### Q29.
For generative AI functions (AI_COMPLETE, AI_CLASSIFY), which tokens are billed?

A) Input only  B) Output only  C) Input + output  D) Whichever is larger  

**Answer: C** — Generative functions bill BOTH input and output tokens.

---

### Q30.
For embedding functions (AI_EMBED), which tokens are billed?

A) Input only  B) Output only  C) Input + output  D) Per dimension  

**Answer: A** — Embedding functions bill only input tokens (embeddings aren't text output).

---

### Q31.
Cortex Guard (guardrails=TRUE) incurs what additional cost?

A) No additional cost  
B) Additional INPUT tokens charged based on the size of AI_COMPLETE's OUTPUT  
C) Double the normal rate  
D) Flat per-call fee  

**Answer: B** — Guard evaluates the output. Cost = input tokens proportional to output size. Charged IN ADDITION to the AI_COMPLETE cost.

---

### Q32.
A warehouse is set to X-LARGE for AI function queries. What impact does this have?

A) Significantly faster inference  
B) No performance improvement but higher cost  
C) Required for large batch jobs  
D) Enables GPU acceleration  

**Answer: B** — Warehouse MEDIUM max for AI functions. Larger = same speed, more $$$.

---

### Q33.
Which query monitors total AI spending by service type over the last 30 days?

```sql
SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.___
WHERE SERVICE_TYPE = 'AI_SERVICES' 
AND START_TIME > DATEADD(day, -30, CURRENT_TIMESTAMP());
```

A) QUERY_HISTORY  B) WAREHOUSE_METERING_HISTORY  C) METERING_DAILY_HISTORY  D) CORTEX_FUNCTIONS_USAGE_HISTORY  

**Answer: C** — METERING_DAILY_HISTORY with SERVICE_TYPE = 'AI_SERVICES' for high-level daily totals.

---

### Q34.
Which service type covers Cortex Code usage in Snowsight?

A) AI_FUNCTIONS  B) CORTEX_CODE_SNOWSIGHT  C) AI_SERVICES  D) CORTEX_AGENTS  

**Answer: B** — CORTEX_CODE_SNOWSIGHT is the specific service type. CORTEX_CODE_CLI for desktop/CLI.

---

### Q35.
Fine-tuning cost is calculated as:

A) Number of training rows × credits per row  
B) Number of input tokens × number of epochs trained  
C) Number of parameters in the model  
D) Flat fee per fine-tuning job  

**Answer: B** — Fine-tuning cost = input tokens × epochs. Check with FINETUNE('DESCRIBE') for trained_tokens.

---

### Q36.
Where do you track fine-tuning training costs?

A) CORTEX_FUNCTIONS_USAGE_HISTORY  
B) CORTEX_FINE_TUNING_USAGE_HISTORY  
C) METERING_DAILY_HISTORY  
D) MODEL_REGISTRY_USAGE  

**Answer: B** — Dedicated view for fine-tuning training costs.

---

### Q37.
Provisioned Throughput is best for:

A) Development and testing  
B) Steady, high-volume production workloads needing guaranteed throughput  
C) One-time batch processing  
D) All workloads (cheaper than per-token)  

**Answer: B** — Reserved capacity for predictable high-volume workloads. Development should use per-token.

---

## ?? Section D: Observability and Evaluation (Q38-Q50)

### Q38.
The RAG Triad in TruLens consists of:

A) Precision, Recall, F1  
B) Context Relevance, Groundedness, Answer Relevance  
C) Accuracy, Latency, Cost  
D) Input Quality, Output Quality, Safety  

**Answer: B** — RAG Triad: (1) Context Relevance (context↔question), (2) Groundedness (answer↔context), (3) Answer Relevance (answer↔question).

---

### Q39.
"Groundedness" measures whether:

A) The question is valid  
B) The answer is supported by the provided context (not hallucinated)  
C) The search results are from reliable sources  
D) The model is properly fine-tuned  

**Answer: B** — Groundedness = is the answer grounded in context? Low groundedness = hallucination.

---

### Q40.
TruLens stores evaluation logs in Snowflake via:

A) Event tables automatically  
B) SnowflakeConnector that writes to Snowflake tables  
C) A dedicated Cortex service  
D) External S3 bucket  

**Answer: B** — `SnowflakeConnector(snowpark_session=session)` directs TruLens to write logs to Snowflake tables.

---

### Q41.
Context Relevance is LOW in a RAG app. What should you fix?

A) Change the LLM model  
B) Improve the Cortex Search service (better chunking, tune scoring weights)  
C) Lower the temperature  
D) Add more VQRs  

**Answer: B** — Low context relevance = search returning irrelevant chunks. Fix the retrieval (Search service), not the generation (LLM).

---

### Q42.
Groundedness is LOW. What should you fix?

A) Improve search results  
B) Strengthen the prompt to instruct LLM to answer ONLY from context (reduce hallucination)  
C) Use a different embedding model  
D) Add more training data  

**Answer: B** — Low groundedness = LLM generating answers beyond what's in the context. Fix with prompt engineering + lower temperature.

---

### Q43.
Cortex Agent evaluations use which framework?

A) RAG Triad  B) GPA (Goal-Plan-Action)  C) BLEU/ROUGE  D) sql_correctness  

**Answer: B** — Agents use GPA: tool selection accuracy, tool execution accuracy, answer correctness, logical consistency.

---

### Q44.
Cortex Analyst evaluations use which metric?

A) Groundedness  B) sql_correctness  C) BLEU score  D) Precision/Recall  

**Answer: B** — sql_correctness uses LLM-as-judge to compare generated SQL results against VQR ground truth.

---

### Q45.
During a Cortex Analyst evaluation, what happens to the VQRs being tested?

A) They are permanently removed  
B) They are temporarily removed so the model is tested without their guidance  
C) They are given extra weight  
D) Nothing — they remain active  

**Answer: B** — Selected VQRs are temporarily removed during evaluation. Analyst must generate SQL without that specific guidance. This tests whether the model can handle those questions independently.

---

### Q46.
Event tables capture which types of AI telemetry?

A) Only errors  
B) AI function calls, Cortex Search queries, Agent invocations, custom events  
C) Only billing data  
D) Only model output text  

**Answer: B** — Event tables capture comprehensive telemetry: function calls, search queries, agent invocations (tools called, reasoning steps), and custom application events.

---

### Q47.
What SQL creates an event table for AI observability?

A) `CREATE EVENT TABLE my_events;` then `ALTER ACCOUNT SET EVENT_TABLE = '...';`  
B) `ALTER ACCOUNT SET LOGGING = TRUE;`  
C) Event tables are created automatically  
D) `CREATE AUDIT TABLE my_events;`  

**Answer: A** — Create the event table object, then set it as the account's event table.

---

### Q48.
Which statement about TruLens feedback functions is TRUE?

A) They only work with OpenAI models  
B) They can use Snowflake Cortex models (via the Cortex provider)  
C) They require external API calls to third-party services  
D) They only evaluate text classification tasks  

**Answer: B** — TruLens has a Cortex provider: `Cortex(snowpark_session=session, model="llama3.1-70b")` — evaluations stay within Snowflake.

---

### Q49.
A CoWork agent shows poor quality. The admin needs to identify which tool the agent selected incorrectly. Which evaluation metric helps?

A) Groundedness  B) Tool selection accuracy (GPA framework)  C) sql_correctness  D) Context relevance  

**Answer: B** — Tool selection accuracy (part of GPA) measures if the agent chose the right tools for the question.

---

### Q50.
Which monitoring approach shows AI function telemetry alongside business metrics in a dashboard?

A) METERING_DAILY_HISTORY only  
B) Event tables (custom events + function calls) queried via SQL/Streamlit  
C) CORTEX_FUNCTIONS_USAGE_HISTORY only  
D) TruLens dashboard only  

**Answer: B** — Event tables provide the most comprehensive telemetry that can be combined with business metrics in custom dashboards.

---

## Answer Key

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | B | 11 | B | 21 | B | 31 | B | 41 | B |
| 2 | C | 12 | B | 22 | D | 32 | B | 42 | B |
| 3 | B | 13 | B | 23 | B | 33 | C | 43 | B |
| 4 | B | 14 | B | 24 | B | 34 | B | 44 | B |
| 5 | B | 15 | A | 25 | B | 35 | B | 45 | B |
| 6 | B | 16 | B | 26 | B | 36 | B | 46 | B |
| 7 | A,C | 17 | B | 27 | B | 37 | B | 47 | A |
| 8 | B | 18 | A | 28 | B | 38 | B | 48 | B |
| 9 | B | 19 | B | 29 | C | 39 | B | 49 | B |
| 10 | B | 20 | B | 30 | A | 40 | B | 50 | B |

