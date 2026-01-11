**Project:** **“Agentic Enterprise Copilot”**
A production-grade **multi-agent orchestration service** with two “domains” you can demo in interviews:

1. **Enterprise Knowledge Copilot (Yarnit-style):** content + enterprise docs assistant (advanced RAG, KG retrieval, prompt tooling, eval/observability)
2. **Security Ops Copilot (Cyber JD):** log triage + anomaly summary + incident response agent

**Core architecture (end state by Day 15):**

* **FastAPI** backend (REST) + optional **Streamlit** UI
* **LangGraph** multi-agent workflow (planner + tools + memory + guardrails)
* Retrieval: **Vector DB** + **SQL agent** + **Knowledge Graph**
* **Evaluation harness** (offline + online feedback)
* **Tracing/observability** (tool traces, prompt versioning, cost/latency)
* **MLOps**: CI/CD, MLflow + DVC, model registry, retraining hooks
* **AWS deploy**: ECS/EKS + S3 + (optional Bedrock/SageMaker/Lambda)

Datasets you can use (security domain):

* Synthetic cybersecurity logs for anomaly detection (Kaggle). ([Kaggle][1])
* Windows Event Logs (Kaggle). ([Kaggle][2])
* Advanced SIEM logs dataset (Hugging Face). ([Hugging Face][3])
* MITRE ATT&CK STIX data for KG retrieval. ([GitHub][4])
* CybersecurityEval (HF) for Q&A/eval or instruction data. ([Hugging Face][5])

---

- Core idea: **3 of these are “log datasets” (rows of events), 1 is “threat-intel knowledge graph data” (STIX objects/relationships), and 1 is “Q&A/eval text data.”** The “easiest” depends on what you want to build.

## What each dataset is (and how to think about it)

### 1) Synthetic cybersecurity logs for anomaly detection (Kaggle)

* **What it is:** Synthetic (simulated) security logs—often shaped like web/HTTP access or security events—made specifically for **anomaly detection** practice. ([Kaggle][1])
* **Why people use it:** Great to learn pipelines: parsing → feature extraction → “normal vs abnormal” scoring, without needing real enterprise data.
* **Typical difficulty:** **Easy–Medium** (synthetic = cleaner patterns, fewer weird edge cases).

### 2) Windows Event Logs (Kaggle)

* **What it is:** A big CSV of **Windows Event Log** entries (Kaggle lists `eventlog.csv` ~77 MB). ([Kaggle][2])
* **Why people use it:** Blue-team analytics: suspicious logon patterns, privilege escalation hints, service creation, etc.
* **Typical difficulty:** **Medium–Hard** because Windows logs are “domain-heavy” (Event IDs, channels, message templates, noisy baselines).

### 3) Advanced SIEM logs dataset (Hugging Face)

* **What it is:** A **synthetic** SIEM-style dataset of **100,000 security events**, stored as **JSONL**; includes many event types (firewall, IDS, auth, endpoint, network, cloud, IoT, etc.) plus metadata and even MITRE technique mentions. ([Hugging Face][3])
* **Why people use it:** End-to-end SOC-style ML tasks: anomaly detection, classification, UEBA-style features.
* **Typical difficulty:** **Medium** (richer schema than simple CSV; but still synthetic and documented).

### 4) MITRE ATT&CK STIX data for KG retrieval (GitHub)

* **What it is:** **Structured threat intelligence** exported as **STIX 2.1 bundles/collections**, representing ATT&CK domains/releases. ([GitHub][4])
* **Why people use it:** Build a **knowledge graph (KG)** / RAG retrieval over techniques, tactics, groups, software, mitigations, relationships.
* **Typical difficulty:** **Hard (conceptually)** because it’s not “log rows”—it’s a graph of objects + relationships (STIX).

### 5) CybersecurityEval (HF) for Q&A/eval or instruction data (Hugging Face)

* **What it is:** A small **Q&A evaluation** dataset (example: CyberNative’s dataset has **500 rows** and explicitly says **NOT FOR TRAINING**). ([Hugging Face][5])
* **Why people use it:** Evaluate LLM Q&A performance in cybersecurity (accuracy, reasoning, consistency).
* **Typical difficulty:** **Easy to read** (it’s basically questions + answers), but it’s **not log analytics**.

---

## Which one is easiest to understand?

If “easy” means **human-readable immediately**:

1. **CyberSecurityEval (HF)** — it’s plain Q&A text. ([Hugging Face][5])
2. **Synthetic cybersecurity logs (Kaggle)** — typically simple, tabular, and made for anomaly detection learning. ([Kaggle][1])
3. **Advanced SIEM Dataset (HF)** — more fields + JSONL, but well-described schema. ([Hugging Face][3])
4. **Windows Event Logs (Kaggle)** — lots of Windows-specific meaning/noise. ([Kaggle][2])
5. **MITRE ATT&CK STIX** — easiest only if you already think in graphs/CTI objects. ([GitHub][4])

If your goal is **anomaly detection on logs (ML)**: start with **Synthetic Kaggle logs → Advanced SIEM → Windows logs**.

## Tiny analogy

* **Logs datasets** = “bank statements” (rows of transactions/events).
* **MITRE STIX** = “a map of crime methods and relationships” (graph of concepts).
* **CyberSecurityEval** = “exam questions and answer key.”

## Common mistake checkpoints (quick)

* **Don’t mix purposes:** STIX is for **knowledge retrieval**, not anomaly detection on timestamps/IPs. ([GitHub][4])
* **Respect usage rules:** Some “eval” datasets explicitly say **not for training**—use them for benchmarking only. ([Hugging Face][5])

[1]: https://www.kaggle.com/datasets/fcwebdev/synthetic-cybersecurity-logs-for-anomaly-detection?utm_source=chatgpt.com "Synthetic Cybersecurity Logs for Anomaly Detection"
[2]: https://www.kaggle.com/datasets/mehulkatara/windows-event-log?utm_source=chatgpt.com "Windows Event Log"
[3]: https://huggingface.co/datasets/darkknight25/Advanced_SIEM_Dataset "darkknight25/Advanced_SIEM_Dataset · Datasets at Hugging Face"
[4]: https://github.com/mitre-attack/attack-stix-data "GitHub - mitre-attack/attack-stix-data: STIX data representing MITRE ATT&CK"
[5]: https://huggingface.co/datasets/CyberNative/CyberSecurityEval "CyberNative/CyberSecurityEval · Datasets at Hugging Face"

---

# Day-wise prompts (copy-paste)

## Day 1

### Day 1 Notes Prompt

```text
Day 1 — Foundations + Architecture (Multi-agent GenAI + Production Python)

You are my Senior AI Engineer interview coach.
Goal: cover core concepts needed for two roles: (1) multi-agent enterprise GenAI + advanced RAG, (2) agentic AI for cybersecurity with AWS/MLOps.

Create structured notes with:
1) Core foundations (LLM basics, tokens/embeddings, RAG vs fine-tune, latency/cost)
2) Multi-agent mental model: planner, tools, memory, guardrails, eval loops
3) Production architecture: FastAPI service boundaries, config, logging, error handling
4) Interview Q&A: 12 questions + strong concise answers
5) “What I will say in interview” story: 8–10 bullets (end-to-end narrative)

Include: small ASCII diagrams + 5 common pitfalls + how to avoid them.
```

### Day 1 PoC Prompt

```text
Day 1 — Build PoC skeleton (production-grade repo)

Act as Staff Engineer. Create a repo blueprint for “Agentic Enterprise Copilot” with:
- FastAPI backend (health, /chat placeholder, /ingest placeholder)
- Modular structure (api, services, agents, retrieval, eval, observability, config)
- Pydantic settings, structured logging, error middleware
- Dockerfile + docker-compose for local dev (API + Postgres)
- Basic unit tests + pre-commit config
- README with run commands + env vars

Output:
1) Final file tree
2) Key files content (minimal but real)
3) Commands to run locally
4) Acceptance checklist for Day 1
```

---

## Day 2

### Day 2 Notes Prompt

```text
Day 2 — Data + Retrieval Fundamentals (chunking, embeddings, vector search)

Teach retrieval like a production RAG engineer:
- Chunking strategies + metadata
- Embeddings (what, how to choose, pitfalls)
- Similarity search + filters + hybrid search concept
- Data freshness: incremental ingestion + re-embedding strategies
- Security logs as a retrieval source (what fields matter)

Include:
- 2 real-world RAG examples (enterprise docs + security logs triage)
- Interview Q&A (10)
- Mini-checklist: “I can explain…” (10 items)
```

### Day 2 PoC Prompt

```text
Day 2 — Ingestion pipeline v1 + dataset loader

Implement:
- /ingest endpoint to load documents + security logs
- Add a dataset loader that can ingest either:
  (A) HuggingFace SIEM-like dataset (Advanced_SIEM_Dataset)
  (B) Kaggle-style CSV logs (HTTP/security logs)
- Normalize logs into a canonical schema (timestamp, source, event_type, severity, message, raw)

Store raw data in Postgres (tables: documents, log_events).
Add basic CLI command: python -m app.ingest --source <...>.

Output:
- Schema (SQLAlchemy models)
- Ingestion code + validation
- 3 unit tests (schema + loader + API)
- README update with example commands
```

