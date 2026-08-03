<h1 align="center">⚡ Domain 2: Snowflake Gen AI Functions</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Weight-38%25-red?style=for-the-badge" alt="38%"/>
  <img src="https://img.shields.io/badge/Questions-~23-green?style=for-the-badge" alt="~23 questions"/>
  <img src="https://img.shields.io/badge/THE_LARGEST_DOMAIN-⚠️-yellow?style=for-the-badge" alt="Largest"/>
</p>

---

> ⚠️ **This is the LARGEST domain at 38%.** You must know every function's syntax, parameters, return types, and appropriate use cases.

---

## 📚 Topics

| | # | Topic | File |
|---|---|-------|------|
| 🧠 | 2.1 | AI_COMPLETE and General Functions | [2.1_AI_Complete_and_General_Functions.md](./2.1_AI_Complete_and_General_Functions.md) |
| 🏷️ | 2.2 | Task-Specific Functions | [2.2_Task_Specific_Functions.md](./2.2_Task_Specific_Functions.md) |
| 📐 | 2.3 | Vector Functions | [2.3_Vector_Functions.md](./2.3_Vector_Functions.md) |
| 🔧 | 2.4 | Helper Functions | [2.4_Helper_Functions.md](./2.4_Helper_Functions.md) |
| 📈 | 2.5 | Data Analysis Patterns | [2.5_Data_Analysis_Patterns.md](./2.5_Data_Analysis_Patterns.md) |
| 💬 | 2.6 | Chat Interfaces and Streamlit | [2.6_Chat_Interfaces_and_Streamlit.md](./2.6_Chat_Interfaces_and_Streamlit.md) |
| 🔄 | 2.7 | Cortex in Data Pipelines | [2.7_Cortex_in_Data_Pipelines.md](./2.7_Cortex_in_Data_Pipelines.md) |
| 🐳 | 2.8 | Third-Party Models (SPCS) | [2.8_Third_Party_Models_SPCS.md](./2.8_Third_Party_Models_SPCS.md) |
| 🌐 | 2.8a | REST API and Fine-Tuning Deep Dive | [2.8a_REST_API_and_Fine_Tuning.md](./2.8a_REST_API_and_Fine_Tuning.md) |
| ⚙️ | 2.9 | Performance and Model Selection | [2.9_Performance_Considerations.md](./2.9_Performance_Considerations.md) |

---

## 🎯 Scenarios & Quiz

| | Resource | File |
|---|----------|------|
| 📋 | Scenarios & Function Selection Guide | [Scenarios_Domain2.md](./Scenarios_Domain2.md) |
| ✅ | Quiz (60 Questions) | [quiz/Questions_Domain2.md](./quiz/Questions_Domain2.md) |

---

## 📝 Function Quick Reference

### 🧠 Generative (Input + Output Tokens Billed)

| Function | What It Does | Returns |
|----------|-------------|---------|
| `AI_COMPLETE` | General completion (text/image/audio/video) | VARCHAR or JSON |
| `AI_CLASSIFY` | Classify into categories | VARCHAR (label) |
| `AI_EXTRACT` | Extract structured fields | OBJECT (JSON) |
| `AI_FILTER` | Boolean condition check | BOOLEAN |
| `AI_AGG` | Aggregate insight across rows | VARCHAR |
| `AI_SENTIMENT` | Sentiment analysis | FLOAT or OBJECT |
| `AI_SUMMARIZE_AGG` | Multi-row summary | VARCHAR |
| `SUMMARIZE` | Single-text summary | VARCHAR |
| `AI_TRANSLATE` | Translation | VARCHAR |
| `AI_REDACT` | PII removal | VARCHAR |
| `AI_TRANSCRIBE` | Speech-to-text | OBJECT (JSON) |

### 📐 Embedding (Input Tokens Only)

| Function | Returns | Use Case |
|----------|---------|----------|
| `AI_EMBED` | VECTOR | Text/image embeddings |
| `AI_MULTI_EMBED` | OBJECT | Video embeddings |
| `AI_SIMILARITY` | FLOAT | Similarity score |

### 🔧 Helpers (Compute Only / Free)

| Function | Returns | Cost |
|----------|---------|------|
| `AI_COUNT_TOKENS` | INT | Compute only |
| `SPLIT_TEXT_RECURSIVE_CHARACTER` | ARRAY | Compute only |
| `TO_FILE` | FILE | Free |
| `PROMPT` | OBJECT | Free |
| `TRY_COMPLETE` | VARCHAR/NULL | Same as AI_COMPLETE |

---

## ⚠️ Key Exam Traps

> ❌ **Trap:** AI_SENTIMENT always returns a float  
> ✅ **Truth:** Without entities → float. **With entities array → OBJECT with categories**

> ❌ **Trap:** AI_AGG is limited by context window  
> ✅ **Truth:** AI_AGG has **no context window limit** — processes in batches

> ❌ **Trap:** AI_COUNT_TOKENS has token-based charges  
> ✅ **Truth:** Only **compute cost** (warehouse) — no token charges

> ❌ **Trap:** response_format is just a suggestion to the model  
> ✅ **Truth:** It **guarantees** schema compliance — every token validated

> ❌ **Trap:** Fine-tuned models work across regions  
> ✅ **Truth:** Cross-region inference does **NOT** support fine-tuned models
