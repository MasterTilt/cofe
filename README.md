---
type: note
status: active
domain: [meridian]
decimal: [d.1.4]
tags: [project/meridian, voice/meri]
keywords: [readme]
session:
created: 2026-04-09
---

# COFE: Comprehension-First Evaluation Framework

*Pronounced "coffee."*

An extensible framework for evaluating any LLM process, prompt, or system.

## What is COFE?

COFE evaluates two things most frameworks conflate: **what an LLM system produced** (output evaluation) and **whether the system can produce it reliably** (system evaluation). It produces three readings: an output score, a system score, and a reliability score that bounds trust by the weaker dimension.

Most evaluation frameworks measure output quality. 
COFE asks first: **did the system understand the task before executing?**

## Why does COFE exist?

Current AI agent evaluation frameworks share three structural gaps:

1. **No pre-execution comprehension evaluation.** No surveyed framework evaluates whether the system understood the task before beginning work. The mechanism is proven -- G-Eval (Liu et al. 2023) demonstrates that structured reasoning before scoring improves evaluation quality. Nobody applies it to the systems being evaluated.

2. **Output and system quality are conflated.** A lucky output from a broken system scores the same as a reliable output from a sound system. Most frameworks conflate product quality with factory quality, evaluating both in a single composite score.

3. **No prior-art comparison requirement.** No framework requires checking what already exists before crediting what is new. Three frontier models independently praised novelty claims that collapsed when a human introduced prior art.

COFE addresses all three gaps.

## How it works

**Output evaluation** (per-operation): Three phases scored via geometric mean.

- **Comprehension** (weighted 0.40) -- How well does the system understand the task?
- **Operation** (weighted 0.30) -- How well was the plan executed?
- **Fidelity** (weighted 0.30) -- How correct and usable is the output?

**System evaluation** (periodic): 22 mechanisms across four phases, scored on effectiveness.

- Comprehension mechanisms (5) -- Can the system reliably understand tasks?
- Operation mechanisms (6) -- Can the system reliably execute and coordinate?
- Fidelity mechanisms (4) -- Can the system reliably verify correctness?
- Efficiency mechanisms (7) -- Can the system reliably manage its resources?

**Reliability Score**: Asymmetric weighted geometric mean of output (0.40) and system (0.60) composites. System weighted higher because reliability is forward-looking -- future performance depends more on the factory than the last product. Trust is bounded by the weaker dimension.

## Key design decisions

- **Geometric mean at every level.** A single critical failure cannot be masked by high scores elsewhere.
- **1/3/5 anchor calibration.** 1 = active failure, 3 = adequate but passive, 5 = strong and intentional.
- **Efficiency as separate cost layer.** Not weighted into the composite -- cost is contextual.
- **N/A handling with mandatory flags.** Systems declare scope upfront. Mandatory mechanisms cannot be gamed via N/A.

## Validation

COFE has been validated through:

- **Internal application** across 370+ sessions in a collaborative human-AI system, including honest self-scoring showing where the system fails.
- **External evaluation** of an independent AI agent system (anonymized), scoring 1.00/1.00/1.00 (Non-functional) -- revealing a 47-component architecture that does not deliver its stated capability.
- **7-evaluator inter-rater study** (S369-370): Same prompt, same documents, seven evaluators (four frontier models, two human, one self-eval). Scores ranged 2.94-5.0, revealing a monotonic gradient mapping to available context depth. COFE's own criteria diagnose where each evaluation diverges.
- **Cross-model evidence**: Three frontier models independently failed prior-art comparison on the same evaluation task, reversing their assessments only after human intervention. The failure is architectural, not model-specific.

## The framework

See [COFE_Rubric.md](COFE_Rubric.md) for the complete specification (v1.3.1).

## Status

COFE is under active development. A research paper is in preparation.

- v1.0 (April 5, 2026) -- Initial framework design
- v1.1 (April 7, 2026) -- External critique pass, absence semantics, reliability score
- v1.2.1 (April 7, 2026) -- Full review: anchor recalibration, geometric mean within phases, five new mechanisms, efficiency scale, N/A rules
- v1.3 (April 8, 2026) -- Prior knowledge and scope anchors sharpened for external context
- v1.3.1 (April 8, 2026) -- Reliability Score changed to asymmetric weighting (Output^0.40 x System^0.60)

## License

MIT

## Citation

If you use COFE in your work, please cite:

```
Gonzales, B. (2026). COFE: Comprehension-Operation-Fidelity-Efficiency.
Comprehension-First Evaluation Framework. GitHub.
https://github.com/MasterTilt/cofe
```

## Contact

Balentin Gonzales -- [GitHub](https://github.com/MasterTilt)