(Examples of datasets: HF Advanced SIEM dataset ([Hugging Face][3]), Kaggle synthetic cybersecurity logs ([Kaggle][1]))

---

## Day 3

### Day 3 Notes Prompt

```text
Day 3 — LLM Gateway + Prompt Engineering Tooling

Cover in depth:
- System vs developer vs user prompts; prompt templates
- Tool instructions + structured outputs (JSON schemas)
- Guardrails basics (refusal, safe completion, data leakage prevention)
- Provider abstraction: OpenAI vs AWS Bedrock vs HF Inference (conceptually)
- Prompt versioning strategy for production

Include:
- Prompt injection examples + defenses
- Interview Q&A (10)
- “Golden prompts” starter pack (5 templates)
```

### Day 3 PoC Prompt

```text
Day 3 — Implement LLM Provider + prompt templates + basic guardrails

Implement:
- LLMProvider interface + 2 implementations:
  1) MockLLM (for deterministic tests)
  2) RealLLM placeholder (OpenAI or Bedrock via env flags, but keep optional)
- Prompt templates folder:
  - rag_answer.md
  - security_triage.md
  - incident_report.md
- Add prompt registry with versioning (e.g., prompt_name + version)
- Add basic safety gate:
  - detect prompt injection phrases
  - refuse requests that ask for secrets/credentials
- Add /chat endpoint that uses templates (no RAG yet, just clean prompt flow)

Output:
- Code changes + tests using MockLLM
- Logging of prompt_name/version + token usage placeholders
- Acceptance checklist
```

---

## Day 4

### Day 4 Notes Prompt

```text
Day 4 — RAG v1 end-to-end (retrieval → context → grounded answer)

Teach:
- RAG pipeline stages and failure modes
- Context construction, citations, and hallucination control
- Retrieval metrics (Recall@K, MRR) + answer quality signals
- Reranking (when/why)
- How RAG differs for enterprise docs vs security logs

Include:
- 2 ASCII flow diagrams
- Interview Q&A (12) with “why this design” answers
```

### Day 4 PoC Prompt

```text
Day 4 — Build RAG v1 (vector retrieval + citations)

Implement:
- Vector index (use a local vector DB like Chroma or Qdrant; pick one and justify)
- /index endpoint: embed + upsert chunks (documents + normalized logs)
- /rag_chat endpoint:
  - retrieve top-k
  - build context with source metadata
  - answer with citations
- Add reranking hook placeholder (interface + TODO)
- Add offline eval script:
  - small gold set (10 queries) + compute Recall@K based on expected doc ids

Output:
- Code + tests + example curl commands
- README section “RAG v1”
```

---

## Day 5

### Day 5 Notes Prompt

```text
Day 5 — Agents 101 (tools, routing, planning, memory)

Teach:
- What makes an “agent” vs a “RAG chain”
- Tool design: idempotency, timeouts, retries, structured tool outputs
- Routing patterns (classifier → tool, function calling)
- Memory types (short-term state vs long-term store) + privacy considerations
- Cybersecurity workflow mapping: triage → investigate → recommend response

Include:
- 2 mini case studies (enterprise doc assistant + SOC assistant)
- Interview Q&A (10)
```

### Day 5 PoC Prompt

```text
Day 5 — Add tool layer + router agent

Implement tools:
- RetrievalTool (vector)
- LogQueryTool (SQL query to Postgres log_events)
- SummarizeTool (LLM summarize with safety)
- IncidentReportTool (structured JSON output)

Add a Router agent that chooses:
- RAG answer
- SQL log query
- Incident report
based on user intent.

Output:
- LangGraph (or equivalent) workflow graph
- Tool schemas (Pydantic models)
- Tests for routing decisions using MockLLM
```

---

## Day 6

### Day 6 Notes Prompt

```text
Day 6 — Multi-agent orchestration (supervisor/worker, parallelism, failures)

Teach:
- Supervisor + specialists pattern
- Parallel tool calls vs sequential planning
- Failure handling: fallback, retries, circuit breaker
- Human-in-the-loop approvals (where it matters)
- Enterprise constraints: governance, audit, data boundaries

Include:
- Interview Q&A (12)
- “Design choices” bullets: why LangGraph-style state machines help
```

