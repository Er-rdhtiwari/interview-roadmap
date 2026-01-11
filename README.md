## Non-negotiable rules (keep you on the roadmap)

1. **One day = one outcome**

* Each day must end with: **(a) working demo**, **(b) tests updated**, **(c) README updated**, **(d) one clean commit**.

2. **No “extra exploring”**

* Do **only** what that day requires.
* If you feel tempted: write it in a **“Later Ideas”** section and move on.

3. **Always work in this order**

* **Notes Prompt → extract interview bullets → PoC Prompt → implement → verify → commit → daily summary**
  (If you code first, you’ll drift.)

4. **Always keep it testable**

* Use **MockLLM** for tests by default.
* Never block progress because a real LLM provider isn’t ready.

5. **Don’t overbuild UI / AWS early**

* UI is mainly **Day 15**
* AWS deployment is mainly **Day 13**
* Fine-tuning heavy work is **Day 9**
* Everything before that is: correctness + structure + eval + safety + observability.

---

## The “State Pack” you paste at the start of EVERY new chat

Copy this template and fill it daily:

```text
DAY X — Agentic Enterprise Copilot

Target JDs:
1) Yarnit: multi-agent + advanced RAG (vector/SQL/KG) + safety + eval + observability
2) Cyber AI/ML: agentic workflows for security logs + AWS + MLOps + CI/CD + monitoring

Where I am now (from yesterday):
- Completed:
  - ...
- Not done / known gaps:
  - ...
- Current issues/errors:
  - ...

Repo evidence:
1) tree -L 4 (paste here)
2) How to run locally (paste commands)
3) Tests status: (e.g., pytest summary)
4) Latest demo proof: (curl commands + outputs OR short notes)
```

This prevents the “new chat reset problem” and keeps me aligned.

---

## Daily chat flow (exact paste order)

### Step 1 — Paste State Pack (above)

This sets context in the new chat.

### Step 2 — Paste the day’s **Notes Prompt**

* Tell me: “Give me notes + Q&A ONLY. No code yet.”
* Then you read and extract **10 bullets**.

### Step 3 — Paste this “Extraction mini-prompt”

```text
From the notes you just generated:
1) Give me the top 10 “I can explain in interview” bullets (one line each)
2) Give me 5 likely interview questions for this day + strong 3–5 line answers
3) Give me 3 pitfalls and how to avoid them in my PoC today
Keep it concise.
```

### Step 4 — Paste the day’s **POC Prompt**

Add one line at the top:

```text
IMPORTANT: Do NOT reprint full repo. Only give TODAY’S delta: exact files to create/modify + acceptance checklist.
```

### Step 5 — You implement locally

When stuck, paste:

* The **exact error**
* The **file + function name**
* The **command you ran**
* The **last 30–50 lines of logs**

### Step 6 — End-of-day “Closeout prompt” (very important)

```text
End of Day X Closeout:
1) Summarize what I built today in 8 bullets (interview-ready)
2) List 5 tradeoffs/decisions I should mention
3) List 5 failure modes I handled or should handle next
4) Provide tomorrow’s short “demo script” (5–8 steps)
5) Provide a commit message suggestion + README section title
Keep it tight and aligned to Day X scope.
```

### Step 7 — Commit + store proof

Your daily proof artifacts:

* `tree -L 4`
* `pytest -q`
* 2–3 curl commands showing it works
* A short “what changed today” note

---

## How to use Master PoC Spec Prompt vs Daily Delta Prompt

### Use **Master PoC Spec Prompt** when:

* Day 1 kickoff (fresh repo)
* You feel lost or the repo structure drifted
* You want a “source of truth” refresh

**How to paste it in a new chat:**

* Paste **State Pack**
* Paste **Master PoC Spec Prompt**
* Set `CurrentDay = X` and include your `AnyExistingState` bullets
* Add: “Output TODAY’s [NOW] items + stubs only; avoid full repo repeats unless needed.”

### Use **Daily Delta Prompt** every day after you commit:

