# HF-IQR Methodology

## Overview

HF-IQR measures reasoning process quality across
frontier AI models using a four round deliberation
protocol with pre-registered scoring metrics.

The core premise is that answer correctness alone
is an insufficient measure of reasoning quality.
A model that gets the right answer through flawed
reasoning is more dangerous than a model that gets
the wrong answer while reasoning correctly — because
the flawed reasoner will fail unpredictably on novel
problems while the sound reasoner can be corrected.

---

## Design Philosophy

### Three Core Principles

**Principle 1 — Process over outcome**
Every question requires a numbered reasoning chain.
Scoring evaluates the chain not the conclusion.
A correct answer with circular reasoning scores lower
than an incorrect answer with valid inference steps.

**Principle 2 — Pressure testing**
Round 1 independent responses establish a baseline.
Rounds 2 and 3 apply deliberation pressure through
anonymous peer critique and forced defense or revision.
Resilience under pressure is a distinct and measurable
property from initial reasoning quality.

**Principle 3 — Anonymized peer review**
All Round 2 critiques are delivered without revealing
the identity of the model being reviewed.
This eliminates brand authority bias — a model cannot
defer to another model's reputation, only to the
quality of the reasoning chain in front of it.

---

## Question Design

### PRR Triplet Format

Every question is structured as a PRR triplet:
Prompt:
The question itself including any
relevant data, scenario, or framing.
May contain embedded traps, false premises,
or misleading authority framing.
Reasoning Request:
Standardized instruction requiring:

Numbered inference steps
Step derivation citations
Explicit trap detection
Confidence percentage
Single evidence threshold statement

Reference Answer:
Ground truth locked before data collection.
Includes correct answer, key insight,
named failure mode, and common model errors.

### Question Categories

**Adversarial (10 questions, difficulty 3-5)**
Tests trap detection and premise verification.
Subtypes: implicit contradiction, false premise,
linguistic red herring, recursive trap,
meta-adversarial.

**Logical Syllogism (10 questions, difficulty 2-5)**
Tests formal logical reasoning.
Distinguishes validity from soundness.
Tests whether models accept conclusions from
invalid arguments when conclusions seem plausible.

**Causal Chain (10 questions, difficulty 2-5)**
Tests root cause identification.
Distinguishes proximate from structural causes.
Tests whether models commit fundamental attribution
error — blaming individuals for system failures.

**Probabilistic (10 questions, difficulty 2-5)**
Tests Bayesian reasoning and base rate application.
Named failure modes: base rate neglect,
prosecutor's fallacy, conjunction fallacy,
expected utility confusion.

**Quantum Reasoning (10 questions, difficulty 3-5)**
Tests formal quantum mechanics reasoning.
All ground truth derivable from standard
quantum mechanics without interpretation.
Named failure modes: Born rule confusion,
FTL communication myth, uncertainty principle
misattribution, separability proof omission.

**Frontier Reasoning (10 questions, difficulty 3-5)**
Tests reasoning about contested scientific and
philosophical claims.
Ground truth limited to characterizing positions
accurately — not adjudicating between them.
Named failure modes: philosophical relativism
from physics, theory/hypothesis confusion,
design inference from fine tuning.

### Difficulty Scale
2 — Standard reasoning required
No significant traps
Clear ground truth
3 — Moderate trap embedded
Requires careful premise checking
One failure mode present
4 — Significant trap or complexity
Multiple failure modes possible
Formal reasoning required
5 — Maximum difficulty
Multiple traps or deep complexity
Formal proof or calculation required
Most models expected to partially fail

---

## Four Round Protocol

### Round 1 — Independent Response

Each model receives the question prompt plus the
standardized reasoning request.
No model sees any other model's response.
Responses are collected and stored before
any model proceeds to Round 2.

Output measured: ESVR

### Round 2 — Anonymous Cross-Examination

Each model receives:
- The original question
- One peer model's Round 1 response
  labeled Peer A with no identity revealed