### Day 6 PoC Prompt

```text
Day 6 — Multi-agent v1 (Supervisor + specialists) + HITL gate

Implement agents:
- SupervisorAgent (routes and validates outputs)
- RAGAgent
- SQLAgent (log analytics)
- ResponseAgent (final answer with citations + tone)

Add HumanApproval step for high-risk actions:
- e.g., “block an IP”, “disable user”, “rotate credentials”
Return “approval_required” with suggested action plan.

Output:
- Graph diagram (ASCII)
- API contract updates
- 5 integration tests (happy path + refusal + approval)
```

---

## Day 7

### Day 7 Notes Prompt

```text
Day 7 — SQL Agents for grounded answers (text-to-SQL safely)

Teach:
- Text-to-SQL risks (injection, wrong joins, hallucinated columns)
- Guardrails: SQL parser/allowlist, read-only role, query validation
- Explainability: show generated SQL + rationale
- Security logs analytics patterns (top sources, anomalies by severity)

Include:
- Interview Q&A (10)
- “Common mistakes” checklist
```

### Day 7 PoC Prompt

```text
Day 7 — Implement safe SQLAgent

Implement:
- SQL generation prompt template + structured output (sql + explanation)
- SQL validator: allowlist tables/columns; block DDL/DML; limit rows
- Execute query against Postgres read-only connection
- Return results + chart-friendly JSON

Add endpoints:
- /sql_chat (agent-driven)
- /logs/stats (prebuilt analytics)

Output:
- Validator code + tests
- Demo queries list (10)
```

---

## Day 8

### Day 8 Notes Prompt

```text
Day 8 — Knowledge Graph RAG (KG retrieval + when it beats vectors)

Teach:
- What KG adds (entities/relations, multi-hop reasoning, governance)
- How to model: nodes/edges, properties, basic queries
- Hybrid RAG: KG → candidate set → vector rerank
- Use MITRE ATT&CK as a practical KG for security reasoning

Include:
- Interview Q&A (10)
- “When NOT to use KG” (tradeoffs)
```

### Day 8 PoC Prompt

```text
Day 8 — Add KG retrieval (MITRE ATT&CK mini KG)

Implement:
- Load MITRE ATT&CK STIX data and build a small KG store
  (use a simple local graph: networkx, or Neo4j optional)
- Add KGRetrievalTool:
  - given an entity (technique/tactic), fetch related nodes
  - return evidence snippets + references

Integrate into Supervisor routing:
- If question mentions attack techniques/tactics → use KG tool + citations.

Output:
- KG loader + docs
- 5 demo questions
- Tests for KG retrieval
```

(MITRE ATT&CK STIX sources: ([GitHub][4]))

---

## Day 9

### Day 9 Notes Prompt

```text
Day 9 — Fine-tuning with PEFT/LoRA + alignment

Teach:
- When to fine-tune vs RAG
- LoRA/PEFT basics (what changes, why it’s cheaper)
- Dataset creation: instruction pairs from domain data
- Eval: regression tests, safety checks, hallucination checks

Include:
- Interview Q&A (12)
- A “fine-tune plan” template (data → train → evaluate → deploy)
```

### Day 9 PoC Prompt

```text
Day 9 — Mini fine-tune experiment + integration plan

Implement a minimal LoRA fine-tune experiment using Hugging Face:
- Choose a small open model (lightweight) and fine-tune on:
  - CybersecurityEval Q&A style dataset OR your own curated Q&A
- Track experiment with MLflow (metrics + artifacts)
- Output a “model card” style summary + rollback plan

Do NOT overtrain; prioritize reproducibility and evaluation.
Output:
- training script
- eval script
- MLflow logging
- README “Fine-tune experiment”
```

(Example dataset: CyberSecurityEval ([Hugging Face][5]))

---

## Day 10

### Day 10 Notes Prompt

```text
Day 10 — Evaluation & quality loops (offline + online)

Teach:
- Retrieval eval (Recall@K, MRR) + answer faithfulness
- LLM-as-judge pitfalls + how to design robust eval
- Red-teaming: prompt injection, jailbreaks, data exfiltration
- Online eval: feedback capture, A/B testing, drift signals

Include:
- Interview Q&A (10)
- A practical eval checklist you’d use in production
```

### Day 10 PoC Prompt

```text
Day 10 — Build evaluation harness (RAG + agents)

Implement:
- offline eval runner:
  - dataset of queries + expected sources + expected “must include” facts
  - compute retrieval metrics + simple faithfulness checks
- prompt-injection test suite (10 attack prompts)
- CI gate: fail if metrics drop below threshold

Output:
- eval/ directory
- GitHub Actions workflow running tests + eval on PRs
- README badge section
```

