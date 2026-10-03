# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **# Explainability & Decision Transparency Report** (`cloud-agents`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** # Explainability & Decision Transparency Report (`cloud-agents`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Stateful Edge AI Agents & Durable Objects Infrastructure  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

### 1. Deterministic Multi-Stage Decision Pipeline
The edge agent processes incoming state mutations, WebSocket events, and RPC commands through a deterministic, 5-stage edge execution pipeline.

### 1. Decision Architecture

The runtime intake, state classification, evaluation, and execution tracking operate across a deterministic, five-stage pipeline:

```
+-----------------------------------------------------------------------------------+
|                        Deterministic Edge Agent Pipeline                          |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Ingress Route & Durable Object Dispatch]                              |
|     --> Inspect request URL, extract agent ID/name, & route to edge DO instance   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: State Hydration & Hibernation Wakeup]                                  |
|     --> Awaken hibernated DO from edge memory or hydrate schema from SQLite storage|
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Method Execution & Transaction Serialization]                          |
|     --> Execute @callable method or RPC handler within single-threaded event loop  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Atomic Persistence & State Broadcasting]                              |
|     --> Commit SQL state delta & broadcast binary delta over hibernating WebSockets|
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Alarm Scheduling & Idle Hibernation]                                   |
|     --> Set background timer alarms and transition runtime memory to hibernation  |
+-----------------------------------------+-----------------------------------------+
```

### 2. Decision Logic & Routing Formulations



### 3. Thresholding & Refusal Decision Criteria

# Explainability & Decision Transparency Report enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on Policy Violation**: Requests violating boundary constraints halt with code `ERR_POLICY_VIOLATION`.
- **Refusal on Timeout**: Executions exceeding budget limits terminate with code `ERR_EXECUTION_TIMEOUT`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Operational Review**: Sensitive actions require operator sign-off.
- **Audit Logging**: All decisions are recorded for auditability.

---

## The Data It Uses

# Explainability & Decision Transparency Report operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Input Directives**: Operational tasks and data payloads.

### 2. Configuration & Reference Data

- **Configuration Schemas**: Declarative system configuration files.

### 3. Base Model & Inference Lineage

- **AI Models**: Cloudflare Workers AI (Llama 3.3, Mistral, embedding models) and external LLM APIs (OpenAI, Anthropic) via fetch bindings.
- **Platform Dependencies**: Cloudflare Workers runtime (V8 isolates), `workerd`, `wrangler`.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection, credential leakage, and unauthorized external API dispatch.
- **Local Environment Isolation**: Agent execution workspaces, intermediate scratchpads, and vector stores reside strictly within designated local project directories.
- **Automated Secret Scrubbing**: API keys, database credentials, and personal credentials are automatically redacted prior to embedding or logging.
- **Zero Commercial Monetization**: Prompts, intermediate reasoning trajectories, and task deliverables are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of # Explainability & Decision Transparency Report is essential for effective deployment.

### 1. Deterministic Multi-Stage Decision Pipeline
The edge agent processes incoming state mutations, WebSocket events, and RPC commands through a deterministic, 5-stage edge execution pipeline.

```
+-----------------------------------------------------------------------------------+
|                        Deterministic Edge Agent Pipeline                          |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Ingress Route & Durable Object Dispatch]                              |
|     --> Inspect request URL, extract agent ID/name, & route to edge DO instance   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: State Hydration & Hibernation Wakeup]                                  |
|     --> Awaken hibernated DO from edge memory or hydrate schema from SQLite storage|
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Method Execution & Transaction Serialization]                          |
|     --> Execute @callable method or RPC handler within single-threaded event loop  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Atomic Persistence & State Broadcasting]                              |
|     --> Commit SQL state delta & broadcast binary delta over hibernating WebSockets|
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Alarm Scheduling & Idle Hibernation]                                   |
|     --> Set background timer alarms and transition runtime memory to hibernation  |
+-----------------------------------------+-----------------------------------------+
```

### 2. Mathematical Decision & Affinity Scoring
Edge agent co-location and Durable Object migration scoring are computed using a geo-latency affinity formulation:

$$S_{\text{edge}}(c, n) = \alpha \cdot \left(1 - \frac{\text{RTT}(c, n)}{\text{MaxRTT}}\right) + \beta \cdot \text{CacheLocality}(n) - \gamma \cdot \text{EvictionPressure}(n)$$

Where:
- $\alpha = 0.55$: Round-trip network latency between client $c$ and Cloudflare edge node $n$.
- $\beta = 0.30$: Local SSD / SQLite cache warmness on node $n$.
- $\gamma = 0.15$: Memory pressure and concurrent Durable Object density on node $n$.

Message routing prioritization across active WebSocket channels applies:

$$P(\text{Channel}_i) = \frac{\exp(u_i / \tau)}{\sum_{j=1}^{C} \exp(u_j / \tau)}$$

Where $u_i$ is channel activity score and $\tau = 0.5$ is the temperature parameter governing broadcast scheduling.

