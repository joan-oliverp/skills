# Google Cloud Professional Agentic Architect (PAA) Exam Scope

This reference document defines the complete scope and toolset for the **Google Cloud Certified — Professional Agentic Architect** certification, with emphasis on practical execution for **Part 2 (Google Skills Hands-On Labs)**.

---

## Exam Structure

* **Part 1: Proctored Architectural & Conceptual Exam (Pearson VUE)**
  * Format: ~80 multiple-choice and multi-select questions.
  * Duration: 3 hours.
  * Focus: Architectural trade-offs, system design, security boundaries, protocol selection, cost vs. latency optimization.
* **Part 2: Hands-On Practical Validation (Google Skills)**
  * Access: Issued via CM Connect upon passing Part 1.
  * Format: Live cloud environment labs testing coding execution, runtime debugging, and workflow wiring.
  * Focus: Implementing ADK agents, writing custom async tools with Pydantic contracts, wiring MCP servers, constructing A2A and Graph workflows, deploying to Cloud Run / Agent Runtime, configuring Agent Identity & Model Armor, and evaluating with `evalset`.

---

## Detailed Exam Domains

### Section 1: Building Agents Using Low-Code Tools (~13%)
* **1.1 Configuring Agentic Workflows & Behavior:**
  * State-based workflows: pages, transition routes, conditional intents, and event handlers using Gemini Enterprise tools (Gemini Enterprise Agent Designer and CX Agent Studio).
  * System instructions and in-console prompt engineering: few-shot exemplars, chain-of-thought (CoT) prompting to guide agent behavior.
* **1.2 Connecting Enterprise Data to Gemini Enterprise:**
  * Securely connecting and querying enterprise proprietary data sources using Gemini Enterprise and Agent Search.
  * Multimodal ingestion and processing: streaming and batch processing of unstructured video, audio, PDFs, and images into the agentic workflow.

### Section 2: Using Coding Agents for Application Development (~17%)
* **2.1 Using Coding Agents Effectively:**
  * Configuring coding agents with Model Context Protocol (MCP) servers, custom skills, rules, and tool access (Google Antigravity and Claude Code on Google Cloud).
  * Operating coding agents inside secure execution sandboxes: Google Kubernetes Engine (GKE), Cloud Workstations, and Antigravity sandboxes.
  * Leveraging coding agents to refactor source code, optimize execution runtimes, and patch application-layer vulnerabilities.
* **2.2 Customizing Coding Agents for Enterprise Workflows:**
  * Authoring Antigravity extensions: skills, plugins, lifecycle extension hooks, directory rules, and specialized subagents.
  * Augmenting Antigravity with the Agents CLI to build, scale, govern, and optimize deployed agents.

### Section 3: Developing Custom Agents (~33% — Heaviest Lab Weight)
* **3.1 Designing and Building Agentic Workflows in Code:**
  * Model selection strategy: LLM vs. SLM (Gemini Pro vs. Flash, Gemma), self-hosted vs. SaaS (Vertex AI Model Garden), open-source vs. proprietary, balancing cost, throughput, and task complexity.
  * Building custom agents using the Agent Development Kit (ADK 2.0).
  * Configuring conversational sessions and memory: Agent Platform Memory Bank, managed sessions, state isolation, and context compaction.
  * Configuring skills with Agents CLI: plugins, toggling agent mode vs. human mode.
* **3.2 Integrating Enterprise Domain Knowledge:**
  * Designing, configuring, and managing Retrieval-Augmented Generation (RAG) pipelines and vector retrieval systems: embedding models (e.g., `text-embedding-005`), similarity metrics (Cosine, Dot Product, Euclidean), and reranking models.
  * Enterprise vector services: Vector Search 1.0, Agent Retrieval, and RAG Engine.
  * Agent Identity configuration for least-privilege retrieval access.
  * Google Cloud integration layers: Agent Registry, Google Cloud MCP Servers, prebuilt and custom connectors for managed databases (BigQuery, Cloud Storage, Firestore, Cloud SQL `pgvector`, Memorystore for Redis), and third-party SaaS connections.
