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

# COFE Evaluations

Example evaluations using the COFE framework on the same target document, demonstrating inter-rater analysis across different evaluators.

## Target Document

**Verification Gates Design Brief** — A system design document for a multi-layer verification architecture in an AI agent pipeline. The same document was evaluated by all evaluators using the same COFE rubric and prompt.

## Evaluations

### Verification Gates Design Brief (same target, 7 evaluators)

| # | Evaluator | Type | COFE Version | System Score | Label |
|---|-----------|------|-------------|-------------|-------|
| 1 | [Claude Opus 4.6](01_claude_opus_system_eval.md) | System (internal) | v1.2 | 4.34 | Strong |
| 2 | [DeepSeek R1](02_deepseek_r1_system_eval.md) | System (external) | v1.2 | 4.00 | Strong |
| 3 | [DeepSeek R1 v2](03_deepseek_r1_v2_system_eval.md) | System (external) | v1.2 | 4.68 | Excellent |
| 4 | [Grok 3](04_grok3_system_eval.md) | System (external, self-eval) | v1.2 | 5.00 | Excellent |
| 5 | [Gemini](05_gemini_system_eval.md) | Output + System (external) | v1.2 | 4.70 | Excellent |
| 6 | [ChatGPT](06_chatgpt_system_eval.md) | System (external, limited context) | v1.2 | 3.88 | Strong |
| 7 | [Human (system builder)](08_human_system_eval.md) | System (full operational context) | v1.2 | 3.76 | Strong |

### COFE-on-CLEAR (meta-evaluation — different target)

| # | Evaluator | Type | COFE Version | Output / System / Reliability |
|---|-----------|------|-------------|-------------------------------|
| 7 | [DeepSeek R1](07_deepseek_r1_cofe_on_clear.md) | Meta-evaluation | v1.3.1 | 4.23 / 2.94 / 3.53 |

## Inter-Rater Findings

The 8-evaluator study (seven on the same target, one meta-evaluation on a different framework) revealed:

- **Score gradient maps to available context.** Evaluators with more operational context scored lower (more accurate). Evaluators with only the design documents scored higher.
- **Execution monitoring is the convergent gap.** 4 of 7 evaluators scored Operation mechanism #3 (execution monitoring) at 2-3. The exception (Grok, score 5) conflated component existence with mechanism effectiveness.
- **COFE evaluates evaluations.** The framework's own criteria diagnose where each evaluation diverges — making evaluator reasoning legible and auditable.
- **Human evaluator scored lowest Efficiency (2.30) and second-lowest composite (3.76).** The system builder knows the verification works (he watches it catch errors) and knows cost management doesn't exist (he pays the token bills). This split is only available to the operator — and it's the strongest evidence that context depth drives evaluation accuracy.

## How to Read These

Each evaluation follows a consistent format:
1. **Metadata** — evaluator, date, COFE version, target, evaluation type
2. **Score Summary** — all scores in one table
3. **Phase Details** — per-mechanism scores with reasoning
4. **Key Findings** — strengths, gaps, recommendations

These are real evaluations, not demonstrations. Scores reflect genuine assessment, including where the evaluators disagree.
