# Feynman Code Review & Evaluation Rubric

This document defines the strict grading criteria and feedback methodology used by the tutor when reviewing student code and architectural implementations.

---

## The "Feynman" Review Philosophy

1. **Guiding, Not Spoiling:**
   * Never output the full corrected code snippet immediately.
   * Point out the exact symptom, behavior anomaly, or violated contract.
   * Ask targeted questions that prompt the student to explain *why* their code behaves the way it does and where the bug originates.
2. **First-Principles Thinking:**
   * Require the student to explain the underlying mechanics (e.g., "What happens to the event loop if this SDK call blocks for 3 seconds?").
   * Connect Python runtime mechanics directly to Google Cloud production implications (e.g., Cloud Run concurrency limits, cold starts, SPIFFE token expiry).

---

## The 5 Core Review Vectors

### 1. Asynchronous Event Loop Safety
* **Check for Loop Starvation:** Did the student call synchronous I/O functions (`requests.get`, `time.sleep`, standard SQLite or file operations) inside an `async def` function?
  * *Correction guidance:* Remind them that the single-threaded event loop freezes for all concurrent agent requests; prompt them to use an async client or `asyncio.to_thread()`.
* **Gather & Concurrency Error Handling:** Did the student use `return_exceptions=True` in `asyncio.gather` or handle individual task exceptions in `TaskGroup`? If one task fails, does the entire agent crash?
* **Timeouts & Cancellation:** Are external network calls wrapped in `asyncio.wait_for` with an explicit timeout? Is `asyncio.CancelledError` properly propagated or logged?

### 2. Pydantic v2 Tool Contract Precision
* **Field Documentation:** Are all schema fields annotated with `Field(..., description="...")`? An LLM cannot accurately call functions if descriptions are ambiguous or missing.
* **Type Safety & Constraints:** Are input constraints enforced at the contract boundary (e.g., regex patterns, minimum values, enum choices) rather than inside the tool body?
* **Validation Mode:** Are validators using `@field_validator` and `@model_validator` correctly with appropriate modes (`before` vs. `after`)?

### 3. Google Cloud Architecture & ADK Patterns
* **Runner State & Lifespan:** Is the ADK `Runner` instantiated inside request handlers (bad anti-pattern leading to memory bloat) or initialized once in a FastAPI `@asynccontextmanager` lifespan?
* **Session Isolation:** Does the code properly isolate user sessions in Memory Bank? Are global variables improperly accumulating state across turns?
* **Model Selection Match:** Did the student pick an appropriate model for the task (e.g., Gemini Flash for low-latency routing, Gemini Pro for complex multi-step reasoning)?

### 4. Enterprise Security & Governance Boundaries
* **Identity vs. Static Keys:** Does the solution rely on hardcoded service account keys, or does it enforce Agent Identity (SPIFFE, short-lived X.509 certs, workload identity)?
* **Principal Access Boundary (PAB):** Are tool permissions scoped to the absolute minimum necessary boundary?
* **Input Sanitization & Guardrails:** Are raw user inputs sent directly to execution tools without Model Armor or prompt boundary checks?
* **Human-in-the-Loop (HITL):** Are irreversible mutations (deletions, payments) guarded by explicit approval states?

### 5. Production Observability & Error Recovery
* **Agent Self-Correction:** When a tool fails, does it throw an unhandled Python exception that crashes the agent runner, or does it return a structured error message that allows the LLM to understand what went wrong and retry/pivot?
* **Structured Logging & Tracing:** Are correlation IDs (trace IDs, session IDs) propagated to Cloud Logging and Cloud Trace?
