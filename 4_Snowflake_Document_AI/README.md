<h1 align="center">📄 Domain 4: Snowflake Document Processing</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Weight-15%25-teal?style=for-the-badge" alt="15%"/>
  <img src="https://img.shields.io/badge/Questions-~9-green?style=for-the-badge" alt="~9 questions"/>
  <img src="https://img.shields.io/badge/Focus-Parse_&_Extract-orange?style=for-the-badge" alt="Parse & Extract"/>
</p>

---

> 📑 **Focus:** Parsing, extracting, and processing documents using Snowflake's AI functions and pipeline patterns.

---

## 📚 Topics

| | # | Topic | File |
|---|---|-------|------|
| 📑 | 4.1 | Document Parsing Functions | [4.1_Document_Parsing_Functions.md](./4.1_Document_Parsing_Functions.md) |
| 📁 | 4.2 | Document Preparation & Management | [4.2_Document_Preparation_and_Management.md](./4.2_Document_Preparation_and_Management.md) |
| ⚙️ | 4.3 | Automated Document Pipelines | [4.3_Automated_Document_Pipelines.md](./4.3_Automated_Document_Pipelines.md) |
| 🔧 | 4.4 | Troubleshooting & Optimization | [4.4_Troubleshooting_and_Optimization.md](./4.4_Troubleshooting_and_Optimization.md) |

---

## 🎯 Scenarios & Quiz

| | Resource | File |
|---|----------|------|
| 📋 | Scenarios & Pipeline Decisions | [Scenarios_Domain4.md](./Scenarios_Domain4.md) |
| ✅ | Quiz (40 Questions) | [quiz/Questions_Domain4.md](./quiz/Questions_Domain4.md) |

---

## 🔀 Document Processing Decision Tree

```
📄 What do you need from the document?
│
├── 📝 Full text content (for search/RAG)?
│   └── ✅ AI_PARSE_DOCUMENT (OCR or LAYOUT mode)
│
├── 🏷️ Specific structured fields (vendor, amount, date)?
│   └── ✅ AI_EXTRACT (with JSON schema)
│
├── ❓ Answer questions ABOUT the document?
│   └── ✅ AI_COMPLETE with TO_FILE (multimodal)
│
├── 🏷️ Classify document type?
│   └── ✅ AI_CLASSIFY with TO_FILE
│
└── 🖥️ Visual pipeline with Snowsight UI training?
    └── ✅ Document AI pipeline (!PREDICT method)
```

---

## ⚠️ Key Exam Traps

> ❌ **Trap:** SNOWFLAKE_FULL encryption works with AI functions  
> ✅ **Truth:** SNOWFLAKE_FULL is client-side — **NOT supported**. Use SNOWFLAKE_SSE

> ❌ **Trap:** Filenames on stages are case-insensitive  
> ✅ **Truth:** **CASE-SENSITIVE** — #1 cause of "file not found" errors

> ❌ **Trap:** LAYOUT mode is more expensive than OCR  
> ✅ **Truth:** Both modes bill at **970 tokens/page** — same cost

> ❌ **Trap:** Document AI and AI_PARSE_DOCUMENT are the same  
> ✅ **Truth:** Document AI is a Snowsight UI pipeline feature. AI_PARSE_DOCUMENT is a SQL function.