---

## Day 11

### Day 11 Notes Prompt

```text
Day 11 — Observability & tracing for agentic systems

Teach:
- What to trace: prompts, tool calls, latencies, token/cost, errors
- Correlation IDs across services
- Debugging agent failures (where it went wrong)
- Privacy: logging redaction and PII handling

Include:
- Interview Q&A (10)
- “Production incident” story: how you’d troubleshoot a bad answer
```

### Day 11 PoC Prompt

```text
Day 11 — Add tracing + structured logs + metrics

Implement:
- OpenTelemetry tracing across API → agents → tools
- Structured JSON logs with correlation_id
- Metrics: request latency, tool latency, error count, tokens placeholder
- Simple local dashboards (Prometheus scrape endpoint + Grafana optional)

Output:
- instrumentation code
- docker-compose updates (if adding prometheus/grafana)
- runbook: “How to debug a bad response”
```

---

## Day 12

### Day 12 Notes Prompt

```text
Day 12 — MLOps end-to-end (CI/CD, registries, DVC/MLflow, retraining)

Teach:
- CI/CD for ML + LLM apps (code, prompts, models, data)
- MLflow vs DVC (what each tracks)
- Rollback strategies (model + prompt + retrieval index)
- Automated retraining triggers (data drift, feedback drop)

Include:
- Interview Q&A (12)
- A clean “MLOps architecture” diagram
```

### Day 12 PoC Prompt

```text
Day 12 — Add MLflow + DVC + release process

Implement:
- MLflow tracking for fine-tune + eval results
- DVC to version gold eval datasets + small sample corpora
- Release versioning:
  - app version
  - prompt registry version
  - index version
- Add a “release checklist” doc

Output:
- dvc.yaml + example storage config
- MLflow setup instructions
- CI pipeline with version bump + changelog snippet
```

---

## Day 13

### Day 13 Notes Prompt

```text
Day 13 — AWS deployment patterns for agentic AI (ECS/EKS/Lambda/SageMaker/Bedrock)

Teach:
- When to use Lambda vs ECS vs EKS vs SageMaker
- Bedrock integration patterns (guardrails, model choice, cost)
- Secrets/IAM best practices
- Storage: S3 for corpora + artifacts; Postgres for state

Include:
- Interview Q&A (10)
- “Cost + SLA” strategies
```

### Day 13 PoC Prompt

```text
Day 13 — Deploy to AWS (choose ECS or EKS) + S3 artifact store

Implement:
- Infrastructure (Terraform preferred):
  - VPC basics + ECS/EKS
  - RDS Postgres
  - S3 bucket for artifacts (eval sets, models, indexes)
- Build/push Docker image
- Deploy service + health check + logs

Output:
- Terraform modules (minimal but clean)
- Deployment steps + rollback steps
- “prod-ready” checklist
```

---

## Day 14

### Day 14 Notes Prompt

```text
Day 14 — Security & Responsible AI for enterprise agent systems

Teach:
- Prompt injection defense in depth
- Data access boundaries (tenant isolation)
- PII handling and logging redaction
- Policy: refusal patterns, safe completion, audit trails
- Cybersecurity domain: explain how your agent helps SOC without risky automation

Include:
- Interview Q&A (12)
- “Threat model” for your PoC
```

### Day 14 PoC Prompt

```text
Day 14 — Add security hardening + governance features

Implement:
- Auth (API key or JWT) + role-based controls
- Tenant/workspace isolation (enterprise vs security workspace)
- PII redaction middleware (basic patterns)
- Audit log table (who asked what, what tools ran, what data sources used)
- Safer action workflow: always require HITL approval for destructive actions

Output:
- Code + tests
- Threat model doc + mitigations
- Demo script for “secure mode”
```

---

## Day 15

### Day 15 Notes Prompt

```text
Day 15 — Interview readiness: system design + deep dive defense

Create:
1) A complete “system design walkthrough” of the PoC:
   - requirements → architecture → tradeoffs → failure modes → scaling
2) 2 versions of the story:
   - Yarnit (enterprise copilot + multi-agent + RAG + KG + observability)
   - Cybersecurity (SOC assistant + logs + IR workflows + AWS + MLOps)
3) Interview Q&A (20) with strong answers
4) A 5-minute verbal pitch + a 30-minute deep-dive outline
5) A “what I would improve next” roadmap (10 items)
```

