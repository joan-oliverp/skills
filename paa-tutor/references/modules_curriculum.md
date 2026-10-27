# PAA Master Curriculum: Stages, Modules & Sub-topics

This document details the progressive curriculum for preparing for the **Google Cloud Professional Agentic Architect** certification, specifically targeted at hands-on execution and Part 2 practical lab readiness.

---

## Stage 0: Agentic Python Prerequisites (Foundational)
*Note: The tutor assumes these concepts are mastered unless the student explicitly starts in or asks questions about Stage 0.*

### Module 0.1: Pydantic v2 for Agent Tool Contracts
* **0.1.1 `BaseModel` & Schema Foundation:** Declaring models, type annotations, strict mode vs. type coercion (`ConfigDict(strict=True)`), serialization to dict/JSON.
* **0.1.2 Field Metadata for LLMs:** Using `Field(..., description="...")`, constraints (`ge`, `le`, regex patterns), and generating JSON schemas consumed by LLM function-calling declarations.
* **0.1.3 Custom Field & Model Validators:** `@field_validator` (mode='before' / 'after') for parameter sanitization; `@model_validator` for cross-field dependency validation before tool execution.
* **0.1.4 Nested & Dynamic Schemas:** Complex nested structures, Optional/Union types, and dynamic model generation for variable tool inputs.

### Module 0.2: `asyncio` for Concurrency & Agent Tool Loops
* **0.2.1 Event Loop & Coroutines:** Single-threaded non-blocking execution, `async def` and `await`, cooperative multitasking, identifying loop-blocking calls (`time.sleep` vs `asyncio.sleep`).
* **0.2.2 Parallel Agent Execution:** Concurrency with `asyncio.gather()` and Python 3.11+ `asyncio.TaskGroup` for parallel subagent and tool execution.
* **0.2.3 SLAs & Cancellation:** Enforcing execution timeouts with `asyncio.wait_for()`, shielding operations, and graceful handling of `CancelledError` during tool aborts.
* **0.2.4 Offloading Blocking Workloads:** Non-blocking threadpool offloading using `asyncio.to_thread()` for legacy synchronous SDKs or CPU-heavy parsing.

### Module 0.3: FastAPI & ASGI Runner Lifespans
* **0.3.1 Non-Blocking REST Endpoints:** `@app.post` route handling with Pydantic request/response body validation.
* **0.3.2 Async Route Mechanics:** Execution flow in ASGI, preventing thread pool exhaustion by properly utilizing `async def` route handlers.
* **0.3.3 Lifespan State Management:** Using `@asynccontextmanager` (`lifespan`) to initialize shared ADK `Runner` instances, database connection pools, and credentials at startup and cleanup on shutdown.
* **0.3.4 Containerization & Cloud Run Entrypoints:** Uvicorn CLI configuration, environment variable management, health check endpoints (`/healthz`), and Cloud Run container considerations.

---

## Stage 1: Low-Code Enterprise Agent Platforms (Exam Domain 1 ~13%)

### Module 1.1: State-Based Workflow Architecture
* **1.1.1 Console Platforms:** Architecture differences between Gemini Enterprise Agent Designer and Customer Experience (CX) Agent Studio.
* **1.1.2 State Components:** Designing pages, state transition routes, conditional intent triggers, and fallback event handlers.
* **1.1.3 Conversational State Control:** Managing parameter collection, form filling, slot tracking, and session lifetimes.

### Module 1.2: Enterprise Data Integration & Prompting in Low-Code
* **1.2.1 In-Console Prompt Engineering:** Constructing effective system instructions, few-shot exemplars, and chain-of-thought (CoT) prompts.
* **1.2.2 Agent Search Integration:** Connecting proprietary enterprise data sources, grounding configurations, and knowledge store synchronization.
* **1.2.3 Multimodal Data Ingestion:** Processing unstructured audio, video streams, and image documents directly within low-code workflows.

---

## Stage 2: Coding Agents & Developer Sandboxing (Exam Domain 2 ~17%)

### Module 2.1: Coding Agent Ecosystem & Extension Authoring
* **2.1.1 Google Antigravity Architecture:** Skills (`SKILL.md`), plugins (`plugin.json`), lifecycle hooks (`hooks.json`), rules (`GEMINI.md`, `AGENTS.md`), and specialized subagents.
* **2.1.2 Progressive Disclosure Pattern:** Designing lightweight skill frontmatter descriptions vs. deep reference documentation to optimize LLM context window usage.
* **2.1.3 Claude Code on GCP:** Workflow integration, command execution, and code review automation on Google Cloud.

### Module 2.2: Agents CLI & Operational Sandboxes
* **2.2.1 Agents CLI Operations:** Toggling between Agent Mode vs. Human Mode, lifecycle management, and agent deployment pipelines.
* **2.2.2 Secure Sandboxing Environments:** Containerized isolation using Google Kubernetes Engine (GKE), Cloud Workstations, and Antigravity sandboxing.
* **2.2.3 Automated Engineering Workflows:** Source code refactoring, runtime performance profiling, and automated patching of application-layer vulnerabilities.

---

## Stage 3: Developing Custom Agents with ADK (Exam Domain 3 ~33%)

### Module 3.1: ADK Core Agent & Runner Architecture
* **3.1.1 `LlmAgent` Declaration:** Configuring system instructions, model parameters (temperature, top_p), and Model Selection Strategy (Gemini 1.5 Pro vs. Flash, Gemma SLM, SaaS vs. OSS).
* **3.1.2 The `Runner` Execution Model:** Asynchronous execution management via `run_async`, message streaming, and conversational turn management.
* **3.1.3 Sessions & Context Memory:** Agent Platform Memory Bank, managed sessions, state isolation across users, and context window compaction strategies.