Task: Identify the single weakest inference step
in Peer A's reasoning chain. Cite the step number.
Provide alternative reasoning if the step is wrong.
If the reasoning is sound state that explicitly.

Peer assignments are rotating and fixed:
Claude   reviews Grok
GPT-4o   reviews Claude
Gemini   reviews GPT-4o
DeepSeek reviews Gemini
Grok     reviews DeepSeek

Output measured: CVS

### Round 3 — Defense or Revision

Each model receives:
- The original question
- Its own Round 1 response
- The critique it received in Round 2

Task: Begin response with DEFENDING or REVISING.
If DEFENDING: provide explicit logical
counter-argument citing specific steps.
If REVISING: acknowledge what the critique
revealed and explain what changed and why.

Output measured: DSS, RI Events

### Round 4 — Mirror Self-Assessment

Each model receives:
- Its own Round 1 response
- The ground truth answer
- One peer model's Round 1 response anonymously

Task: Answer four structured questions:
A. Where did your reasoning succeed and fail
   compared to ground truth?
B. Was your original confidence accurate?
   What confidence would you assign now?
C. What was the peer's strongest and weakest step?
D. What single change would most improve your
   original response?

Output measured: Confidence recalibration,
self-assessment accuracy

---

## Pre-Registration

Pre-registration was filed before any data collection.
The pre-registration document locked:

- Four formal hypotheses
- All scoring formulas with exact parameters
- Complete protocol specification
- Dataset integrity hash
- Model registry with exact version strings

The tamper-evident chain consists of three timestamps:
Pre-registration filed: 2026-05-02T20:19:02Z
Council run started:    2026-05-04T20:43:27Z
Results locked:         2026-05-04T23:55:34Z

SHA256 hashes:
Dataset:        9b02ba527720b55e0552410375186c4e
Pre-reg:        3400153ee46e02df73b24ea4f2206fb7
Results:        76d3c6cc6d161583695f9d50f53f7ae7
85a9ed24bef24becdf573ca662723d4b

---

## Human Baseline

The researcher completed 8 questions across
4 categories before the full council run.
Questions were answered blind with no access
to ground truth or model responses.

Results documented in session notes.
Pattern identified: Strong on trap detection
and logical structure. Growth area on formal
probability calculation and base rate application.

This baseline serves as a reference point for
interpreting model performance relative to
a non-specialist human reasoner.

---

## Infrastructure
Execution environment: Google Colab
Data storage: Google Drive (sovereign)
Checkpoint system: After every API call
Session recovery: Auto-resume from checkpoint
Hudson Forge IRMB-C cluster:
The Architect: RTX 5070
Agent 5: NucBox M6 Ultra 32GB
Scout: Raspberry Pi 5 8GB
Local models: Mistral-Nemo, DeepSeek-R1:14b,
Gemma3:4b, LLaMA3:8b

---

## Limitations

**Scoring subjectivity**
ESVR and CVS use heuristic rule-based scoring.
Inter-rater reliability metrics (Cohen's kappa)
were not calculated for v1.0.
This is identified as the primary limitation
and is addressed in V2 planning.

**Ground truth scope**
Ground truth was written by the researcher.
External validation of ground truth answers
was not completed for v1.0.

**Parse failure rate**
ESVR parse failure rate was 6.7%.
Within the pre-registered 15% threshold
but introduces measurement noise.

**Model version drift**
Models improve over time.
Results reflect specific model versions
at a specific point in time.
Longitudinal re-running is planned for V2.

---

## Reproducibility

All code is available in the repository notebooks.
All data is available on Hugging Face.
The pre-registration establishes the protocol
that was followed before data collection.

To reproduce:
1. Load HF_IQR_Master_Dataset_v1.json
2. Verify dataset hash against pre-registration
3. Run HF_IQR_Council_Run_v1.ipynb
4. Compare results hash against published record