### Day 15 PoC Prompt

```text
Day 15 — Polish to portfolio-grade + demo-ready release

Finalize:
- A demo UI (Streamlit or minimal web) with:
  - upload/ingest
  - chat
  - show citations + traces
  - “approval required” flow
- Add end-to-end demo datasets + scripted scenarios
- Add documentation:
  - architecture diagram
  - runbooks (deploy, debug, rollback)
  - evaluation report (latest metrics)
- Add “portfolio README” with screenshots placeholders + talking points

Output:
- final file tree
- final README (portfolio version)
- demo script: 3 scenarios (enterprise RAG, security triage, KG MITRE reasoning)
```

---

## How to use this efficiently (quick routine)

Daily:

1. Run **Notes Prompt** → read + extract 10 “I can explain” bullets
2. Run **POC Prompt** → implement, commit, and update README
3. Keep a **Daily Interview Log**: what you built + tradeoffs + metrics + failures fixed

[1]: https://www.kaggle.com/datasets/fcwebdev/synthetic-cybersecurity-logs-for-anomaly-detection?utm_source=chatgpt.com "Synthetic Cybersecurity Logs for Anomaly Detection"
[2]: https://www.kaggle.com/datasets/mehulkatara/windows-event-log?utm_source=chatgpt.com "Windows Event Log"
[3]: https://huggingface.co/datasets/darkknight25/Advanced_SIEM_Dataset?utm_source=chatgpt.com "darkknight25/Advanced_SIEM_Dataset · Datasets at ..."
[4]: https://github.com/mitre-attack/attack-stix-data?utm_source=chatgpt.com "STIX data representing MITRE ATT&CK"
[5]: https://huggingface.co/datasets/CyberNative/CyberSecurityEval?utm_source=chatgpt.com "CyberNative/CyberSecurityEval · Datasets at Hugging Face"

---
### Master PoC Spec Prompt
- A single “Master PoC Spec Prompt” that you can paste anytime to regenerate the full repo architecture and ensure each day stays aligned.

