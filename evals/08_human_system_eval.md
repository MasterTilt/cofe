---
type: note
status: active
domain: [meridian]
decimal: [d.1.4]
tags: [project/meridian, voice/meri]
keywords: [human, eval, tilt, system]
session:
created: 2026-04-09
---

# COFE System Evaluation — Verification Gates Design

| Field | Value |
|-------|-------|
| **Evaluator** | Balentin Gonzales (human, system builder and operator) |
| **Date** | April 9, 2026 (reconstructed from S369 scores, April 8, 2026) |
| **COFE Version** | v1.2 |
| **Target** | Verification Gates Design Brief (dated version) |
| **Evaluation Type** | System evaluation — design review |
| **Context Level** | Full — evaluator designed, built, and operates the system being evaluated |

---

## Score Summary

| Dimension | Score | Label |
|-----------|-------|-------|
| Comprehension | 3.90 | Strong |
| Operation | 3.40 | Developing |
| Fidelity | 4.47 | Strong |
| **System Composite** | **3.76** | **Strong** |
| Efficiency (cost layer) | 2.30 | Weak |

---

## Comprehension Mechanisms

| # | Mechanism | Score | Reasoning |
|---|-----------|-------|-----------|
| 1 | System legibility (M) | 3 | Documented, but not necessarily well-structured or easy to follow. Readable by an LLM but a human may not find it flows well. Structure could benefit both human and LLM evaluators. |
| 2 | Pre-work comprehension check (M) | 5 | Excellent and thorough implementation with verification. |
| 3 | Scope definition process (M) | 5 | Well defined, with verification. |
| 4 | Prior knowledge mapping (M) | 3 | Exists, but is a judgment call, not mechanical. Should use specific tools to gather sources and output a verifiable list. |
| 5 | Gap identification | 4 | Good but needs refinement. Somewhat ambiguous — related to the prior knowledge mapping gap. |

**Phase Score: 3.90** (geometric mean)

---

## Operation Mechanisms

| # | Mechanism | Score | Reasoning |
|---|-----------|-------|-----------|
| 1 | Planning / approach structure (M) | 4 | Detailed instructions, failure modes, examples. Needs verification of plan quality. |
| 2 | Scope enforcement during execution (M) | 3 | Checks exist, but are post-hoc and external. |
| 3 | Execution monitoring (M) | 2 | Verifies but post-hoc. Doesn't ensure execution is correct during the work. |
| 4 | Inter-agent information transfer (M) | 5 | Well-defined agents and handshakes with validation. |
| 5 | Role specialization producing value | 4 | Role distinction is implemented, clear, and purposeful. Convergent validation present but impactful uniques not always quantified. |
| 6 | Delivery tracking (M) | 3 | Tracking is sufficient for basic and terminal deliverables. Needs depth. |

**Phase Score: 3.40** (geometric mean)

---

## Fidelity Mechanisms

| # | Mechanism | Score | Reasoning |
|---|-----------|-------|-----------|
| 1 | Output verification (M) | 4 | Multiple layers of checks, internal to external. However, this version lacks internal tracking — the system cannot prove verification ran. Effective on most cases but missing mechanical guarantee. |
| 2 | Factual accuracy checking (M) | 5 | Factual checks are thorough and vetted through independent source. |
| 3 | Reasoning chain validation (M) | 4 | Reasoning check by external layer. Process can be better defined and more nuanced. |
| 4 | Cross-agent consistency | 5 | Disagreements are flagged with confidence and presented for review or reinitiate procedure. |

**Phase Score: 4.47** (geometric mean)

---

## Efficiency Mechanisms

| # | Mechanism | Score | Reasoning |
|---|-----------|-------|-----------|
| 1 | Resource fitness | 1 | No way of tracking overhead other than token input count. |
| 2 | Adaptive scaling | 1 | No tiers or options for simple tasks. |
| 3 | Waste prevention | 4 | Early gates prevent waste downstream, but no scaling processes — produces waste for less complex tasks. |
| 4 | Instruction economy | 4 | Instructions are clear and concise. However, can be more efficient. |
| 5 | Verification targeting | 4 | Checks prioritize load-bearing claims early and at terminal output. |
| 6 | Completion rate | N/A | Rolling metric — not available at design level. |
| 7 | Human intervention rate | N/A | Rolling metric — not available at design level. |

**Efficiency Score: 2.30** (geometric mean — resource fitness and adaptive scaling at 1 pull sharply)

---

## Key Findings

**This is the only evaluation from someone who built and operates the system.** The scores reflect operational knowledge unavailable to any model evaluator.

**Fidelity at 4.47 (Strong) — scored against the deployed version.** Output verification dropped from 5 to 4 because this version lacks internal tracking — the system cannot prove verification ran, only that the output looks verified. Model evaluators scored the design brief's description; the human scored what was actually running.

**Lowest Efficiency in the study (2.30).** Resource fitness and adaptive scaling scored 1 (absent-in-scope). The evaluator pays the token bills and knows there is no cost tracking system, no tiered complexity handling, no budget mechanism. Model evaluators scored these 2-5 by crediting targeted reads and conditional dispatch as "resource awareness" — the human knows those are waste prevention, not resource management.

**Execution monitoring convergent at 2.** Matches our internal evaluation (2-3) and DeepSeek's first pass (2). The convergent finding across the study: 4 of 7 evaluators scored this mechanism 2-3. The system verifies after the fact but cannot ensure correct execution during the work.

**Legibility at 3 — unique finding.** Every model evaluator scored legibility 4-5. The human scored 3 because the documents are LLM-parseable but not well-structured for human readers. This gap suggests that better document structure would improve evaluation accuracy for both humans and LLMs — LLM tolerance of messy structure masks a real legibility gap.

**Inter-rater position:** System composite 3.76 (Strong). In the gradient: Us 2.94 / Tilt 3.76 / ChatGPT 3.88 / DeepSeek 4.00 / Claude-web 4.34 / Gemini 4.70 / Grok 5.00. The human evaluator scores second-lowest — where the most informed evaluators cluster.

**Context improves accuracy.** The inter-rater gradient maps to available context. Evaluators with more operational evidence scored lower (more critically, more accurately). The information that makes evaluation harder — failure modes, edge cases, known gaps — is the same information that makes evaluation better. This finding supports COFE's design claim that evaluation requires a human with domain expertise.

---

*Evaluation produced using COFE v1.2 criteria. Evaluator is the system designer and operator with full operational context. Per-mechanism scores provided directly by the evaluator; composites computed via geometric mean per COFE specification. Originally scored during S369 (April 8, 2026); reconstructed and documented S371 (April 9, 2026).*