### Module 3.2: Custom Tools, Database Layers & MCP Integration
* **3.2.1 Native Async ADK Tools:** Defining tools using `@tool` / function declarations with strict Pydantic schemas, parameter descriptions, and robust exception handling.
* **3.2.2 Model Context Protocol (MCP):** Architecture of MCP (client, server, transport), configuring Google Cloud MCP servers, OpenAPI tool wrappers, and third-party SaaS tool binding.
* **3.2.3 Managed Database Connectors:** Building clean integration layers for BigQuery, Cloud Storage, Firestore, Cloud SQL, and Memorystore for Redis.

### Module 3.3: Enterprise RAG Pipelines & Vector Retrieval
* **3.3.1 RAG Architecture:** Vector embeddings (`text-embedding-005`), chunking strategies, distance metrics (Cosine vs. Dot Product), and cross-encoder rerankers.
* **3.3.2 Vector Search 1.0 & Agent Retrieval:** Deploying Vector Search index endpoints, hybrid search (keyword + dense vector), and RAG Engine integration.
* **3.3.3 Database Vector Extensions:** In-database vector search with Cloud SQL `pgvector` and Firestore Vector Search.

### Module 3.4: Multi-Agent Orchestration & Agentic Protocols
* **3.4.1 Orchestration Topologies:** Sequential pipeline chains, parallel fan-out / fan-in consensus, and hierarchical supervisor-worker patterns.
* **3.4.2 ADK 2.0 Graph Workflows:** Defining deterministic execution graphs, dynamic conditional routing, state machines, and cycle control.
* **3.4.3 Agent2Agent (A2A) Protocol:** Cross-service agent communication, task delegation contracts, handoff state payload serialization, and decentralized collaboration.

---

## Stage 4: Evaluating & Deploying Agentic Workloads (Exam Domain 4 ~22%)

### Module 4.1: Agent Evaluation & Benchmark Datasets
* **4.1.1 Dataset Construction:** Curating golden datasets, synthetic test generation, boundary condition prompts, and adversarial edge cases.
* **4.1.2 Evaluation Metrics:** Automated scoring for Faithfulness, Answer Relevance, Context Precision, Tool Calling Accuracy, and Goal Completion Rate.
* **4.1.3 Tooling & Autoraters:** ADK evaluation tooling (`evalset`), Agent Platform Gen AI evaluation service, and LLM-as-a-judge autorater calibration.

### Module 4.2: Production Deployment Runtimes
* **4.2.1 Runtime Selection:** Architectural trade-offs between Agent Runtime (managed agent engine), Cloud Run (serverless containerized ADK runners), and GKE (custom scale & sandboxing).
* **4.2.2 CI/CD Automation:** Containerizing ADK agents, Cloud Build pipelines, automated evaluation gating before production rollouts.

### Module 4.3: Operational Observability & Debugging
* **4.3.1 Cloud Observability:** Distributed tracing with Cloud Trace, structured logging with Cloud Logging, latency breakdowns per tool/model call.
* **4.3.2 Troubleshooting Pathologies:** Diagnosing and mitigating agent drift, infinite reasoning loops, tool invocation timeouts, and hallucinated function calls.

---

## Stage 5: Securing & Governing Agentic Workflows (Exam Domain 5 ~15%)

### Module 5.1: Agent Identity, Authentication & Tool Authorization
* **5.1.1 Agent Identity Architecture:** SPIFFE standard, workload identity federation, short-lived X.509 certificates, mTLS, and DPoP tokens (eliminating static keys).
* **5.1.2 Tool Authorization:** Agent-to-tool API calls via OAuth 2.0 and Google Cloud Auth Manager (3-legged OAuth flows for user delegation).
* **5.1.3 Principal Access Boundary (PAB):** Defining granular PAB policies attached to Agent Identity to prevent privilege escalation.

### Module 5.2: Agent Governance, Safety & Guardrails
* **5.2.1 Agent Gateway & Registry:** Centralized traffic inspection, routing policies, rate limiting, and agent discovery via Agent Registry.
* **5.2.2 Model Armor & Defense:** In-flight prompt injection detection, jailbreak mitigation, toxic content filtering, and sensitive data redaction (Cloud DLP integration).
* **5.2.3 Human-in-the-Loop (HITL):** Implementing mandatory human approval gates for critical actions (financial transfers, infrastructure mutation).

---

## Stage 6: Practical Projects & Lab Simulations (Hands-On Crucible)

These 4 full-scale projects simulate the Google Skills Part 2 lab challenges:

1. **Phase 1: Command-Line Tool Agent**
   * Implementation of an ADK `LlmAgent` with strict Pydantic tool models, Google Search grounding, and non-blocking asynchronous execution.
2. **Phase 2: Grounded Enterprise RAG Service**
   * Building an end-to-end RAG pipeline using Vector Search 1.0, Agent Retrieval, GCS document ingestion, and Agent Identity authentication.
3. **Phase 3: Multi-Agent Graph & Protocols**
   * Multi-agent system implementing an ADK 2.0 Graph workflow with dynamic routing, A2A task delegation, and external MCP server integration.
4. **Phase 4: Production AgentOps Container**
   * Containerized Cloud Run / Agent Runtime deployment fronted by Agent Gateway, secured with PAB policies and Model Armor, evaluated against golden datasets using `evalset`.