```text
MASTER PoC SPEC PROMPT — “Agentic Enterprise Copilot” (Yarnit + Cybersecurity AI/ML)

You are acting as:
- Staff/Principal AI Engineer (Agentic GenAI + RAG + MLOps)
- Production Backend Engineer (FastAPI + testing + clean architecture)
- System Design Interview Coach (explain trade-offs, failure modes, scaling)
- MLOps/Cloud Engineer (AWS, CI/CD, observability, IaC)

========================
0) CONTEXT + GOAL (STRICT)
========================
I am preparing for TWO target roles:
(1) Senior AI Engineer (Generative AI) — multi-agent, advanced RAG (vector + SQL + KG), fine-tuning (PEFT/LoRA), safety/alignment, tracing/observability, eval loops.
(2) AI/ML Engineer (Agentic AI for Cybersecurity) — agentic workflows for logs/anomaly/incident response, strong AWS (Bedrock/SageMaker/Lambda/EKS/ECS), CI/CD, MLflow/DVC, Terraform, monitoring, and basic cybersecurity/SIEM context.

Goal by Day 15:
- A production-grade portfolio PoC that I can demo and defend in interviews.
- I can clearly explain architecture, trade-offs, evaluation, observability, and deployment.

========================
1) INPUTS (YOU MUST WORK EVEN IF SOME ARE MISSING)
========================
- CurrentDay: <1..15>  (If not provided, assume CurrentDay=1)
- DeploymentTarget: <local|aws-ecs|aws-eks> (default local)
- LLMProvider: <mock|openai|aws-bedrock|hf> (default mock + pluggable)
- VectorStore: <chroma|qdrant|pinecone> (default chroma local OR qdrant local)
- GraphStore: <networkx|neo4j> (default networkx local)
- Database: <postgres> (default postgres)
- RepoName: agentic-enterprise-copilot
- AnyExistingState: (optional) bullet list of what already exists; if empty, generate full repo from scratch.

IMPORTANT:
- Do NOT ask me follow-up questions.
- Make best assumptions and proceed.
- If choices are ambiguous (e.g., chroma vs qdrant), pick ONE and justify briefly.

========================
2) PROJECT DEFINITION (SOURCE OF TRUTH)
========================
Project: “Agentic Enterprise Copilot”
A multi-agent orchestration service with two “domains”:
A) Enterprise Knowledge Copilot (Yarnit-style)
B) Security Ops Copilot (Cybersecurity-style)

Core capabilities (end-state):
- FastAPI backend (clean architecture, Pydantic settings, structured logging)
- Multi-agent workflow (LangGraph-style state machine recommended)
  - SupervisorAgent + SpecialistAgents
  - Tools: VectorRetrievalTool, SQLLogQueryTool, KGRetrievalTool (MITRE ATT&CK), SummarizeTool, IncidentReportTool
  - HITL (Human-in-the-loop) approvals for risky actions
- Advanced RAG:
  - Vector retrieval + metadata filters + citations
  - SQL agent (safe text-to-SQL) for grounded log analytics
  - Knowledge Graph retrieval (MITRE ATT&CK STIX) + hybrid (KG->candidates->vector rerank)
- Evaluation:
  - Offline eval harness (retrieval metrics + basic faithfulness checks)
  - Prompt injection test suite
  - CI gate to block regressions
- Observability:
  - OpenTelemetry traces, correlation IDs
  - Metrics (latency/tool latency/errors) via Prometheus endpoint
  - Structured JSON logs with redaction
- MLOps:
  - MLflow experiment tracking for fine-tune + eval
  - DVC for datasets/golden sets
  - Release versioning (app + prompts + index)
- Security/Governance:
  - Auth (API key or JWT)
  - Tenant/workspace isolation (enterprise vs security)
  - Audit log (who asked what, what tools ran, what sources used)
  - PII redaction in logs
- AWS (if DeploymentTarget != local):
  - IaC via Terraform (minimal clean modules)
  - Container deployment (ECS/EKS)
  - S3 artifact store (indexes, eval sets, model artifacts)
  - CloudWatch logs (optional)

========================
3) NON-FUNCTIONAL REQUIREMENTS (STRICT)
========================
- Production-grade structure: modular services, clear boundaries, testability.
- Deterministic tests: use MockLLM for unit/integration tests.
- Safe defaults: refuse unsafe requests; require HITL approval for actions.
- Observability-first: every request has correlation_id and trace spans.
- Documentation-first: README + architecture.md + runbooks + threat_model.md.

========================
4) OUTPUT FORMAT (STRICT)
========================
You MUST produce outputs in this order:

A) “Executive Summary” (10–15 lines)
   - what this repo is, what it demonstrates for each JD

B) “Architecture” section
   - 2 ASCII diagrams:
     1) high-level system
     2) agent graph (Supervisor + tools)
   - Clear trade-offs: vector vs SQL vs KG; why multi-agent; why these stores

C) “Repo File Tree” (final expected tree for CurrentDay end-state)
   - Mark items as: [NOW] implement today, [LATER] planned, [OPTIONAL]

D) “API Contract”
   - Endpoints, request/response examples (JSON)
   - Error model, auth header format, correlation_id

E) “Implementation Plan for CurrentDay”
   - Exact tasks to implement today
   - Definition of Done (DoD)
   - Tests to add today (min 3)
   - Demo steps (curl or scripts)

F) “Code Artifacts”
   - Provide COMPLETE code for all [NOW] files.
   - For [LATER] files, provide stubs with TODOs (only if needed to compile/import).
   - Code must be copy-paste ready.

G) “Commands”
   - Local run commands (docker compose, migrations, tests)
   - Optional AWS deploy commands if DeploymentTarget is aws-*

H) “Interview Defense Pack”
   - 12 interview questions + strong answers tied to this repo
   - 8 talking points (story)
   - 6 failure modes + mitigations
   - 5 metrics I track and why

========================
5) CODING RULES (STRICT)
========================
- Language: Python 3.11+
- Framework: FastAPI
- Tests: pytest
- DB: Postgres (SQLAlchemy or SQLModel acceptable; pick one)
- Config: Pydantic Settings (.env)
- Logging: structured JSON logs + correlation_id middleware
- LLM: LLMProvider interface + MockLLM + one Real provider skeleton (optional)
- Agents: use LangGraph-like pattern; if you can’t use the library, implement a simple state machine with typed state.
- Retrieval: pick VectorStore (default chroma or qdrant local) and implement wrapper.
- SQL Agent: safe validator (allowlist tables/columns, block DDL/DML, LIMIT)
- KG: load subset of MITRE ATT&CK STIX; store in networkx; retrieval tool returns evidence + references.
- Security: do not implement offensive capabilities; focus on analysis, triage, recommendations, and HITL.

========================
6) DAY ALIGNMENT RULE (IMPORTANT)
========================
Given CurrentDay, implement up to that day’s capabilities and ensure earlier parts are solid.
If AnyExistingState is provided, align with it (don’t delete; refactor safely).

Suggested day progression (you must enforce logically):
Day 1: repo skeleton + FastAPI + settings + logging + docker + tests
Day 2: ingestion + Postgres schema + dataset loader
Day 3: prompt templates + provider abstraction + safety gate
Day 4: RAG v1 + indexing + citations + basic eval script
Day 5: tools + router agent
Day 6: multi-agent Supervisor + HITL
Day 7: safe SQL agent
Day 8: KG retrieval (MITRE)
Day 9: LoRA fine-tune experiment + MLflow tracking
Day 10: eval harness + CI gate
Day 11: tracing + metrics dashboard basics
Day 12: DVC + release versioning
Day 13: AWS deploy + S3 artifact store + IaC
Day 14: auth + tenant isolation + audit logs + threat model
Day 15: demo UI + runbooks + final portfolio README + interview defense

========================
7) START NOW
========================
Use:
- CurrentDay: <fill if provided else 1>
- DeploymentTarget: <fill if provided else local>
- LLMProvider: <fill if provided else mock>
- VectorStore: <fill if provided else chroma OR qdrant local>
- GraphStore: networkx
- Database: postgres

Now generate the full “Source of Truth” output as per section 4, and ensure it is aligned to CurrentDay and the two JDs.
```
---
### “Daily Delta Prompt”
- (smaller than this master spec) That you run each day after you commit code—so it tells you exactly what to change next without reprinting the whole repo.

