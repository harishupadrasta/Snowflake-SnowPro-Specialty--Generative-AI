# 📖 Glossary of Terms

[![Back to Main](https://img.shields.io/badge/← Back_to_Main-README-blue?style=flat-square)](README.md)

> **How to use this glossary:** This is your quick-reference dictionary for every technical term used in this study guide. Terms are organized alphabetically. Use `Ctrl+F` / `Cmd+F` to search for any keyword.

---

## A

| Term | Definition |
|------|-----------|
| **ACCOUNTADMIN** | The highest-privilege built-in role in Snowflake. Can manage all objects, billing, and account-level settings. |
| **Agent SDK** | Snowflake's Python SDK for building Cortex Agents programmatically with tool definitions and instructions. |
| **Agentic AI** | AI systems that can autonomously plan, use tools, and take actions to accomplish goals (vs. simple Q&A). |
| **AI_CLASSIFY** | Cortex function that classifies text into one or more categories from a provided list. |
| **AI_COMPLETE** | The core Cortex LLM function. Sends a prompt to a specified model and returns the generated text. |
| **AI_COUNT_TOKENS** | Utility function that counts the number of tokens a given text would consume for a specific model. |
| **AI_EMBED** | Generates a vector embedding (numerical representation) of input text using an embedding model. |
| **AI_EXTRACT** | Extracts structured key-value data from unstructured text based on specified fields. |
| **AI_FILTER** | Filters rows in a table based on a natural-language condition evaluated by an LLM. |
| **AI_MULTI_EMBED** | Generates embeddings for multiple inputs in a single call (batch embedding). |
| **AI_PARSE_DOCUMENT** | Extracts text content from documents (PDF, images) stored on a Snowflake stage. |
| **AI_REDACT** | Removes or masks sensitive information (PII) from text using an LLM. |
| **AI_SENTIMENT** | Analyzes text and returns a sentiment score from -1 (negative) to +1 (positive). |
| **AI_SIMILARITY** | Computes semantic similarity between two text inputs using embeddings. |
| **AI_SUMMARIZE_AGG** | Aggregate function that summarizes multiple rows of text into a single summary. |
| **AI_TRANSCRIBE** | Converts audio files to text transcription. |
| **AI_TRANSLATE** | Translates text from one language to another. |
| **Analytical Search** | A Cortex Search feature that returns aggregated/analytical results rather than individual documents. |
| **arctic-embed** | Snowflake's family of open-source text embedding models optimized for retrieval tasks. |
| **arctic-extract** | Snowflake's document extraction model that can be fine-tuned with custom Dataset objects. |
| **AUTO_SUSPEND** | Parameter for Cortex Search Service — minimum value is **1800 seconds** (30 minutes). |

## B

| Term | Definition |
|------|-----------|
| **Batch Search** | Calling Cortex Search over multiple queries in a single request for efficiency. |
| **BM25** | Best Matching 25 — a classical keyword-based ranking algorithm used in hybrid search. Scores documents by term frequency and inverse document frequency. |
| **BCR** | Behavior Change Release — Snowflake's mechanism for rolling out breaking changes with opt-in/opt-out periods. |

## C

| Term | Definition |
|------|-----------|
| **CHAT()** | SQL function that invokes a Cortex Agent, passing messages and receiving responses. |
| **Chunking** | Splitting large documents into smaller overlapping pieces for embedding and retrieval in RAG pipelines. |
| **CKE** | Cortex Knowledge Extensions — a Snowflake feature for deploying documentation search services. |
| **CoCo** | Cortex Code — Snowflake's AI-powered IDE/coding assistant (desktop and in-browser). |
| **Compute Pool** | A set of GPU or CPU nodes in Snowpark Container Services used to run containers. |
| **Context Window** | The maximum number of tokens an LLM can process in a single request (input + output combined). |
| **Cortex Agent** | An AI agent that can reason, call tools (Cortex Search, Analyst, SQL, Python), and orchestrate multi-step tasks. |
| **Cortex Analyst** | Natural-language-to-SQL engine powered by Semantic Views. Converts business questions to SQL. |
| **Cortex Guard** | A built-in safety classifier that detects harmful/unsafe content in LLM inputs and outputs. |
| **Cortex REST API** | HTTP endpoint for calling Cortex LLM functions from external applications (OpenAI-compatible format). |
| **Cortex Search** | Managed RAG retrieval service combining hybrid search (BM25 + semantic) with neural reranking. |
| **CORTEX_ENABLED_CROSS_REGION** | Account-level parameter that controls whether LLM inference can route to other cloud regions. |
| **CORTEX_MODELS_ALLOWLIST** | Account-level parameter that restricts which LLM models users can access. |
| **Cosine Similarity** | A measure of similarity between two vectors (embeddings) based on the angle between them. Ranges from -1 to 1. |
| **CoWork** | Snowflake's collaborative AI assistant in Snowsight (formerly called Snowflake Intelligence). Provides no-code data exploration, analysis, and natural-language interaction with your data. |
| **Cross-Region Inference** | Routing LLM requests to models hosted in different cloud regions to improve availability. |

## D

| Term | Definition |
|------|-----------|
| **Dataset (object)** | A Snowflake object used to package training data for fine-tuning (replaces raw table references for arctic-extract). |
| **Deep Research** | CoWork feature that performs multi-step autonomous research across your Snowflake data. |
| **Document AI** | Snowflake feature for extracting structured data from documents using AI models. |
| **DOCUMENT_INTELLIGENCE_CREATOR** | Role required to create Document AI models/pipelines. |
| **Dynamic Table** | A Snowflake table that automatically refreshes based on a defining query and a target lag. |

## E

| Term | Definition |
|------|-----------|
| **Embedding** | A dense numerical vector representation of text that captures its semantic meaning. Used for similarity search. |
| **Embedding Model** | An AI model (e.g., arctic-embed, e5-base-v2) that converts text into fixed-size vector representations. |
| **ENTITY_SENTIMENT** | Legacy Cortex function that extracts entities from text and assigns sentiment scores to each. |
| **Egress** | Data transfer costs when data moves out of a cloud region (relevant for cross-region inference). |

## F

| Term | Definition |
|------|-----------|
| **Few-Shot Learning** | Providing a few examples in the prompt to guide the model's behavior without fine-tuning. |
| **FILE (data type)** | Snowflake data type for referencing files stored on stages. Used with Document AI functions. |
| **Fine-Tuning** | Training a pre-existing model on your custom data to specialize its behavior. Uses `FINETUNE()` function in Snowflake. |
| **FLATTEN** | SQL function that expands arrays/objects into rows. Essential for parsing LLM JSON outputs. |

## G

| Term | Definition |
|------|-----------|
| **GA (Generally Available)** | A feature that has completed preview and is production-ready with SLA guarantees. |
| **GPU** | Graphics Processing Unit — specialized hardware for running ML model inference and training. |
| **Grounding / Groundedness** | The degree to which an LLM's response is supported by the retrieved context (not hallucinated). Part of the RAG Triad. |
| **Guardrails** | Safety mechanisms (input/output filters, Cortex Guard) that prevent LLMs from producing harmful or off-topic content. |

## H

| Term | Definition |
|------|-----------|
| **Hallucination** | When an LLM generates information that sounds plausible but is factually incorrect or not supported by the source data. |
| **Hybrid Search** | Combining keyword search (BM25) and semantic (vector) search for better retrieval quality. Used by Cortex Search. |
| **Hugging Face** | Open-source ML platform; Snowflake supports deploying Hugging Face models via SPCS. |

## I

| Term | Definition |
|------|-----------|
| **Image Repository** | A Snowflake-managed Docker registry for storing container images used by SPCS. |
| **Inference** | The process of running a trained model on new inputs to generate predictions or responses. |
| **Instance Family** | Categories of SPCS compute pool hardware (GPU_NV_S, GPU_NV_M, GPU_NV_L) with different GPU sizes. |

## J

| Term | Definition |
|------|-----------|
| **JSON Mode** | A structured output mode where the LLM is constrained to return valid JSON. Set via `response_format`. |
| **JWT** | JSON Web Token — used for authentication with the Cortex REST API (key-pair auth). |

## K

| Term | Definition |
|------|-----------|
| **Keyword Search** | Traditional text retrieval using exact/stemmed word matching (BM25). Complements semantic search. |

## L

| Term | Definition |
|------|-----------|
| **LLM** | Large Language Model — a neural network trained on massive text data to understand and generate language (e.g., GPT-4, Llama, Mistral). |
| **LLM-as-Judge** | Using one LLM to evaluate the quality of another LLM's outputs (used in Cortex Analyst evaluations). |
| **log_model** | Python API for registering ML models into Snowflake's Model Registry. |
| **LoRA** | Low-Rank Adaptation — a parameter-efficient fine-tuning method that only trains small adapter weights. |

## M

| Term | Definition |
|------|-----------|
| **MCP** | Model Context Protocol — an open standard for connecting AI agents to external tools and data sources. |
| **Model Registry** | Snowflake's catalog for versioning, deploying, and managing ML models. |
| **Multimodal** | AI models that can process multiple input types (text, images, audio). |
| **MoE** | Mixture of Experts — model architecture where only a subset of parameters activate per input (e.g., Mixtral). |

## N

| Term | Definition |
|------|-----------|
| **Native App** | A Snowflake application package that can be distributed via Marketplace and runs in the consumer's account. |
| **Neural Reranking** | Using a trained model to re-score and re-order search results for better relevance after initial retrieval. |

## O

| Term | Definition |
|------|-----------|
| **OCR** | Optical Character Recognition — extracting text from images/scanned documents. Used by AI_PARSE_DOCUMENT. |
| **Owner's Rights** | Cortex Search services run with the privileges of the service creator (owner), not the caller. |

## P

| Term | Definition |
|------|-----------|
| **PAT** | Programmatic Access Token — a token for authenticating API calls to Snowflake without interactive login. |
| **PEFT** | Parameter-Efficient Fine-Tuning — techniques (like LoRA) that fine-tune only a small number of model parameters. |
| **Per-Token Billing** | Cortex AI charges based on the number of input + output tokens processed. |
| **Prompt Caching** | REST API feature that caches repeated prompt prefixes to reduce latency and cost. OpenAI: auto for 1024+ tokens; Anthropic: 5-min TTL, max 4 breakpoints. |
| **Prompt Engineering** | Designing and optimizing prompts to get better results from LLMs without changing the model. |
| **Provisioned Throughput** | Reserved, dedicated model capacity for consistent performance (vs. serverless on-demand). |

## R

| Term | Definition |
|------|-----------|
| **RAG** | Retrieval-Augmented Generation — a pattern where relevant documents are retrieved first, then fed to an LLM as context to generate grounded answers. Reduces hallucination. |
| **RAG Triad** | Three evaluation metrics for RAG quality: **Context Relevance** (are retrieved docs relevant?), **Groundedness** (is the answer supported by context?), **Answer Relevance** (does the answer address the question?). |
| **RBAC** | Role-Based Access Control — Snowflake's permission model where privileges are granted to roles, and roles are granted to users. |
| **Reciprocal Rank Fusion** | A technique for combining results from multiple retrieval methods (keyword + semantic) into a single ranked list. |
| **Reranking** | A second-pass scoring step that uses a more expensive model to re-order initial retrieval results by relevance. |
| **REST API** | Representational State Transfer API — HTTP-based interface for accessing Cortex functions from external apps. |

## S

| Term | Definition |
|------|-----------|
| **Scoring Config** | JSON configuration in Cortex Search that sets weights between keyword (BM25) and semantic search components. |
| **Semantic View** | A metadata layer on top of tables that defines business metrics, dimensions, and relationships. Powers Cortex Analyst. |
| **Serverless** | Snowflake-managed compute that auto-scales without user-managed warehouses (used by Cortex AI, Dynamic Tables, Cortex Search). |
| **SPCS** | Snowpark Container Services — Snowflake's platform for running custom Docker containers (including ML models) with GPU support. |
| **Stage** | A Snowflake storage location (internal or external) for files. Used to store documents, model artifacts, and container specs. |
| **Stemming** | Reducing words to their root form (e.g., "running" → "run") for better keyword matching. |
| **Streamlit** | Python framework for building interactive data apps. Streamlit in Snowflake (SiS) runs natively inside Snowsight. |
| **Structured Output** | Constraining LLM responses to follow a specific schema (JSON mode or response_format). |

## T

| Term | Definition |
|------|-----------|
| **Temperature** | A parameter (0.0–1.0) controlling randomness in LLM outputs. Lower = more deterministic, higher = more creative. |
| **Tokens** | The basic units of text that LLMs process. Roughly ~4 characters or ~¾ of a word in English. |
| **Tool Calling** | An LLM capability where the model can request execution of external functions (search, SQL, APIs) as part of its reasoning. |
| **TPM / RPM** | Tokens Per Minute / Requests Per Minute — rate limits for Cortex AI functions. |
| **TruLens** | Open-source framework (by Snowflake/TruEra) for evaluating and monitoring LLM/RAG application quality. |

## U

| Term | Definition |
|------|-----------|
| **UDF** | User-Defined Function — custom function written in SQL, Python, Java, or Scala that runs in Snowflake. |
| **UDTF** | User-Defined Table Function — a UDF that returns multiple rows (a table) instead of a scalar value. |
| **USE AI FUNCTIONS** | A database-level privilege required (in addition to role grants) for users to call Cortex AI functions. |

## V

| Term | Definition |
|------|-----------|
| **Vector** | A fixed-size array of numbers representing the semantic meaning of text. Used for similarity search. |
| **VECTOR (data type)** | Snowflake's native data type for storing embedding vectors. Supports cosine, L1, L2 distance operations. |
| **Vector Search** | Finding similar items by computing distance between embedding vectors. |
| **Verified Query Representations (VQR)** | Pre-validated SQL examples in a Semantic View that Cortex Analyst uses as reference patterns. |
| **vLLM** | An open-source high-performance LLM serving framework. Can be deployed on SPCS. |

## W

| Term | Definition |
|------|-----------|
| **Warehouse** | Snowflake's compute resource for running SQL queries. Cortex AI functions use serverless compute (no warehouse needed) unless using Streamlit. |
| **Web Search Tool** | A Cortex Agent tool that searches the internet for real-time information (via Brave Search). |

---

## 🎯 Exam-Critical Terms

> These terms appear most frequently in exam questions. Know them cold.

| # | Term | Why It Matters |
|---|------|---------------|
| 1 | **RAG** | Core architecture pattern — understand the full pipeline |
| 2 | **BM25** | Keyword scoring in Cortex Search hybrid search |
| 3 | **Semantic View** | Powers Cortex Analyst — know structure (metrics, dimensions, facts) |
| 4 | **VQR** | Critical for Analyst accuracy — know the format and purpose |
| 5 | **CORTEX_USER** | Default role for AI function access — know the privilege hierarchy |
| 6 | **AUTO_SUSPEND** | Minimum 1800s for Cortex Search — classic exam trap |
| 7 | **Cross-Region Inference** | Know the parameter name and cost implications |
| 8 | **Owner's Rights** | Cortex Search runs as owner — security implication |
| 9 | **Fine-Tuning** | Know which models support it and the column requirements |
| 10 | **Prompt Caching** | Know differences between OpenAI and Anthropic approaches |
| 11 | **SPCS** | Know compute pools, image repos, service functions |
| 12 | **Cortex Guard** | Know input vs output filtering use cases |
| 13 | **Document AI** | Know DOCUMENT_INTELLIGENCE_CREATOR role requirement |
| 14 | **Tool Calling** | How agents invoke external functions — Plan→Tools→Reflect loop |
| 15 | **TruLens / RAG Triad** | Three metrics for evaluating RAG quality |

---

## 📝 Quick Reference: Role Hierarchy

| Role | What It Grants |
|------|---------------|
| `CORTEX_USER` | All standard Cortex AI functions |
| `AI_FUNCTIONS_USER` | Same as CORTEX_USER (legacy name) |
| `CORTEX_ANALYST_USER` | Cortex Analyst access |
| `CORTEX_AGENT_USER` | Cortex Agent creation and use |
| `CORTEX_EMBED_USER` | Embedding functions only |
| `CORTEX_REST_API_USER` | REST API endpoint access |
| `COPILOT_USER` | CoWork access (legacy role name from when the product was called Copilot) |
| `DOCUMENT_INTELLIGENCE_CREATOR` | Document AI model creation |

---

## 📝 Quick Reference: Function Categories

| Category | Functions |
|----------|-----------|
| **Generation** | AI_COMPLETE, CHAT, TRY_COMPLETE |
| **Task-Specific** | AI_CLASSIFY, AI_EXTRACT, AI_FILTER, AI_SENTIMENT, AI_SUMMARIZE_AGG, AI_TRANSLATE, AI_TRANSCRIBE, AI_REDACT |
| **Embedding** | AI_EMBED, AI_MULTI_EMBED, AI_SIMILARITY |
| **Document** | AI_PARSE_DOCUMENT, SPLIT_TEXT_RECURSIVE_CHARACTER, SPLIT_TEXT_MARKDOWN_HEADER |
| **Utility** | AI_COUNT_TOKENS, CORTEX_GUARD, PROMPT |
| **Aggregate** | AI_AGG, AI_SUMMARIZE_AGG |

---

[![Back to Main](https://img.shields.io/badge/← Back_to_Main-README-blue?style=flat-square)](README.md)