* **3.3 Orchestrating and Coordinating Agentic Workflows:**
  * Agent communication protocols: Model Context Protocol (MCP) and Agent2Agent (A2A) protocol.
  * Multi-agent orchestration patterns: sequential pipelines, parallel fan-out/fan-in, and deterministic / dynamic ADK Graph workflows.
  * Handoff management and governance using Agent Identity, Agent Registry, Agent Runtime, and agent policies.

### Section 4: Evaluating and Deploying Agentic Workloads (~22%)
* **4.1 Evaluating Agents in Development and Production:**
  * Creating test sets: golden datasets, synthetic prompt variations, adversarial prompts, and edge cases.
  * Continuous evaluation pipelines for tool execution accuracy, goal completion, and latency SLAs.
  * Evaluation tooling: ADK evaluation framework (`evalset`), Agent Platform Gen AI evaluation service, and custom autoraters (LLM-as-a-judge).
  * Response and retrieval metrics: faithfulness, answer relevance, context precision, and context recall.
* **4.2 Deploying and Scaling Production Workloads:**
  * Selecting optimal deployment runtimes based on use case, statefulness, and cost: Agent Runtime (formerly Agent Engine), Cloud Run (serverless containers), and GKE.
  * Operational troubleshooting: diagnosing agent drift, tool invocation latency, infinite reasoning loops, and system failures.
  * Performance, reliability, and cost monitoring using Google Cloud Observability (Cloud Logging and Cloud Trace).

### Section 5: Securing and Governing Agentic Workflows (~15%)
* **5.1 Configuring Agent Security and Governance:**
  * Agent authentication and secure tool execution: agent-to-tool API calls using OAuth 2.0 and Auth Manager.
  * Agent Identity architecture: SPIFFE standard, short-lived X.509 certificates, mTLS, and DPoP token binding (superseding static service account keys).
  * Principal Access Boundary (PAB) policies configured with Agent Identity.
  * Agent Gateway configuration for traffic inspection, audit logging, and agent tracking.
  * Enterprise governance and policy enforcement using Agent Registry and Model Armor.
* **5.2 Implementing Secure Agent Behavior and Execution:**
  * Safety frameworks and guardrails: prompt injection and jailbreak defense via Model Armor, sensitive data redaction via Sensitive Data Protection (DLP).
  * Human-in-the-Loop (HITL) approval gates for high-stakes tool invocations.
  * Secure identity propagation and delegated authorization across multi-agent chains.

---

## Official Toolset In-Scope

1. **Agent Development Kit (ADK)**
2. **Agent evaluation & `evalset`**
3. **Agent Gateway**
4. **Agent Identity**
5. **Agent Registry**
6. **Agent Retrieval and Vector Search 1.0**
7. **Agent Runtime (formerly Agent Engine)**
8. **Agent Search (formerly Vertex AI Search)**
9. **Agentic protocols (Agent2Agent [A2A], MCP)**
10. **Agents CLI in Agent Platform**
11. **Antigravity (CLI, SDK, App)**
12. **Auth Manager (OAuth 2.0)**
13. **BigQuery**
14. **Cloud Run**
15. **Cloud SQL (`pgvector`)**
16. **Cloud Storage (GCS)**
17. **Firestore (Vector Search)**
18. **Gemini Enterprise (Agent Designer, CX Agent Studio)**
19. **Gemini LLMs (Flash, Pro) & Gemma SLMs**
20. **Google Cloud Observability (Cloud Logging and Cloud Trace)**
21. **Google Kubernetes Engine (GKE)**
22. **Memorystore for Redis**
23. **Model Armor**
24. **Model Context Protocol (MCP) servers**
25. **Model Garden**
26. **RAG Engine**
27. **Sensitive Data Protection (Cloud DLP)**
28. **Skill Registry**
