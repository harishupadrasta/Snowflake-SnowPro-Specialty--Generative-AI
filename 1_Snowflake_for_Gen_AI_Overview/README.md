<h1 align="center">🏗️ Domain 1: Snowflake for Gen AI Overview</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Weight-18%25-blue?style=for-the-badge" alt="18%"/>
  <img src="https://img.shields.io/badge/Questions-~11-green?style=for-the-badge" alt="~11 questions"/>
  <img src="https://img.shields.io/badge/Topics-12_files-orange?style=for-the-badge" alt="12 topics"/>
</p>

---

> 💡 **Focus:** This domain tests your understanding of the Snowflake Cortex AI ecosystem — what each product does, how they relate, and when to use which component.

---

## 📚 Topics

| | # | Topic | File |
|---|---|-------|------|
| 🌐 | 1.1 | Snowflake Cortex Overview | [1.1_Snowflake_Cortex_Overview.md](./1.1_Snowflake_Cortex_Overview.md) |
| 🔍 | 1.2 | Cortex Search (Complete Reference) | [1.2_Cortex_Search.md](./1.2_Cortex_Search.md) |
| 🔍 | 1.2a | Cortex Search Advanced | [1.2a_Cortex_Search_Advanced.md](./1.2a_Cortex_Search_Advanced.md) |
| 📊 | 1.3 | Cortex Analyst & Semantic Views | [1.3_Cortex_Analyst.md](./1.3_Cortex_Analyst.md) |
| 🤖 | 1.4 | Cortex Agents | [1.4_Cortex_Agents.md](./1.4_Cortex_Agents.md) |
| 💬 | 1.5 | CoWork (formerly Snowflake Intelligence) | [1.5_Snowflake_Intelligence.md](./1.5_Snowflake_Intelligence.md) |
| 💻 | 1.6 | Cortex Code | [1.6_Cortex_Code.md](./1.6_Cortex_Code.md) |
| 🧩 | 1.6a | CoCo, CoWork, Skills & Plugins | [1.6a_CoCo_CoWork_Skills_Plugins.md](./1.6a_CoCo_CoWork_Skills_Plugins.md) |
| 🌍 | 1.7 | Cross-Region Inference | [1.7_Cross_Region_Inference.md](./1.7_Cross_Region_Inference.md) |
| 🐳 | 1.8 | Model Registry and SPCS | [1.8_Model_Registry_and_SPCS.md](./1.8_Model_Registry_and_SPCS.md) |
| 🐳 | 1.8a | SPCS Deep Dive | [1.8a_SPCS_Deep_Dive.md](./1.8a_SPCS_Deep_Dive.md) |
| 🔌 | 1.9 | MCP and Knowledge Extensions | [1.9_MCP_and_CKE.md](./1.9_MCP_and_CKE.md) |

---

## 🎯 Scenarios & Quiz

| | Resource | File |
|---|----------|------|
| 📋 | Scenarios & Decision Guide | [Scenarios_Domain1.md](./Scenarios_Domain1.md) |
| ✅ | Quiz (50 Questions) | [quiz/Questions_Domain1.md](./quiz/Questions_Domain1.md) |

---

## 🧭 Quick Decision Matrix

> **"Which product do I use?"** — The most common exam question pattern

```
┌─────────────────────────────────────────────────────────────────┐
│  🎯 USER NEED                │  ✅ USE THIS                      │
├──────────────────────────────┼───────────────────────────────────┤
│  Search unstructured docs    │  🔍 Cortex Search (RAG)           │
│  Query databases in NL       │  📊 Cortex Analyst (Semantic View)│
│  Both + reasoning            │  🤖 Cortex Agents (orchestrates)  │
│  No-code chat for users      │  💬 CoWork (ai.snowflake.com)     │
│  Help me write SQL           │  💻 Cortex Code                   │
│  Deploy custom model         │  🐳 SPCS + Model Registry         │
│  Access frontier models      │  🌍 Cross-Region Inference        │
│  Connect to Jira/Salesforce  │  🔌 MCP Connectors                │
│  Pre-built knowledge         │  📦 CKE from Marketplace          │
└──────────────────────────────┴───────────────────────────────────┘
```

---

## ⚠️ Key Exam Traps

> ❌ **Trap:** ORGADMIN can set CORTEX_ENABLED_CROSS_REGION  
> ✅ **Truth:** Only **ACCOUNTADMIN** can set it (account-level only)

> ❌ **Trap:** Larger warehouse = faster AI functions  
> ✅ **Truth:** **MEDIUM max** — larger doesn't improve performance

> ❌ **Trap:** Cross-region stores data at the processing region  
> ✅ **Truth:** Data transits **transiently** and is **NOT persisted**

> ❌ **Trap:** SEARCH_PREVIEW is for production queries  
> ✅ **Truth:** Testing only — 300KB limit, higher latency, no batch
