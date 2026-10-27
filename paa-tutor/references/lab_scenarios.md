# Part 2 Hands-On Lab Scenarios & Challenge Bank

This document outlines realistic hands-on lab challenges designed to prepare students for the **Google Skills Part 2 practical exam**.

---

## Lab Challenge Format

Each lab challenge simulates a real-world task in the Google Cloud Agentic ecosystem. When presenting a challenge:
1. Provide the **Business Scenario & Architecture Objective**.
2. Specify the **Technical Requirements & Constraints** (no boilerplate provided; student implements from scratch).
3. Define the **Verification & Pass Criteria** (functional correctness, async safety, schema validity, security).

---

## Scenario 1: Native ADK Tool Implementation & Error Resilience
* **Objective:** Implement a production-grade custom tool for an ADK `LlmAgent` to query an inventory database.
* **Requirements:**
  * Define a Pydantic v2 schema for input parameters (`sku: str`, `warehouse_id: int`, `max_results: int = 10`) with comprehensive field descriptions and regex validation on SKU format.
  * Implement an `async def fetch_inventory_levels` tool function.
  * Offload a simulated legacy blocking DB client call using `asyncio.to_thread`.
  * Handle custom domain errors (`WarehouseNotFoundException`, network timeout) by returning structured, informative error strings that allow the agent to self-correct without crashing.
* **Pass Criteria:**
  * Zero event loop blockage.
  * Complete JSON schema descriptions enabling accurate LLM function calling.
  * Graceful error recovery.

---

## Scenario 2: Model Context Protocol (MCP) Server Integration
* **Objective:** Wire an ADK agent to communicate with a remote MCP server providing enterprise customer data.
* **Requirements:**
  * Configure an MCP client using standard transport (Stdio or SSE).
  * Expose MCP tools dynamically to the ADK `LlmAgent`.
  * Validate tool call arguments using Pydantic contracts before dispatching to the MCP server.
  * Enforce execution timeout SLAs using `asyncio.wait_for`.
* **Pass Criteria:**
  * Clean abstraction between ADK agent and MCP client transport.
  * Proper session context propagation.
  * Timeout handling without orphan MCP requests.

---

## Scenario 3: Deterministic ADK 2.0 Graph Workflow with Conditional Routing
* **Objective:** Build a multi-step agentic graph workflow for automated customer support triage and refund processing.
* **Requirements:**
  * Define an ADK Graph with nodes: `TriageNode`, `PolicyCheckNode`, `RefundExecutionNode`, `EscalationNode`.
  * Implement dynamic routing edges based on refund threshold ($ > \$500$ routes to `EscalationNode` for HITL approval).
  * Maintain shared state dictionary across node transitions.
  * Implement cycle detection to terminate looping after 3 failed validation attempts.
* **Pass Criteria:**
  * Graph routes execute deterministically.
  * State is immutably or safely updated across transitions.
  * Cycles are trapped and gracefully escalated.

---

## Scenario 4: Agent2Agent (A2A) Task Delegation & Protocol Handoff
* **Objective:** Orchestrate two autonomous agents: a `LeadCustomerFacingAgent` and a specialized `DatabaseAnalyticsAgent` using the A2A protocol.
* **Requirements:**
  * Implement the A2A task delegation schema: goal specification, input parameters, caller identity token, and callback URL.
  * Write the delegation invocation logic from Agent 1 to Agent 2.
  * Receive the async task completion event from Agent 2 and synthesize the final answer.
  * Propagate Agent Identity across the boundary.
* **Pass Criteria:**
  * Valid A2A payload structure.
  * Non-blocking asynchronous waiting for handoff completion.
  * Proper authorization headers attached to the delegation request.

---

## Scenario 5: Enterprise RAG with Vector Search 1.0 & Hybrid Reranking
* **Objective:** Architect and code a retrieval tool using Google Cloud Vector Search and Agent Retrieval.
* **Requirements:**
  * Implement dense embedding generation using `text-embedding-005`.
  * Execute nearest neighbor search against a Vector Search index endpoint with similarity threshold filtering.
  * Combine keyword search (lexical) with vector search (hybrid retrieval).
  * Apply a cross-encoder reranker to pick the top 3 most relevant context chunks.
  * Pass formatted groundings to the agent context with source citations.
* **Pass Criteria:**
  * Accurate embedding dimensionality matching.
  * Robust handling of empty retrieval results.
  * Clean citation metadata mapping.

---

## Scenario 6: Production Agent Container with FastAPI Lifespan & Cloud Run
* **Objective:** Package an ADK agent into an enterprise-ready FastAPI microservice for Cloud Run deployment.
* **Requirements:**
  * Implement `@asynccontextmanager` lifespan to initialize the ADK `Runner`, Memory Bank connection, and database client pool once at application boot.
  * Expose a non-blocking `POST /agent/run` endpoint accepting user prompts and session IDs.
  * Implement a `GET /healthz` endpoint verifying backend connectivity.
  * Ensure proper graceful shutdown releasing connections.
* **Pass Criteria:**
  * Runner instance is reused across requests (not re-instantiated per request).
  * Non-blocking ASGI execution.
  * Zero memory leaks across consecutive requests.

---

## Scenario 7: Securing Tools with SPIFFE, PAB Policies & Model Armor
* **Objective:** Implement security boundaries for an agent executing sensitive database updates.
* **Requirements:**
  * Validate that incoming requests carry a valid short-lived SPIFFE X.509 certificate / Agent Identity token.
  * Enforce Principal Access Boundary (PAB) rules to restrict tool execution to authorized database tables only.
  * Pass user prompts through a Model Armor inspection filter before feeding them into the LLM context.
  * Trigger a Human-in-the-Loop (HITL) approval callback if the prompt requests deletion operations.
* **Pass Criteria:**
  * Injection attempts are rejected by Model Armor filter.
  * PAB violations are logged and blocked before API dispatch.
  * Critical mutations are halted pending HITL token verification.

---

## Scenario 8: Automated Evaluation Pipeline with ADK `evalset`
* **Objective:** Create an automated evaluation suite to assess agent faithfulness and tool accuracy before deployment.
* **Requirements:**
  * Construct a golden dataset with 10 representative queries, expected tool calls, and ground truth answers.
  * Configure an ADK `evalset` runner to execute the test suite against the agent.
  * Implement an autorater (LLM-as-a-judge) using Gemini 1.5 Pro to score faithfulness and tool parameter precision.
  * Output an evaluation report with pass/fail threshold metrics.
* **Pass Criteria:**
  * Evaluator accurately flags misaligned tool arguments.
  * Scoring metrics correctly parse agent output against golden references.