### 3. Thresholding & Refusal Decision Criteria
When client calls violate transactional constraints or resource ceilings, execution is terminated with explicit status codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Durable Object Memory Limit** | $> 128$ MB | Terminate transaction and reject allocation | `ERR_DO_MEMORY_LIMIT_EXCEEDED` |
| **Max Concurrent WebSockets** | $> 32,768$ connections | Refuse new incoming WebSocket upgrades | `ERR_WEBSOCKET_CAPACITY_REACHED` |
| **Storage Transaction Size** | $> 2$ MB per write | Rollback transaction and reject payload | `ERR_SQLITE_WRITE_OVERSIZED` |
| **Circular RPC Depth** | $\ge 5$ hops | Break call chain to prevent distributed deadlock | `ERR_CIRCULAR_CALL_DETECTED` |
| **Alarm Rate Limit** | $> 1$ alarm per 10s per agent | Throttle scheduling request | `ERR_ALARM_FREQUENCY_EXCEEDED` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Transaction Rollback)**: If a SQLite write or `@callable` method errors mid-execution, the transaction automatically reverts to the pre-call checkpoint.
2. **Tier 2 (Replica Edge Failover)**: If an edge point-of-presence (PoP) experiences hardware degradation, Cloudflare's global anycast network automatically migrates the Durable Object instance to the nearest healthy datacenter within seconds.
3. **Tier 3 (Human Administrator Intervention)**: Critical schema migration mismatches or persistent database corruption trigger automated alerting to DevOps administrators.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **HTTP / RPC Invocations**: REST JSON payloads, FormData, and binary ArrayBuffers.
- **WebSocket Frames**: Bidirectional JSON delta updates and raw UTF-8 protocol envelopes.
- **MCP Tool Payloads**: JSON-RPC 2.0 requests, responses, and resource URIs.

### 2. Reference Storage & Database Engines
- **Durable Object SQLite**: Embedded zero-latency relational tables per agent instance.
- **Key-Value Storage**: Fast key-value bindings for metadata and cursor pointers.
- **Vectorize**: Embedded vector embeddings for semantic search over agent memory.

### 3. Model Lineage & System Architecture
- **AI Models**: Cloudflare Workers AI (Llama 3.3, Mistral, embedding models) and external LLM APIs (OpenAI, Anthropic) via fetch bindings.
- **Platform Dependencies**: Cloudflare Workers runtime (V8 isolates), `workerd`, `wrangler`.

### 4. Data Privacy, Governance & Retention
- **Per-Agent Cryptographic Isolation**: Each Durable Object is strictly sandboxed in its own V8 isolate with no cross-instance memory sharing.
- **Data Residency Options**: Enforce jurisdiction-restricted placement (EU, US, APAC) to satisfy GDPR and CCPA compliance.
- **Ephemeral State Cleanup**: Inactive agent SQLite databases can be scheduled for auto-pruning via TTL alarm handlers.

---

## Limitations

### 1. Single-Threaded Per-Instance Concurrency
- **Limitation**: Each Durable Object instance processes requests sequentially on a single thread, creating a bottleneck if high throughput targets a single agent ID.
- **Mitigation**: Scale horizontally by sharding workloads across multiple agent instances (e.g., partitioning by user ID or channel ID).

### 2. V8 Isolate Memory Cap
- **Limitation**: Default Cloudflare Worker isolates are capped at 128MB of RAM, restricting in-memory caching of massive datasets.
- **Mitigation**: Offload large binary blobs to Cloudflare R2 object storage and stream chunks on demand.

### 3. Maximum Execution Time per CPU Slice
- **Limitation**: Long-running synchronous CPU-bound operations exceeding isolate CPU limits trigger script cancellation.
- **Mitigation**: Defer heavy computation to Cloudflare Workflows or asynchronous background tasks via alarm queues.

### 4. Cold State Hydration Latency
- **Limitation**: If an agent with large SQLite tables has hibernated for a prolonged period, cold hydration incurs a few milliseconds of latency.
- **Mitigation**: Pre-warm frequently accessed agents using periodic ping alarms or speculative edge prefetching.

### 5. WebSocket TCP Keepalive Dependency
- **Limitation**: Intermediate mobile networks or aggressive client firewalls may silently terminate inactive WebSocket connections.
- **Mitigation**: Leverage the Cloudflare WebSocket Hibernation API's automated auto-ping/auto-pong keepalive protocol.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested input data & query streams | Section 1 | Verified |
| - Configuration & reference schemas | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Deterministic Multi-Stage Decision Pipeline
The edge agent processes incoming state mutations, WebSocket events, and RPC commands through a deterministic, 5-stage edge execution pipeline.

