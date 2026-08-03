<h1 align="center">🔒 Domain 3: Snowflake Gen AI Governance</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Weight-29%25-purple?style=for-the-badge" alt="29%"/>
  <img src="https://img.shields.io/badge/Questions-~17-green?style=for-the-badge" alt="~17 questions"/>
  <img src="https://img.shields.io/badge/Focus-Access_&_Cost-orange?style=for-the-badge" alt="Access & Cost"/>
</p>

---

> 🔐 **Focus:** How to control, monitor, and govern AI usage — RBAC, model access, cost tracking, and observability.

---

## 📚 Topics

| | # | Topic | File |
|---|---|-------|------|
| 🔑 | 3.1 | Model Access Controls | [3.1_Model_Access_Controls.md](./3.1_Model_Access_Controls.md) |
| 🛡️ | 3.1a | Guardrails & Advanced Governance | [3.1a_Guardrails_and_Advanced_Governance.md](./3.1a_Guardrails_and_Advanced_Governance.md) |
| 👥 | 3.2 | RBAC and Privileges | [3.2_RBAC_and_Privileges.md](./3.2_RBAC_and_Privileges.md) |
| 💰 | 3.3 | Cost Management & Monitoring | [3.3_Cost_Management_and_Monitoring.md](./3.3_Cost_Management_and_Monitoring.md) |
| 👁️ | 3.4 | AI Observability | [3.4_AI_Observability.md](./3.4_AI_Observability.md) |

---

## 🎯 Scenarios & Quiz

| | Resource | File |
|---|----------|------|
| 📋 | Scenarios & Governance Decisions | [Scenarios_Domain3.md](./Scenarios_Domain3.md) |
| ✅ | Quiz (50 Questions) | [quiz/Questions_Domain3.md](./quiz/Questions_Domain3.md) |

---

## 👥 RBAC Roles Quick Reference

```
┌─────────────────────────────────────────────────────────────────────┐
│  ROLE                          │ ACCESS                │ DEFAULT?   │
├────────────────────────────────┼───────────────────────┼────────────┤
│  SNOWFLAKE.CORTEX_USER         │ ALL Cortex features   │ ✅ PUBLIC  │
│  SNOWFLAKE.AI_FUNCTIONS_USER   │ Scalar AI functions   │ ❌         │
│  SNOWFLAKE.CORTEX_ANALYST_USER │ Analyst only          │ ❌         │
│  SNOWFLAKE.CORTEX_AGENT_USER   │ Agents only           │ ❌         │
│  SNOWFLAKE.CORTEX_EMBED_USER   │ Embeddings + Search   │ ❌         │
│  SNOWFLAKE.CORTEX_REST_API_USER│ REST API only         │ ❌         │
│  SNOWFLAKE.COPILOT_USER        │ Code in Snowsight     │ ❌         │
└────────────────────────────────┴───────────────────────┴────────────┘
```

---

## ⚠️ Key Exam Traps

> ❌ **Trap:** Users only need CORTEX_USER to use AI functions  
> ✅ **Truth:** Need BOTH `USE AI FUNCTIONS` privilege AND a database role

> ❌ **Trap:** RBAC and allowlist use AND logic  
> ✅ **Truth:** Access if **EITHER** permits it (OR relationship)

> ❌ **Trap:** REST API uses the session role  
> ✅ **Truth:** REST API uses the user's **DEFAULT ROLE**

> ❌ **Trap:** AI_COUNT_TOKENS has token charges  
> ✅ **Truth:** Only compute cost — no token billing