```text
DAILY DELTA PROMPT — “Agentic Enterprise Copilot” (Post-Commit Next-Step Guide)

You are my Staff AI Engineer + MLOps Architect + Interview Coach.

I have committed today’s changes to my repo: “agentic-enterprise-copilot”.
Your job is to tell me the NEXT DAY delta (what to add/modify) without reprinting the whole repo.

========================
INPUTS (I will paste these)
========================
1) CurrentDay: <1..15>
2) What I completed today (bullet list)
3) Current repo file tree (output of: tree -L 4)
4) Key diffs (optional): 
   - git diff --stat
   - OR list of changed files
5) Current issues/errors (if any)
6) DeploymentTarget: <local|aws-ecs|aws-eks> (default local)
7) LLMProvider: <mock|openai|aws-bedrock|hf> (default mock)

IMPORTANT:
- Do NOT ask follow-up questions.
- If inputs are missing, make best assumptions and proceed.
- Keep output focused: delta only.

========================
YOUR OUTPUT (STRICT)
========================
A) Day Alignment Check (5–10 bullets)
- Verify my repo is still aligned to the 2 target JDs:
  (1) Yarnit multi-agent + advanced RAG (vector/SQL/KG) + safety + tracing + eval
  (2) Cybersecurity agentic AI + AWS + MLOps + CI/CD + monitoring
- Call out what’s missing or drifting.

B) Next-Day Goal (3 lines)
- One clear objective statement for tomorrow.
- What “done” looks like.

C) Delta Plan (the actionable changes)
1) Add / Modify (exact)
   - list the exact files to create/modify (max 12)
   - for each file: what changes, why it’s needed
2) API changes (if any)
   - endpoint additions/changes + request/response examples
3) Data/Schema changes (if any)
   - migration steps (Alembic or SQL) and backward compatibility notes
4) Tests to add (min 3)
   - specify test names, what to mock, what to assert
5) Observability updates (if any)
   - what to log/trace/measure tomorrow

D) Copy-Paste Tasks (Jira-style checklist)
- 10–20 checkbox items, ordered, each small (15–45 min)
- Each task must be verifiable.

E) “Definition of Done” (DoD)
- 6–10 acceptance criteria, including:
  - tests passing
  - run commands
  - demo scenario works
  - failure mode handling

F) Demo Script for Tomorrow
- 5–8 steps using curl/Makefile/CLI to prove it works

G) Interview Upgrade (small but high value)
- 5 talking points I can use in interviews based on tomorrow’s delta
- 5 likely questions + strong 2–4 line answers

========================
RULES (STRICT)
========================
- Do NOT output full repo code unless I explicitly ask.
- If code is needed, output ONLY:
  - small patch snippets (<= 80 lines total) OR
  - pseudo-code for complex sections.
- Prefer best-practice choices (testing, security, observability).
- If I’m behind schedule, propose a “catch-up” delta that keeps Day 15 goal feasible.
- Always include at least 1 safety/guardrail improvement OR 1 eval/observability improvement in the delta (even if small).

========================
START
========================
Based on my pasted inputs, generate the Daily Delta output now.
```
---