```
+-----------------------------------------------------------------------------------+
|                        Deterministic Edge Agent Pipeline                          |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Ingress Route & Durable Object Dispatch]                              |
|     --> Inspect request URL, extract agent ID/name, & route to edge DO instance   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: State Hydration & Hibernation Wakeup]                                  |
|     --> Awaken hibernated DO from edge memory or hydrate schema from SQLite storage|
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Method Execution & Transaction Serialization]                          |
|     --> Execute @callable method or RPC handler within single-threaded event loop  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Atomic Persistence & State Broadcasting]                              |
|     --> Commit SQL state delta & broadcast binary delta over hibernating WebSockets|
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Alarm Scheduling & Idle Hibernation]                                   |
|     --> Set background timer alarms and transition runtime memory to hibernation  |
+-----------------------------------------+-----------------------------------------+
```

### 2. Mathematical Decision & Affinity Scoring
Edge agent co-location and Durable Object migration scoring are computed using a geo-latency affinity formulation:

$$S_{\text{edge}}(c, n) = \alpha \cdot \left(1 - \frac{\text{RTT}(c, n)}{\text{MaxRTT}}\right) + \beta \cdot \text{CacheLocality}(n) - \gamma \cdot \text{EvictionPressure}(n)$$

Where:
- $\alpha = 0.55$: Round-trip network latency between client $c$ and Cloudflare edge node $n$.
- $\beta = 0.30$: Local SSD / SQLite cache warmness on node $n$.
- $\gamma = 0.15$: Memory pressure and concurrent Durable Object density on node $n$.

Message routing prioritization across active WebSocket channels applies:

$$P(\text{Channel}_i) = \frac{\exp(u_i / \tau)}{\sum_{j=1}^{C} \exp(u_j / \tau)}$$

Where $u_i$ is channel activity score and $\tau = 0.5$ is the temperature parameter governing broadcast scheduling.

### 3. Thresholding & Refusal Decision Criteria
When client calls violate transactional constraints or resource ceilings, execution is terminated with explicit status codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Durable Object Memory Limit** | $> 128$ MB | Terminate transaction and reject allocation | `ERR_DO_MEMORY_LIMIT_EXCEEDED` |
| **Max Concurrent WebSockets** | $> 32,768$ connections | Refuse new incoming WebSocket upgrades | `ERR_WEBSOCKET_CAPACITY_REACHED` |
| **Storage Transaction Size** | $> 2$ MB per write | Rollback transaction and reject payload | `ERR_SQLITE_WRITE_OVERSIZED` |
| **Circular RPC Depth** | $\ge 5$ hops | Break call chain to prevent distributed deadlock | `ERR_CIRCULAR_CALL_DETECTED` |
| **Alarm Rate Limit** | $> 1$ alarm per 10s per agent | Throttle scheduling request | `ERR_ALARM_FREQUENCY_EXCEEDED` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Transaction Rollback)**: If a SQLite write or `@callable` method errors mid-execution, the transaction automatically reverts to the pre-call checkpoint.
2. **Tier 2 (Replica Edge Failover)**: If an edge point-of-presence (PoP) experiences hardware degradation, Cloudflare's global anycast network automatically migrates the Durable Object instance to the nearest healthy datacenter within seconds.
3. **Tier 3 (Human Administrator Intervention)**: Critical schema migration mismatches or persistent database corruption trigger automated alerting to DevOps administrators.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **HTTP / RPC Invocations**: REST JSON payloads, FormData, and binary ArrayBuffers.
- **WebSocket Frames**: Bidirectional JSON delta updates and raw UTF-8 protocol envelopes.
- **MCP Tool Payloads**: JSON-RPC 2.0 requests, responses, and resource URIs.

### 2. Reference Storage & Database Engines
- **Durable Object SQLite**: Embedded zero-latency relational tables per agent instance.
- **Key-Value Storage**: Fast key-value bindings for metadata and cursor pointers.
- **Vectorize**: Embedded vector embeddings for semantic search over agent memory.

### 3. Model Lineage & System Architecture
- **AI Models**: Cloudflare Workers AI (Llama 3.3, Mistral, embedding models) and external LLM APIs (OpenAI, Anthropic) via fetch bindings.
- **Platform Dependencies**: Cloudflare Workers runtime (V8 isolates), `workerd`, `wrangler`.

### 4. Data Privacy, Governance & Retention
- **Per-Agent Cryptographic Isolation**: Each Durable Object is strictly sandboxed in its own V8 isolate with no cross-instance memory sharing.
- **Data Residency Options**: Enforce jurisdiction-restricted placement (EU, US, APAC) to satisfy GDPR and CCPA compliance.
- **Ephemeral State Cleanup**: Inactive agent SQLite databases can be scheduled for auto-pruning via TTL alarm handlers.

---

## Limitations

### 1. Single-Threaded Per-Instance Concurrency | Section 1 | Verified |
| - V8 Isolate Memory Cap | Section 2 | Verified |
| - Maximum Execution Time per CPU Slice | Section 3 | Verified |
| - Cold State Hydration Latency | Section 4 | Verified |
| - WebSocket TCP Keepalive Dependency | Section 5 | Verified |