* Next morning, new chat
* Paste **State Pack**
* Paste **Daily Delta Prompt**
* Include: `CurrentDay`, what completed, tree, diff summary, issues.

This keeps each day small and controlled.

---

## Day-by-day “what to paste” checklist (Day 1 → Day 15)

### Day 1 (Repo foundation)

Paste in order:

1. State Pack (even if empty)
2. **Day 1 Notes Prompt**
3. Extraction mini-prompt
4. **Day 1 PoC Prompt**
5. Closeout prompt

**Checkpoint:** FastAPI runs + tests pass + Docker works + clean structure.

---

### Day 2 (Ingestion + Postgres schema + dataset loader)

Paste in order:

1. State Pack
2. Day 2 Notes Prompt
3. Extraction mini-prompt
4. Day 2 PoC Prompt (delta only)
5. Closeout prompt

**Checkpoint:** `/ingest` loads docs/logs → Postgres tables updated → 3 tests.

---

### Day 3 (Prompt templates + provider abstraction + basic guardrails)

Same paste order.

**Checkpoint:** `/chat` uses templates + prompt registry version + basic injection refusal + tests with MockLLM.

---

### Day 4 (RAG v1)

Same paste order.

**Checkpoint:** indexing + retrieval + citations + minimal offline eval script runs.

---

### Day 5 (Tools + router agent)

Same paste order.

**Checkpoint:** tools are typed + router chooses path deterministically in tests.

---

### Day 6 (Multi-agent supervisor + HITL)

Same paste order.

**Checkpoint:** supervisor + specialist agents + approval_required flow + integration tests.

---

### Day 7 (Safe SQL agent)

Same paste order.

**Checkpoint:** text-to-SQL guarded by allowlist + read-only + LIMIT; returns structured results.

---

### Day 8 (Knowledge Graph retrieval: MITRE)

Same paste order.

**Checkpoint:** MITRE KG loaded + KG tool answers technique/tactic questions with references.

---

### Day 9 (LoRA fine-tune experiment + MLflow)

Same paste order.

**Checkpoint:** training script + eval script + MLflow logs + model card summary. (Keep small.)

---

### Day 10 (Eval harness + CI gate)

Same paste order.

**Checkpoint:** eval runner + injection tests + GitHub Actions gate.

---

### Day 11 (Tracing + metrics)

Same paste order.

**Checkpoint:** correlation_id + OpenTelemetry spans + Prometheus endpoint + runbook snippet.

---

### Day 12 (DVC + release versioning)

Same paste order.

**Checkpoint:** DVC tracks eval set + versioning approach documented + release checklist.

---

### Day 13 (AWS deploy + S3 artifact store + IaC)

Same paste order, but add:

* Terraform plan/apply logs (sanitized)
* AWS target chosen (ECS or EKS)

**Checkpoint:** service deployed + health endpoint + logs visible + rollback steps written.

---

### Day 14 (Auth + tenant isolation + audit logs + threat model)

Same paste order.

**Checkpoint:** auth enforced + workspace separation + audit logs + threat_model.md.

---

### Day 15 (Demo UI + portfolio polish)

Same paste order.

**Checkpoint:** demo UI + scripted scenarios + final README + architecture diagram + interview defense pack.

---

## “Anti-deviation” checkpoints (use daily)

At the end of each day, answer YES/NO:

* ✅ Did I ship a working demo?
* ✅ Did I add/adjust at least 3 tests?
* ✅ Did I update README/runbook?
* ✅ Did I keep scope limited to today’s plan?
* ✅ Can I explain today’s work in 2 minutes?
  If any is NO → tomorrow’s first task is to fix it (before adding new features).

---

## If you want the simplest daily instruction

In every new chat, start with:

1. **State Pack**
2. “I am on Day X. First generate Notes using Day X Notes Prompt only.”
3. Paste Notes Prompt
4. “Now generate today’s PoC delta only.”
5. Paste PoC Prompt
6. End with Closeout prompt

That’s it.

When you start **Day 1**, paste your State Pack (even if it’s empty) and say “Day 1 start—Notes first.”
