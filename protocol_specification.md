# HF-IQR Protocol Specification

## Purpose

This document provides a complete specification
for reproducing the HF-IQR council run.
It covers environment setup, execution sequence,
checkpoint management, and session recovery.

---

## Environment Requirements
Python 3.10+
Google Colab (recommended) or local Jupyter
Required packages:
anthropic
openai
google-genai
requests
json (standard library)
hashlib (standard library)
time (standard library)
API keys required:
ANTHROPIC_API_KEY
OPENAI_API_KEY
GEMINI_API_KEY
DEEPSEEK_API_KEYS
XAI_API_KEY
Optional (local models):
NGROK_URL (Ollama server via NGROK)

---

## Model Registry

```python
MODEL_REGISTRY = {
    "claude":   "claude-sonnet-4-5",
    "gpt4o":    "gpt-4o",
    "gemini":   "gemini-2.5-pro",
    "deepseek": "deepseek-chat",
    "grok":     "grok-3",
    "local":    "mistral-nemo:latest"
}
```

---

## Peer Assignments

Fixed and pre-registered. Do not modify
between rounds or sessions.

```python
PEER_ASSIGNMENTS = {
    "claude":   "grok",
    "gpt4o":    "claude",
    "gemini":   "gpt4o",
    "deepseek": "gemini",
    "grok":     "deepseek"
}
```

---

## Execution Sequence

### Phase 1 — Setup (Cells 1-7)
Cell 1: Mount Drive, configure paths,
initialize session metadata
Cell 2: Install packages
Cell 3: Load API keys, initialize clients
Cell 4: Load model registry and peer assignments
Cell 5: Initialize CheckpointManager
Cell 6: Initialize BudgetGuardian
Cell 7: Load master dataset, verify SHA256 hash
against pre-registration

Dataset integrity check must pass before
any API calls are made. If hash verification
fails do not proceed — the dataset may have
been modified after pre-registration.

### Phase 2 — Round 1 (Cells 8-9)
Cell 8: Define query_model_round1() function
Cell 9: Execute Round 1 runner
For each question in ALL_QUESTIONS:
For each model in SUBJECT_MODELS:
Call query_model_round1(model, question)
Append result to round1_results
checkpoint_mgr.save("round1_{category}")
budget.log_call(...)
time.sleep(1)

Run category by category:
Change CURRENT_CATEGORY for each run.
Order: adversarial → logical_syllogism →
causal_chain → probabilistic →
quantum_reasoning → frontier_reasoning

### Phase 3 — Round 2 (Cells 10-11)
Cell 10: Define query_model_round2() function
Cell 11: Execute Round 2 runner
Build r1_lookup from Round 1 results
For each question:
For each model:
peer = PEER_ASSIGNMENTS[model]
peer_response = r1_lookup[qid][peer]
Call query_model_round2(model, question,
peer_response)
Checkpoint after every call

### Phase 4 — Round 3 (Cells 12-13)
Cell 12: Define query_model_round3() function
Cell 13: Execute Round 3 runner
Build r2_lookup from Round 2 results
For each question:
For each model:
own_response = r1_lookup[qid][model]
critique = r2_lookup[qid][model]
Call query_model_round3(model, question,
own_response,
critique)
Detect DEFENDING or REVISING in response
Checkpoint after every call

### Phase 5 — Mirror (Cells 14-15)
Cell 14: Define query_model_mirror() function
Cell 15: Execute mirror runner
For each question:
For each model:
own_response = r1_lookup[qid][model]
peer = PEER_ASSIGNMENTS[model]
peer_response = r1_lookup[qid][peer]
ground_truth = question["ground_truth"]
Call query_model_mirror(model, question,
own_response,
peer_response,
confidence)
Checkpoint after every call

### Phase 6 — Scoring (Cells 16-19)
Cell 16: ESVR scoring on all Round 1 responses
Cell 17: DSS scoring on all Round 3 responses
Cell 18: CVS scoring on all Round 2 critiques
Cell 19: RI event detection across Round 3

### Phase 7 — Reporting (Cells 20-22)
Cell 20: Cost and token report
Cell 21: Final analysis report
Cell 22: Export verification and manifest

---

## Checkpoint System

