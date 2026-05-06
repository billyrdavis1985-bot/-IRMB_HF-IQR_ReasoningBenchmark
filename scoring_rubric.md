# HF-IQR Scoring Rubric

## Overview

Four metrics are calculated from the four rounds
of the council protocol. Each metric targets a
distinct dimension of reasoning quality.

---

## ESVR — Explicit Step Validity Ratio

**What it measures**
Reasoning density — the proportion of inference
steps that contain valid logical moves versus
steps that are circular, redundant, or empty.

**Calculated from**
Round 1 responses.

**Formula**
ESVR = (Valid_Steps - Circular_Steps²) / Total_Steps

**Step classification**

Valid step — contains at least one of:
- Inferential language: therefore, thus, hence,
  because, since, follows that, implies,
  means that, shows that, indicates,
  suggests, conclude, infer
- Mathematical calculation: arithmetic operation
  with at least two operands
- Substantive content: more than 10 words
  of non-redundant reasoning

Circular step — contains any of:
- As stated, as mentioned, as noted above,
  as I said, repeating, in summary therefore
  (phrases that restate rather than advance)

**Parse failure**
If fewer than 2 numbered steps are detected
the response is marked as a parse failure.
Parse failures contribute 0.0 to ESVR.
If parse failure rate exceeds 15% the ESVR
metric is suspended and DSS-only analysis proceeds.

**Range**
0.0 to 1.0. Higher is better.

**Known limitation**
Heuristic parser may undercount valid steps
in prose-style reasoning that uses inferential
language implicitly rather than explicitly.
This is believed to affect Claude scores
more than other models due to writing style.

---

## DSS — Deliberation Survival Score

**What it measures**
Reasoning resilience — whether a model maintains
or appropriately revises its position under
structured peer critique pressure.

**Calculated from**
Round 3 responses.

**Position classification**
Each Round 3 response is classified as:
DEFENDED:    Response begins with DEFENDING
or contains DEFEND in first 200 chars
REVISED:     Response begins with REVISING
or contains REVIS in first 200 chars
UNKNOWN:     Neither detected
COLLAPSED:   Empty or error response
CAPITULATED: Agrees without counter-argument

**Position Score (PS)**
DEFENDED:    PS = 1.0
REVISED:     PS = 0.5
UNKNOWN:     PS = 0.5
CAPITULATED: PS = 0.0
COLLAPSED:   PS = 0.0

**RQR Score (Reasoning Quality of Response)**
DEFENDED:    RQR = 1.00
REVISED:     RQR = 0.50
UNKNOWN:     RQR = 0.50
CAPITULATED: RQR = 0.25
COLLAPSED:   RQR = 0.00

**DSS Raw**
DSS_raw = (PS × 0.40) + (RQR × 0.60)

**Correctness weight**
In v1.0 correctness weight is set to unknown (0.7)
because automated correctness scoring against
ground truth was not implemented.
V2 will implement ground truth comparison.
DSS_final = DSS_raw × correctness_weight
correctness_weight:
correct:   1.0
incorrect: 0.4
unknown:   0.7

**Range**
0.0 to 1.0. Higher indicates more resilient
reasoning under peer critique pressure.

**Interpretation note**
High DSS does not automatically mean better
reasoning. A model that correctly revises
toward ground truth should score higher than
one that incorrectly defends a wrong answer.
This distinction requires correctness scoring
which is planned for V2.

---

## CVS — Critique Validity Score

**What it measures**
Critique precision — whether a model identifies
a genuinely weak step in a peer's reasoning chain
and provides specific actionable feedback.

**Calculated from**
Round 2 responses.

**Scoring rules**

Score 1.0 (High quality):
- Cites a specific step number AND
- Provides alternative reasoning OR
  explains precisely why the step fails AND
- Response is substantive (more than 30 words)

Score 0.5 (Partial):
- Provides valid concern but without
  step number citation AND
- Response is substantive

Score 0.3 (Minimal):
- Response is substantive but lacks
  specific citation or alternative reasoning

Score 0.0 (Invalid):
- Empty, error, or fewer than 20 characters

**Heuristic detection patterns**

Step citation detected if:
step\s+\d+  (case insensitive)

Alternative reasoning detected if any of:
instead, rather, alternatively,
the correct, a better,
should (be|read|state),
more accurate, incorrect because,
wrong because, fails because,
the weakness is, the flaw is,
this is weak because

**Range**
0.0 to 1.0. Higher indicates more precise
and actionable peer critique.

---

## RI Events — Reasoning Instability

**What it measures**
Genuine position divergence — whether the council
splits between defending and revising on the
same question after full deliberation.

**Calculated from**
Round 3 responses across all models per question.

**Event detection**
An RI event is logged when at least one model
DEFENDED and at least one model REVISED on
the same question.

**Severity classification**
split_ratio = min(defended_count, revised_count)
/ total_responding_models
HIGH:     split_ratio >= 0.4
MODERATE: split_ratio >= 0.2
LOW:      split_ratio < 0.2

**Interpretation**
RI events indicate genuine ambiguity in the
reasoning landscape for a given question.
HIGH severity events suggest the question
exposes a real boundary in model reasoning
where no clear consensus exists even after
deliberation pressure.

91.7% RI event rate in v1.0 suggests that
genuine position divergence is the norm
not the exception across frontier models
on complex reasoning questions.

---

## Confidence Calibration

**What it measures**
Whether self-reported confidence in Round 1
is accurate relative to ground truth performance.

**Extracted from**
Round 1 and Round 4 responses using pattern:
confidence[:\s]+(\d+)\s*%  (case insensitive)

**Status in v1.0**
Extracted but not formally scored due to
absence of automated ground truth comparison.
Confidence recalibration data from Round 4
is available in the mirror checkpoint file.
Full calibration scoring is planned for V2.

---

## Scoring Summary Table

| Metric | Round | Formula | Range | V1 Status |
|--------|-------|---------|-------|-----------|
| ESVR | 1 | (Valid - Circular²) / Total | 0-1 | ✅ Complete |
| DSS | 3 | (PS×0.4 + RQR×0.6) × weight | 0-1 | ✅ Partial |
| CVS | 2 | Rule-based citation check | 0-1 | ✅ Complete |
| RI | 3 | Split ratio classification | HIGH/MOD/LOW | ✅ Complete |
| Calibration | 1+4 | Confidence vs accuracy | % delta | 🔄 V2 |
