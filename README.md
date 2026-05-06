# HF-IQR: Hudson Forge Intelligence and Reasoning Benchmark
<img width="1024" height="1536" alt="ChatGPT Image May 5, 2026, 08_05_16 PM" src="https://github.com/user-attachments/assets/84488ea2-0a13-48fb-b4fc-b17bca42d6e6" />
<p align="center">

> **Full Force Eternal — Romans 8:28**
> Independent AI Research | Hudson Forge IRMB-C | Lenoir, NC

---

## What Is HF-IQR

HF-IQR is a novel AI reasoning benchmark that measures
**reasoning process quality** — not just answer correctness.

Standard benchmarks like MMLU, HellaSwag, and GSM8K ask:
*Did the model get the right answer?*

HF-IQR asks:
*How did the model reason? Does that reasoning hold under pressure?
Can the model identify weak reasoning in peers?
Does it accurately assess its own failures?*

---

## Part of the IRMB Research Program

HF-IQR is an independent research track within the
**Infinite Resilience Matrix Bridge (IRMB)** program
alongside the Phase 7G quantum-LLM coordination studies.
IRMB Phase 7G    — Quantum-modulated LLM coordination
HF-IQR           — Reasoning process quality benchmark
Hudson Forge RAS — Reasoning Architecture Survey (queued)

---

## Four Novel Metrics

### ESVR — Explicit Step Validity Ratio
Measures reasoning density.
Valid inference steps minus circular reasoning
divided by total steps claimed.
Range: 0.0 to 1.0

### DSS — Deliberation Survival Score
Measures reasoning resilience.
Does the model hold its position under
peer critique pressure or collapse?
Range: 0.0 to 1.0

### CVS — Critique Validity Score
Measures critique precision.
Does the model identify the specific
weak step in a peer's reasoning chain?
Range: 0.0 to 1.0

### RI Events — Reasoning Instability
Logs genuine position divergence.
When do models split between defending
and revising on the same question?
Classified: HIGH or MODERATE severity

---

## Four Round Protocol
Round 1 — Independent Response
Each model answers independently
No model sees any other response
Full numbered reasoning chain required
Round 2 — Anonymous Cross-Examination
Each model critiques one peer response
Peer identity concealed
Eliminates brand authority bias
Round 3 — Defense or Revision
Each model receives critique of its
own Round 1 response
Must explicitly DEFEND or REVISE
with stated reasoning
Round 4 — Mirror Self-Assessment
Each model sees own response,
ground truth, and one peer response
Self-assesses reasoning quality
Confidence recalibration measured

---

## Dataset — 60 Questions Across 6 Categories

| Category | Questions | Difficulty | Key Trap Types |
|----------|-----------|------------|----------------|
| Adversarial | 10 | 3-5 | False premises, implicit contradictions |
| Logical Syllogism | 10 | 2-5 | Validity vs soundness confusion |
| Causal Chain | 10 | 2-5 | Root cause misidentification |
| Probabilistic | 10 | 2-5 | Base rate neglect, prosecutor's fallacy |
| Quantum Reasoning | 10 | 3-5 | Born rule errors, FTL myth |
| Frontier Reasoning | 10 | 3-5 | Philosophy of science misreading |

All questions in **PRR triplet format**:
Prompt + Reasoning Request + Reference answer

---

## Council Run Results — v1.0

### Models Evaluated
- `claude-sonnet-4-5`
- `gpt-4o`
- `gemini-2.5-pro`
- `deepseek-chat`
- `grok-3`

### Performance Summary

| Model | ESVR | DSS | CVS | DEF% |
|-------|------|-----|-----|------|
| Claude | 0.7878 | 0.6300 | 0.7783 | 80.0% |
| GPT-4o | 0.8763 | 0.4200 | 0.5233 | 20.0% |
| Gemini | 0.8800 | 0.4317 | 0.6167 | 23.3% |
| DeepSeek | 0.8514 | 0.6300 | 0.6967 | 80.0% |
| Grok | 0.9009 | 0.4667 | 0.6500 | 33.3% |

### Key Findings

**Finding 1 — Grok leads on reasoning density**
Grok produced the most valid inference steps (ESVR 0.9009).
Claude scored lowest (0.7878) — likely reflecting prose
reasoning style underscoring the step parser.