The CheckpointManager saves after every
individual API call. This means:

- If the session fails at call 187 of 300
  the run resumes from call 188 automatically
- No data is lost on session timeout
- Resume by running the same runner cell
  The auto_resume() method detects completed calls

```python
# Auto-resume pattern
can_resume, completed, results = (
    checkpoint_mgr.auto_resume(
        checkpoint_name, total_expected))

if can_resume:
    # Skip already completed calls
    completed_ids = set(
        f"{r['question_id']}_{r['model']}"
        for r in results)
```

Checkpoint files are saved to:
/content/drive/MyDrive/HF_IQR/
council_run_v1/checkpoints/

---

## Session Recovery

If a new Colab session is started mid-run
the following variables must be rebuilt:

```python
# Run this recovery cell before resuming

# 1. Reload Round 1 consolidated file
with open(f"{RESULTS_PATH}/
          round1_all_responses.json", 'r') as f:
    r1_data = json.load(f)
all_round1 = r1_data["responses"]

# 2. Rebuild r1_lookup
r1_lookup = {}
for r in all_round1:
    qid = r["question_id"]
    model = r["model"]
    if qid not in r1_lookup:
        r1_lookup[qid] = {}
    r1_lookup[qid][model] = r["response"]

# 3. Reload Round 2 results
round2_results = checkpoint_mgr.load(
    "round2_results")

# 4. Rebuild r2_lookup
r2_lookup = {}
for r in round2_results:
    qid = r["question_id"]
    peer = r["peer_reviewed"]
    if qid not in r2_lookup:
        r2_lookup[qid] = {}
    r2_lookup[qid][peer] = r["critique"]

# 5. Reload Round 3 if needed
round3_results = checkpoint_mgr.load(
    "round3_results")
```

---

## Budget Management

The BudgetGuardian tracks costs without
halting execution. It warns at 50% and 80%
of the configured budget limit.

```python
# Cost per 1000 tokens (approximate)
COST_PER_1K = {
    "claude":   0.003,
    "gpt4o":    0.005,
    "gemini":   0.002,
    "deepseek": 0.001,
    "grok":     0.005,
    "local":    0.000
}
```

Actual v1.0 costs:
Round 1: $1.49
Round 2: $3.41
Round 3: estimated ~$1.50
Mirror:  $3.30
Total:   $9.33

---

## Data File Locations
/content/drive/MyDrive/HF_IQR/
/dataset/
HF_IQR_Master_Dataset_v1.json
HF_IQR_Preregistration_v1.json
HF_IQR_Preregistration_v1_HASH.txt
/council_run_v1/
round1_all_responses.json
round2_all_responses.json
budget_log.json
export_manifest.json
final_analysis_report.json
integrity_record.json
meta_council_full_report.json
/checkpoints/
  round1_adversarial.json
  round1_logical_syllogism.json
  round1_causal_chain.json
  round1_probabilistic.json
  round1_quantum_reasoning.json
  round1_frontier_reasoning.json
  round2_results.json
  round3_results.json
  mirror_results.json

---

## Integrity Verification

To verify data integrity after download:

```python
import hashlib
import json

# Verify dataset
with open('HF_IQR_Master_Dataset_v1.json',
          'r') as f:
    content = f.read()
hash_value = hashlib.sha256(
    content.encode()).hexdigest()

expected = "9b02ba527720b55e0552410375186c4e"
assert hash_value.startswith(expected[:16]), \
    "Dataset hash mismatch"
print("Dataset integrity verified ✅")
```

---

## Known Issues and Workarounds

**Gemini token inflation**
Gemini 2.5-pro uses thinking tokens that
inflate total token count significantly.
Use total_token_count not the sum of
prompt and candidates token counts.

```python
# Correct Gemini token extraction
tokens = r.usage_metadata.total_token_count
```

**NGROK 404 error**
Include the browser warning bypass header:
```python
headers = {
    "ngrok-skip-browser-warning": "true",
    "Content-Type": "application/json"
}
```

Use /api/generate not /v1/chat/completions
for Ollama endpoints.

**Round 3 output cleared accidentally**
Use checkpoint recovery cell.
All data is preserved in Drive checkpoints
regardless of Colab output display
