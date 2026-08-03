# Domain 1: Scenarios and Decision Guide

> **When to use what** — Real-world scenarios mapped to Cortex products and configurations

---

## Scenario 1: "We need a chatbot for our HR policies"

**Situation:** HR department wants employees to ask questions about policies (vacation, benefits, expenses) stored as PDFs.

**Decision Flow:**
```
PDFs on stage → AI_PARSE_DOCUMENT (LAYOUT mode) 
    → SPLIT_TEXT_RECURSIVE_CHARACTER (512 tokens, 50 overlap)
    → CREATE CORTEX SEARCH SERVICE (ON parsed_text, ATTRIBUTES department)
    → CREATE AGENT with cortex_search tool
    → Deploy to CoWork for employees
```

**Why not AI_COMPLETE alone?** Without retrieval, the LLM would hallucinate policies. RAG grounds answers in your actual documents.

**Why not Cortex Analyst?** Policies are unstructured text, not tables. Analyst is for structured data queries.

---

## Scenario 2: "Sales team wants to ask 'What was Q4 revenue by region?'"

**Situation:** Business users want natural language access to sales data warehouse.

**Decision Flow:**
```
fact_orders + dim_customers + dim_products
    → CREATE SEMANTIC VIEW (tables, relationships, metrics, dimensions)
    → Add VQRs for common questions
    → CREATE AGENT with cortex_analyst_text_to_sql tool
    → Deploy to CoWork
```

**Why Semantic View?** Maps business terminology ("revenue") to physical columns (SUM(order_amount)). Without it, LLM doesn't know your schema.

**Why not Cortex Search?** Data is structured (tables with numbers). Search is for unstructured text.

---

## Scenario 3: "We need both structured AND unstructured answers"

**Situation:** Customer support team needs to query order data AND search knowledge base articles.

**Decision Flow:**
```
CREATE CORTEX SEARCH SERVICE for knowledge_base articles
    + CREATE SEMANTIC VIEW for orders/customers tables
    + CREATE AGENT with BOTH tools:
        - cortex_analyst_text_to_sql (for order queries)
        - cortex_search (for article lookup)
    → Agent automatically routes questions to correct tool
```

**Why an Agent?** It orchestrates between tools. "What's my order status?" → Analyst. "What's your return policy?" → Search.

---

## Scenario 4: "Need to access Claude/GPT models but we're in EU"

**Situation:** Account in AWS EU Frankfurt. Need claude-sonnet-4-6 for a project.

**Solution:**
```sql
-- Only ACCOUNTADMIN can do this:
ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'AWS_EU';
-- Or for maximum access:
ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'ANY_REGION';
```

**Key facts:**
- Data stays stored in Frankfurt
- Only prompt/response transits (encrypted, not persisted at processing region)
- No egress charges
- Credits charged in your region (Frankfurt)

**Common exam trap:** ORGADMIN CANNOT set this. Only ACCOUNTADMIN. It's account-level only (not session/user).

---

## Scenario 5: "Restrict data scientists to specific models only"

**Situation:** Finance wants to limit which LLMs can process their data.

**Solution (RBAC approach — recommended):**
```sql
USE ROLE ACCOUNTADMIN;

-- Block all models via allowlist
ALTER ACCOUNT SET CORTEX_MODELS_ALLOWLIST = 'None';

-- Grant specific models to specific roles
GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-LLAMA3.1-70B" TO ROLE finance_role;
GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-MISTRAL-LARGE2" TO ROLE finance_role;

-- Data science team gets additional access
GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-CLAUDE-SONNET-4-6" TO ROLE ds_role;
```

**Why RBAC over allowlist?** Allowlist is account-wide (all-or-nothing). RBAC is per-role. Allowlist being deprecated Aug 2026.

**Exam trap:** Access granted if EITHER allowlist OR RBAC permits (OR relationship, not AND).

---

## Scenario 6: "Deploy a custom fine-tuned Hugging Face model"

**Situation:** Team fine-tuned a model externally, need it served in Snowflake.

**When to use what:**

| Approach | When |
|----------|------|
| **Cortex Fine-Tuning** | Fine-tune FROM a supported base model (llama, mistral) on your data |
| **Model Registry** | Deploy sklearn/XGBoost/simple models as UDFs |
| **SPCS** | Deploy custom Docker containers (Hugging Face, vLLM, custom PyTorch) |

**For this scenario → SPCS:**
```sql
-- 1. Create image repo + push Docker image
CREATE IMAGE REPOSITORY my_repo;
-- docker push <org>-<acct>.registry.snowflakecomputing.com/db/schema/my_repo/vllm:v1

-- 2. Create GPU compute pool
CREATE COMPUTE POOL gpu_pool MIN_NODES=1 MAX_NODES=2 INSTANCE_FAMILY=GPU_NV_M;

-- 3. Deploy service
CREATE SERVICE llm_service IN COMPUTE POOL gpu_pool FROM SPECIFICATION $$
spec:
  containers:
    - name: llm
      image: /db/schema/my_repo/vllm:v1
      resources:
        requests:
          nvidia.com/gpu: 1
  endpoints:
    - name: predict
      port: 8000
$$;
```

---