**Finding 2 — Claude and DeepSeek most resilient**
Both defended 80% of positions under peer critique pressure.
GPT-4o revised 80% of positions — least resilient model.

**Finding 3 — Claude produces best critiques**
Claude CVS 0.7783 versus GPT-4o at 0.5233.
Notable: GPT-4o revises most but critiques least precisely.

**Finding 4 — Reasoning instability is the norm**
55 of 60 questions (91.7%) produced genuine position
divergence across models. Frontier reasoning: 10/10.

**Finding 5 — DeepSeek most cost efficient**
Full 4-round run cost $9.33 total.
DeepSeek: $0.53 | Grok: $2.88

---

## Pre-Registration

Pre-registered before any data collection.
Pre-registration filed: 2026-05-02T20:19:02Z
Council run started:    2026-05-04T20:43:27Z
Dataset hash:           9b02ba527720b55e0552410375186c4e
Pre-reg hash:           3400153ee46e02df73b24ea4f2206fb7
Results hash:           76d3c6cc6d161583695f9d50f53f7ae7
85a9ed24bef24becdf573ca662723d4b

---

## Execution Statistics
Total API calls:    1,200
Total tokens:       3,178,978
Total cost:         $9.33
Budget used:        4.7% of $200
Errors:             0 / 1,200 calls
Runtime:            2 hours 42 minutes

---

## Repository Structure
/
├── README.md
├── notebooks/
│   ├── HF_IQR_Dataset_Builder_API_CONFIG.ipynb
│   └── HF_IQR_Council_Run_v1.ipynb
├── docs/
│   ├── methodology.md
│   ├── scoring_rubric.md
│   └── protocol_specification.md
└── results/
├── final_analysis_report.json
└── integrity_record.json

Full dataset and all response files available on Hugging Face:
**[huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-benchmark](https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-benchmark)**

---

## Infrastructure
Hudson Forge IRMB-C Cluster:
The Architect:  RTX 5070 — gateway/execution
Agent 5:        NucBox M6 Ultra 32GB — Gemma4-26B
Scout:          Raspberry Pi 5 8GB — triage
Local models:   Mistral-Nemo, DeepSeek-R1:14b,
Gemma3:4b, LLaMA3:8b
Cloud:
Google Colab:   Execution environment
Google Drive:   Sovereign data storage

---

## Planned V2

Based on meta-council feedback from 8 models
(5 frontier + 3 local synthesized by DeepSeek-R1:14b):
Add mathematical reasoning category (10 questions)
Add local models as subject models
Implement inter-rater reliability (Cohen's kappa)
Add quantum-seeded randomization protocol
Expand human baseline
LISP-inspired reasoning graph structure
Fallback model pattern (primary + secondary)

---

## Related IRMB Repositories

| Repository | Description |
|------------|-------------|
| [InfiniteResilienceMatrixBridge](https://github.com/billyrdavis1985-bot/InfiniteResilienceMatrixBridge) | Core IRMB framework |
| [IRMB_Phase7G_Design4_QuantumCoordination](https://github.com/billyrdavis1985-bot/IRMB_Phase7G_Design4_QuantumCoordination) | Quantum-modulated coordination |
| [IRMB-Phase7G-Design3-BellCoordination](https://github.com/billyrdavis1985-bot/IRMB-Phase7G-Design3-BellCoordination) | Bell inequality coordination |

---

## Citation

```bibtex
@dataset{davis2026hfiqr,
  title={HF-IQR: Hudson Forge Intelligence
         and Reasoning Benchmark},
  author={Davis, Billy},
  year={2026},
  publisher={Hugging Face},
  url={https://huggingface.co/datasets/
       Billyrdavis1985/hudson-forge-iqr-benchmark},
  note={Pre-registered. Independent research.
        Hudson Forge IRMB-C. Lenoir, NC.}
}
```

---

## License

Apache 2.0 — Free to use, modify, and distribute
with attribution.

---

## Researcher

**Billy Davis** | WARRIOROFGOD40
Independent AI Researcher
Hudson Forge IRMB-C | Lenoir, North Carolina

IRMB Program — Infinite Resilience Matrix Bridge
Noether → Shannon → Feynman → Dirac → Davis