## Scenario 7: "Make our agent accessible to external applications"

**Situation:** A React app needs to call your Cortex Agent via HTTP.

**Two approaches:**

| Method | When to Use |
|--------|-------------|
| **MCP Server** | Expose agent to MCP-compatible clients (CoCo, other agents) |
| **REST API (Agent:run)** | Direct HTTP integration from any application |

**For a React app → REST API:**
```
POST /api/v2/databases/{db}/schemas/{schema}/agents/{name}:run
Authorization: Bearer <PAT>

{
  "messages": [{"role": "user", "content": [{"type": "text", "text": "What's Q4 revenue?"}]}],
  "thread_id": "session-123"
}
```

**For another AI agent → MCP Server:**
```sql
CREATE MCP SERVER my_mcp_server FROM SPECIFICATION $$
tools:
  - tool_spec:
      type: "CORTEX_AGENT_RUN"
      name: "analytics_agent"
tool_resources:
  analytics_agent:
    agent: "db.schema.my_agent"
$$;
```

---

## Scenario 8: "Cortex Code won't work for our developers"

**Diagnosis checklist:**
1. ❓ Is cross-region inference enabled? → `SHOW PARAMETERS LIKE 'CORTEX_ENABLED_CROSS_REGION' IN ACCOUNT;`
2. ❓ Does user have COPILOT_USER role? → `GRANT DATABASE ROLE SNOWFLAKE.COPILOT_USER TO ROLE dev_role;`
3. ❓ Is the value DISABLED? → Must be anything OTHER than DISABLED

**Fix:**
```sql
ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'ANY_REGION';
GRANT DATABASE ROLE SNOWFLAKE.COPILOT_USER TO ROLE developer_role;
```

---

## Scenario 9: "Pre-built knowledge base for Snowflake docs"

**Situation:** Want agents to answer questions about Snowflake features using official documentation.

**Solution: Cortex Knowledge Extension (CKE)**
```sql
-- Install from Marketplace
CREATE DATABASE SNOWFLAKE_DOCUMENTATION FROM LISTING 'GZSTZ67BY9OQ4';

-- Add to agent as search tool
-- In agent specification:
tool_resources:
  DocSearch:
    name: "SNOWFLAKE_DOCUMENTATION.SHARED.CKE_SNOWFLAKE_DOCS_SERVICE"
    max_results: "5"
```

**What is a CKE?** A pre-built Cortex Search service shared via Marketplace. Contains curated, indexed content ready to query.

---

## Scenario 10: "Connect our agent to Jira for ticket management"

**Situation:** Agent should be able to create/update Jira tickets.

**Solution: MCP Connector (outbound)**
```sql
CREATE EXTERNAL MCP SERVER jira_connector
  WITH DISPLAY_NAME = 'Atlassian Jira'
  URL = 'https://mcp.atlassian.com/v1/mcp'
  API_INTEGRATION = jira_api_integration;
```

Then add to agent:
```yaml
tools:
  - tool_spec:
      type: "mcp_servers"
      name: "jira"
tool_resources:
  jira:
    mcp_server: "db.schema.jira_connector"
```

**Key distinction:**
- `CREATE MCP SERVER` = **expose Snowflake TO** external clients (inbound)
- `CREATE EXTERNAL MCP SERVER` = **connect FROM Snowflake TO** external tools (outbound)

---

## Quick Decision Matrix (Print This!)

```
┌──────────────────────────────────────────────────────────────────────┐
│  WHAT DO YOU NEED?           │  USE THIS                             │
├──────────────────────────────┼───────────────────────────────────────┤
│  Search documents            │  Cortex Search                        │
│  Query databases in NL       │  Cortex Analyst + Semantic View       │
│  Both + reasoning            │  Cortex Agent                         │
│  No-code chat for users      │  CoWork (ai.snowflake.com)            │
│  Chat in Microsoft Teams     │  Teams Integration                    │
│  Write/debug SQL             │  Cortex Code                          │
│  Build agentic app (code)    │  Cortex Code Agent SDK                │
│  Pre-built knowledge         │  CKE from Marketplace                 │
│  Connect to Jira/Salesforce  │  MCP Connector (EXTERNAL)             │
│  Expose to external clients  │  MCP Server                           │
│  Classify/extract/summarize  │  AI Functions (AI_CLASSIFY, etc.)     │
│  Custom model deployment     │  SPCS + Model Registry                │
│  Fine-tune existing model    │  CORTEX.FINETUNE                      │
│  Process documents at scale  │  AI_PARSE_DOCUMENT + pipeline         │
│  Real-time inference API     │  Cortex REST API                      │
│  Scheduled reports           │  CoWork Automations                   │
│  Complex multi-step analysis │  CoWork Deep Research                 │
│  Reusable agent capability   │  Skills (SKILL.md on stage)           │
│  Share skills across org     │  Cortex Extensions / Plugins Catalog  │
└──────────────────────────────┴───────────────────────────────────────┘
```

---

*Use this guide alongside the detailed topic notes to answer scenario-based exam questions.*

---

<p align="center">
  <a href="./README.md">🏠 Domain Home</a> &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="../README.md">📚 Main Home</a>
</p>
