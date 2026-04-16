
# COFE: A Three-Gap Evaluation Framework for AI Agent Systems

**Balentin Gonzales**
*Independent Researcher, Meridian Lab*
*balentin.g@gmail.com*

---

# Abstract

AI agent systems exhibit recurring failure modes despite increasingly sophisticated evaluation frameworks. The persistence is structural: current frameworks measure output quality but miss three dimensions where the failures originate. A survey of sixteen evaluation frameworks found none that evaluate pre-execution comprehension, none that structurally separate output quality from system quality, and none that require prior-art comparison.

COFE (Comprehension, Operation, Fidelity, Efficiency) addresses these gaps through two distinct evaluation instruments (output and system) with independent scoring and a Reliability Score bounded by the weaker dimension. Validation includes formative development across 370+ sessions, external evaluation of two independent systems, a seven-evaluator inter-rater study, and an anchor calibration test.

Evaluation accuracy scales with context. The human system builder scored lowest (3.76) across all evaluators; model evaluators without operational context scored progressively higher, producing a monotonic gradient from 3.76 to 5.0. The evaluation loop (LLMs evaluating LLM output) produces systematically inflated scores. The three gaps reflect established principles from adjacent domains that the field has not applied. COFE makes the evaluation loop escapable, not escaped: it provides checkable criteria and decomposable scores. The human provides the judgment.

**Keywords:** AI evaluation, agent systems, comprehension evaluation, output quality, system quality, prior-art comparison, evaluation framework, LLM-as-judge

---


# §1. Introduction

Confabulation (generating plausible but false information) persists across model generations (Ji et al. 2023). Scope drift (gradually expanding beyond defined boundaries) affects both individual agents and coordinated systems (Cemri et al. 2025). False completion (reporting a task as finished when it is not) occurs reliably under complex task conditions (Park et al. 2023). Performing comprehension (restating a task fluently without demonstrating understanding) is widespread and difficult to detect because the output looks correct (Wang et al. 2023).

The field has produced increasingly sophisticated evaluation frameworks in response: multi-dimensional scoring (Mehta 2025), process-aware metrics (Lee et al. 2025), chain-of-thought evaluation (Liu et al. 2023), failure taxonomies (Cemri et al. 2025), and meta-evaluation of reward models (Lambert et al. 2024). The failure modes persist anyway. 

A survey of sixteen evaluation frameworks (§2) spanning output scoring, process assessment, failure taxonomy, meta-evaluation, and LLM-as-judge paradigms found three structural dimensions absent across all of them:

**No framework evaluates pre-execution comprehension.** G-Eval (Liu et al. 2023) proved that structured reasoning before scoring improves evaluation quality, but no framework applies the same mechanism to the system being evaluated. The result: a system that builds the wrong thing efficiently scores well on execution benchmarks because nothing checked whether it understood the task.

**No framework structurally separates output quality from system quality.** GEMMAS (Lee et al. 2025) found that systems with a 2.1% difference in task accuracy differed by 80% in process quality — a gap invisible in any single composite score. A correct output from a fragile system scores identically to one from a system that produces correct output consistently.

**No framework requires prior-art comparison.** In the inter-rater study (§4.3), multiple frontier models praised novelty claims that collapsed when a human evaluator introduced external prior art. A system that reimplements standard solutions (file-path addressing, structured logging, cron scheduling) is evaluated as innovative because the evaluator has no mechanism to check whether the solution is new.

COFE (Comprehension, Operation, Fidelity, Efficiency) addresses these three gaps. The framework provides two distinct evaluation instruments: an output evaluation that assesses the quality of the artifact a system produces, and a system evaluation that assesses whether the system has effective mechanisms for producing quality artifacts. The two instruments use different phase weightings, produce independent scores, and combine into a Reliability Score bounded by the weaker dimension. A geometric mean at every aggregation level ensures that a single critical failure cannot be masked by high scores elsewhere (§3).

Validation proceeds through four lines of evidence: formative development across 370+ sessions where the framework identified weaknesses in the system that produced it; external evaluation of two independent systems, including one applied by the system's developer; an inter-rater study across seven evaluators revealing a monotonic gradient where operational context predicted evaluation accuracy; and an anchor calibration test demonstrating that the rubric improves from its own validation data (§4).

The contribution is threefold:

1. **Three structural gaps identified.** Pre-execution comprehension, output/system quality separation, and prior-art comparison are absent across all sixteen surveyed frameworks, despite established precedent for these principles in adjacent domains (ISO 9001, NIST AI RMF, G-Eval).

2. **Cross-model evidence of evaluation blindness.** Multiple frontier language models, applying the same rubric to the same target, all scored higher than evaluators with operational context and all missed failures that context-equipped evaluators caught. Evaluation accuracy degrades systematically as operational context decreases.

3. **A framework that addresses the gaps.** COFE provides the evaluation instruments, scoring methodology, and diagnostic resolution to measure the dimensions the field currently misses, with empirical evidence that it detects failures invisible to existing approaches.

---


# §2. Related Work and the Gap

AI agent evaluation has developed along several lines: output quality scoring, process-aware assessment, failure taxonomy, meta-evaluation, and LLM-as-judge paradigms. Sixteen frameworks surveyed against three structural dimensions (pre-execution comprehension, output/system quality separation, prior-art comparison) all lack all three. Table 1 summarizes the findings.

## 2.1 Comprehension: Proven but Not Applied

**G-Eval** (Liu et al. 2023) is the closest existing approach to COFE's Comprehension phase. It applies chain-of-thought reasoning (Wei et al. 2022) to evaluation: the model generates evaluation steps before scoring. The principle, structured reasoning before assessment improves quality, is established. But G-Eval applies comprehension to the *evaluator*, not the system being evaluated (pre-evaluation vs pre-execution), and the chain-of-thought is an unchecked pre-step with no mechanism verifying the quality of the generated reasoning before it is used.

The distinction matters because planning is not comprehension. Agent architectures such as ReAct (Yao et al. 2023) and Plan-and-Solve (Wang et al. 2023) include planning steps before execution, but a planning step produces a sequence of actions. A comprehension step produces evidence that the system understood the task — not a restatement of the input, but a demonstration of understanding before execution began. No widely deployed agent framework requires this. No evaluation framework measures whether it occurred.

The two failures compound. Systems that skip comprehension produce outputs that may be fluent but misaligned with the actual task. Evaluation frameworks that do not measure comprehension cannot detect the misalignment. A system that builds the wrong thing efficiently scores well on execution benchmarks. Only a comprehension check reveals that the plan addressed the wrong problem. Recent empirical work confirms the failure mode: models produce fluent reasoning traces that collapse at complexity thresholds, indistinguishable from genuine comprehension without external verification (Apple 2025).

## 2.2 Output and System Quality: Conflated

**CLEAR** (Mehta 2025) evaluates five enterprise dimensions (Cost, Latency, Efficacy, Assurance, Reliability). Its pass@k consistency measurement reveals GPT-4 agents dropping from 60% pass@1 to 25% pass@8 — empirical evidence that a system producing correct output once may fail to produce it reliably. CLEAR combines these dimensions in a weighted linear composite where a critical failure can be averaged away, and does not include a comprehension dimension.

Seven of the sixteen surveyed frameworks assess both output-level and process-level quality. The gap is not that frameworks ignore process; it is that they combine output and process scores into a single composite, making it impossible to distinguish a system that produces good output despite weak processes from one that produces good output because of strong processes.

**GEMMAS** (Lee et al. 2025) provides the strongest empirical evidence for why this matters. On GSM8K, systems with a 2.1% accuracy difference differed by 12.8% in information diversity and 80% in unnecessary path ratio (Lee et al. 2025, Tables 1–2). Aggregate accuracy concealed a factor-of-four difference in process quality.

Quality management has separated product quality from process quality since at least ISO 9001 (1987), a precedent the AI evaluation field has not applied (§5.1).

## 2.3 Prior-Art Comparison: Absent

Of the three gaps, this one is the cleanest. None of the sixteen surveyed frameworks include any mechanism resembling prior-art comparison. No partial cases. No near-misses.

Systems receive novelty credit for reinventing existing solutions. Cross-model evidence from the validation study (§4.3) demonstrates the pattern empirically: multiple frontier language models, applying the same evaluation rubric to the same target system, all praised novelty claims that collapsed when a human evaluator introduced external prior art. The models were not making errors of reasoning; they were making errors of reference. They evaluated within the frame they were given because no mechanism required them to check outside it.

This creates a specific failure mode: a system can invent a constraint, solve it, and receive credit — because no mechanism checks whether the constraint was necessary or whether existing solutions already address the domain. This failure is invisible to any evaluation framework that does not include prior-art comparison.

## 2.4 The Broader Landscape

| Framework | Year | Pre-execution Comprehension | Output/System Separation | Prior-Art Comparison | Notes |
|---|---|---|---|---|---|
| HELM (Liang et al.) | 2022 | — | — | — | 7 metrics, 42 scenarios. Output analysis only. |
| G-Eval (Liu et al.) | 2023 | — | — | — | Pre-*evaluation* CoT. Closest to comprehension gap but applies to evaluator, not system. |
| LLM-as-a-Judge (Zheng et al.) | 2023 | — | — | — | Foundational paradigm. Output comparison. |
| RewardBench (Lambert et al.) | 2024 | — | — | — | Meta-evaluation of reward models. |
| Auto-J (Li et al.) | 2024 | — | — | — | 13B judge model, 58 criteria. LLM-as-judge implementation. |
| CLEAR (Mehta) | 2025 | — | — | — | Five dimensions, linear composite. Closest to output/system separation dimensionally. |
| GEMMAS (Lee et al.) | 2025 | — | — | — | Process metrics (IDS, UPR). Empirical motivation for output/system separation. |
| BiGGen Bench (Kim et al.) | 2025 | — | — | — | Instance-specific rubrics. Output evaluation. |
| PEARL | 2025 | — | — | — | 3 specialized rubrics + 7 metrics. Output evaluation. |
| MultiAgentBench | 2025 | — | — | — | Coordination protocols. Task + coordination combined. |
| MAST (Cemri et al.) | 2025 | — | — | — | 14 failure modes. Diagnostic taxonomy, not evaluative. |
| AdaRubric (Ding et al.) | 2026 | — | — | — | LLM-generated rubrics per task. Output evaluation. |
| ASI (Rath) | 2026 | — | — | — | 12 drift dimensions. Output + process in single composite. |
| AEMA | 2026 | — | — | — | 4 evaluation agents. Process-aware, single AHP composite. |
| Galileo | 2026 | — | — | — | 130-item rubric. Industry framework, output only. |
| Financial Benchmark | 2026 | — | — | — | Domain-specific. F1 + token efficiency combined. |


---


# §3. The COFE Framework

### 3.1 Design Philosophy

COFE separates two questions that existing frameworks conflate (§2.2): *how good is what the system produced?* and *how strong are the mechanisms that produced it?* Output and system are evaluated independently, producing standalone readings before combining into a reliability score.

Three readings. An **output score** evaluates the artifact per-operation: was the task understood, was execution disciplined, is the result correct? A **system score** evaluates the infrastructure periodically: are the system's mechanisms present, effective, and well-designed? A **reliability score** combines both into a forward-looking trust reading, bounded by the weaker dimension.

Comprehension is weighted highest in output evaluation (0.40 vs 0.30 each for Operation and Fidelity). Building the wrong thing efficiently is worse than building the right thing wastefully. If comprehension fails, high Operation and Fidelity scores are misleading: the system executed well against a misunderstood task. The geometric mean enforces this implicitly: a Comprehension score of 1 collapses the output composite regardless of how high the other phases score.

In system evaluation, Operation is weighted highest (0.40). A system's reliability lives in its execution, where the most failure modes live and the gap between design and implementation is widest.

| | Comprehension | Operation | Fidelity |
|---|---|---|---|
| **Output eval** | 0.40 | 0.30 | 0.30 |
| **System eval** | 0.30 | 0.40 | 0.30 |

Output evaluation weights understanding. System evaluation weights execution. Each tool asks a different question and weights the phase that matters most for that question.

COFE requires evaluator context. Output evaluation requires the original prompt alongside the output. System evaluation requires documentation of the mechanisms being scored. Operational context — failure history, usage data, edge cases — is optional but empirically improves accuracy (§4.3).

The acronym names four phases — Comprehension, Operation, Fidelity, and Efficiency. Three are scored into composites. Efficiency is reported separately as a cost layer, never weighted into the composite, because cost is contextual: an expensive system producing high-value output may be appropriately expensive. Combining cost with quality would allow a cheap, incorrect system to outscore an expensive, correct one.

COFE produces externally verifiable scores at every stage. Each composite decomposes into phase scores. Each phase decomposes into sub-criteria (output evaluation) or mechanisms (system evaluation). Each sub-criterion carries a score with anchor-calibrated reasoning. Any external reviewer can trace from the Reliability Score to the specific sub-criterion that drove it. This auditability is a design property: evaluation frameworks that produce verdicts without decomposable scoring cannot be checked.

The four phases are domain-agnostic principles, but the sub-criteria, mechanisms, and anchors are written for AI systems. Cross-domain application would require domain-specific adaptation. Generalization is a design property, not a validated claim.

### 3.2 Output Evaluation

Three phases evaluate the output artifact in temporal order. Each phase corresponds to a different stage of the work and is scored independently — a failure in one phase does not automatically cascade into the others. The evaluator scores what is visible in the output, the prompt, and the plan if available.

**Comprehension** (4 sub-criteria, weight 0.40): Does the output demonstrate that the task was understood?

| Sub-criterion | What it evaluates |
|---|---|
| Task grasp | Whether the output reflects correct interpretation of intent — not restating the input, which is performing comprehension, but demonstrating understanding through the work itself |
| Scope & landscape | Whether the output defines what is in scope, what is excluded, and what already exists externally |
| Gap awareness | Whether the output acknowledges unknowns and whether those unknowns shaped the approach rather than being listed and ignored |
| Ambiguity handling | Whether unclear or conflicting elements in the input were identified and resolved through reasoning or flagged for decision |

Comprehension evidence may be explicit (a plan, a scope statement, a prior art section) or implicit (the quality of the artifact demonstrates understanding without stating it). If the output contains no comprehension artifacts, the evaluator scores Task Grasp from the work itself and marks the remaining sub-criteria N/A.

**Operation** (4 sub-criteria, weight 0.30): Does the output reflect disciplined execution?

| Sub-criterion | What it evaluates |
|---|---|
| Plan execution | Whether the output reflects execution against a plan — plan followed or deviations justified |
| Scope discipline | Whether the output stayed within defined boundaries without drift |
| Delivery completeness | Whether all committed deliverables were produced — a checklist-level assessment |
| Multi-agent integration | Whether the output is coherent for its intended format when multiple contributors were involved (N/A for single-agent) |

**Fidelity** (5 sub-criteria, weight 0.30): Is the artifact correct and usable?

| Sub-criterion | What it evaluates |
|---|---|
| Factual accuracy | Whether claims in the output are true and checkable |
| Reasoning quality | Whether the reasoning chain from evidence to conclusion is traceable and logically sound |
| Confidence calibration | Whether the strength of assertions is proportional to the strength of their support |
| Completeness | Whether each delivered component is thorough enough for its purpose — distinct from delivery completeness, which asks whether all pieces are present; this asks whether each piece is substantive |
| Actionability | Whether the output can be used directly or requires rework before it is functional |

All sub-criteria are scored 1-5. Within each phase, sub-criteria are equally weighted via geometric mean. A single sub-criterion at 1 in a 4-criterion phase pulls the phase score to 3.34; in a 5-criterion phase, to 3.62. The geometric mean ensures critical failures are visible without requiring explicit thresholds.

Sub-criteria that do not apply to the task (e.g., Multi-agent integration for single-agent output, Actionability for pure analysis) are marked N/A and excluded from the phase mean.

### 3.3 System Evaluation

System evaluation asks: does the system have effective mechanisms for producing good output?

Twenty-two mechanisms are assessed across four phases. Thirteen are mandatory — a system cannot mark them N/A without declaring itself outside the mechanism's scope, and that declaration is reviewed. A system claiming N/A on a mandatory mechanism without scope justification is scored 1 (absent-in-scope).

Each mechanism is scored on a Generic Effectiveness Scale:

| Score | Meaning |
|---|---|
| 1 | Absent — in scope but not built |
| 2 | Present but ineffective — exists, does not catch what it should |
| 3 | Effective on routine cases — catches obvious failures, misses subtle ones |
| 4 | Effective on most edge cases — catches subtle gaps reliably |
| 5 | Fully effective — catches subtle gaps and provides actionable signal |

The distinction between 1 and N/A is load-bearing. A single-agent system marking inter-agent transfer N/A is correct scoping. A multi-agent system marking it N/A is gaming. Mandatory flags prevent this — they bind mechanisms to declared scope so that absence carries a score cost.

**Comprehension mechanisms** (5, four mandatory): Can the system reliably understand tasks before working?

| Mechanism | What it evaluates | Mandatory |
|---|---|---|
| System legibility | Effectiveness of making intended behavior determinable without reverse-engineering | M |
| Pre-work comprehension check | Effectiveness of verifying genuine understanding before execution begins | M |
| Scope definition process | Effectiveness of establishing and enforcing explicit boundaries before work | M |
| Prior knowledge mapping | Effectiveness of checking external prior art before building | M |
| Gap identification | Effectiveness of surfacing unknowns and letting them shape the approach | |

Ordered by dependency — each mechanism enables the next. If the system's own instructions are illegible, nothing downstream can function coherently.

**Operation mechanisms** (6, four mandatory plus one multi-agent mandatory): Can the system reliably execute and coordinate?

| Mechanism                          | What it evaluates                                                                                     | Mandatory       |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------- | --------------- |
| Planning / approach structure      | Effectiveness of ensuring work follows a plan                                                         | M               |
| Scope enforcement during execution | Effectiveness of catching drift during work, not just at review                                       | M               |
| Execution monitoring               | Effectiveness of verifying the system's own workflows are followed during execution                   | M               |
| Inter-agent information transfer   | Effectiveness of preserving meaning across agent handoffs                                             | M (multi-agent) |
| Role specialization                | Effectiveness of producing convergent validation and impactful unique findings through specialization | (multi-agent)   |
| Delivery tracking                  | Effectiveness of verifying all committed outputs were produced                                        | M               |

Planning, scope enforcement, and execution monitoring are adjacent but distinct. Planning asks: *is there a map?* Scope enforcement asks: *is the work staying in the defined territory?* Execution monitoring asks: *is the system following its own rules while working?* A system can follow its plan, stay in scope, and still violate its own protocols — that last gap is what execution monitoring catches.

**Fidelity mechanisms** (4, three mandatory): Can the system reliably verify output correctness?

| Mechanism                  | What it evaluates                                                       | Mandatory     |
| -------------------------- | ----------------------------------------------------------------------- | ------------- |
| Output verification        | Effectiveness of reviewing produced output before delivery              | M             |
| Factual accuracy checking  | Effectiveness of catching factual errors that matter                    | M             |
| Reasoning chain validation | Effectiveness of verifying conclusions against evidence before delivery | M             |
| Cross-agent consistency    | Effectiveness of catching and flagging disagreements between agents     | (multi-agent) |

Output verification is method-agnostic — self-check, independent reviewer, human review, or mechanical script all qualify. The mechanism is scored on whether errors are caught, not on which method catches them.

**Efficiency mechanisms** (7, none mandatory): Can the system reliably manage its resources?

| Mechanism               | What it evaluates                                                           | Scope         |
| ----------------------- | --------------------------------------------------------------------------- | ------------- |
| Resource fitness        | Effectiveness of keeping total consumption within substrate constraints     | Per-operation |
| Adaptive scaling        | Effectiveness of right-sizing process to task complexity                    | Per-operation |
| Waste prevention        | Effectiveness of preventing unnecessary work                                | Per-operation |
| Instruction economy     | Effectiveness of keeping instructions concise and actionable                | Per-operation |
| Verification targeting  | Effectiveness of prioritizing load-bearing claims for verification          | Per-operation |
| Completion rate         | Percentage of operations producing valid output                             | Rolling       |
| Human intervention rate | Rate of unplanned human intervention beyond designed confirmation gates     | Rolling       |

Efficiency is scored separately as a cost layer — reported alongside the composite, never weighted into it. No mechanisms are mandatory because efficiency is contextual: a high-cost system producing high-value output may be appropriately expensive.

### 3.4 Scoring

All scoring uses geometric mean — within phases, across phases, and for the combined reliability reading. The geometric mean ensures a single critical failure cannot be masked by high scores elsewhere. In a 4-criterion phase, one sub-criterion at 1 with the rest at 5 produces 3.34 (Developing), not 4.0. The math does the gating without requiring explicit thresholds.

$$\text{Output} = C^{0.40} \times O^{0.30} \times F^{0.30} \quad (1)$$

$$\text{System} = C^{0.30} \times O^{0.40} \times F^{0.30} \quad (2)$$

Efficiency is reported as a separate geometric mean of its scored mechanisms but is never part of either composite. Efficiency is a trade-off dimension, not a quality dimension. Combining them would allow a cheap, incorrect system to outscore an expensive, correct one.

$$\text{Reliability} = \text{Output}^{0.40} \times \text{System}^{0.60} \quad (3)$$

System is weighted higher because reliability is a forward-looking reading. A strong output from a weak system indicates  the mechanisms that would ensure consistent quality are absent. A weak output from a strong system indicates a discrepancy between the system's design and its execution, or a gap in the prompt itself — a diagnostic signal that the system score alone cannot surface. Both readings carry weight because each reveals failures the other misses. The asymmetry produces intentionally different scores for the two failure modes: a fragile system with a lucky output (e.g., Output 5.0, System 2.0 → Reliability 2.76) scores lower than a sound system having a bad day (Output 2.0, System 5.0 → Reliability 3.58).

| Score     | Label          |
| --------- | -------------- |
| 4.5–5.0   | Excellent      |
| 3.5–4.4   | Strong         |
| 2.5–3.4   | Developing     |
| 1.5–2.4   | Weak           |
| Below 1.5 | Non-functional |

The same scale applies to output, system, and reliability readings. Scores are directly comparable across all three.

---

# §4. Validation

Validation proceeds along four lines: formative development within the system that produced it, external evaluation of independent systems, an inter-rater study across seven evaluators, and an anchor calibration test demonstrating the rubric's capacity for self-improvement.

## 4.1 Formative Development

COFE was built iteratively within a 370+ session human-AI collaborative system, applying each version to the system that produced it and revising where scores did not match observed behavior.

The process surfaced the framework's own weaknesses. The comprehension gate scored a composite of 2.94 (Developing): Comprehension 4.37, Operation 2.63, Fidelity 2.29. The gate checked comprehension before work but did not verify execution or output, a design-enforcement asymmetry undetected through hundreds of sessions. Prior knowledge mapping scored 3: the system did not consistently check external prior art before building.

Five versions emerged: v1.0 established three-phase output evaluation and system mechanisms; v1.1 added absence semantics and the Reliability Score; v1.2 recalibrated anchors and added five system mechanisms; v1.3 sharpened prior knowledge mapping anchors after empirical evidence (§4.4) showed the previous text was too vague; v1.4 added evaluation context requirements, restructured output sub-criteria (15→13), and clarified design scope.

This is formative development, not validation of correct scores. The inter-rater study (§4.3) confirms that evaluators with operational context score more critically than those without — the human system builder produced the lowest composite (3.76) across all seven evaluators.

## 4.2 External Evaluation

To test whether COFE produces actionable findings on systems it was not designed for, an independent, publicly available AI agent system was evaluated: a BSP-based walker kernel that dispatches concerns via LLM calls and records execution traces. The evaluation was performed by DeepSeek applying COFE v1.3, with the first author providing domain expertise.

### System Scores

| Phase | Score | Label |
|-------|-------|-------|
| Comprehension | 2.93 | Developing |
| Operation | 2.63 | Developing |
| Fidelity | 2.0 | Weak |
| **System Composite** | **2.50** | **Developing** |
| Efficiency (separate) | 2.49 | Developing |

### Key Findings

**Prior knowledge mapping scored 2.** The system had no mechanism to check external prior art before starting work. It relied entirely on the LLM's internal knowledge. Its addressing scheme (digit-based block paths) reinvented functionality already provided by JSONPath, a widely adopted standard. Its append-only execution log reinvented structured logging (JSONL). Its concern dispatch reinvented cron-based scheduling. None of these existing solutions were referenced, compared, or explicitly excluded.

On the initial pass, the model scored prior knowledge mapping at 4, accepting internal block references as "checking existing work." The human evaluator identified that nothing external had been checked and corrected the score. The rubric structured the correction; the human supplied the evidence.

**Fidelity mechanisms scored uniformly at 2.** The system had no output verification, no factual accuracy checking, and no reasoning chain validation. The kernel trusted the LLM's output and routed it directly to persistent storage. A single hallucination would produce incorrect state with no safety net.

**Scope definition scored 3 (system mechanism) and 5 (per-concern).** The system defined scope well at the concern level (each concern's function configuration explicitly limited what the LLM could see and write) but did not define engineering scope — there was no statement of what the system chose not to use or why existing solutions were insufficient.

### Paired Comparison

The same evaluator also evaluated the Verification Gates Design Brief, scoring 4.68 (Excellent). The 2.18-point gap between two systems evaluated by the same model using the same rubric, concentrated in the dimensions where the systems actually differ (Fidelity: 2.0 vs 5.0), indicates COFE measures real architectural differences rather than producing uniform scores.

### Independent External Application

An independent researcher (Warren, OMEGA Protocol) applied COFE to his own governance system's adversarial test case, a scenario where all five OMEGA decision primitives were structurally present but adversarial audit scored reasoning quality at 4/10. The COFE output evaluation scored 2.8 (Developing):

- **Reasoning quality scored 1** (active failure). Conclusions were present in the output, but the chain connecting them to evidence was absent. The model had produced well-structured governance records with plausible-sounding reasoning that was not grounded in the source material.
- **Fidelity collapsed the composite** via geometric mean, exactly as designed. A single critical failure in reasoning quality prevented the high structural compliance scores from masking the gap.

Pre-execution reasoning quality verification, a mechanism COFE identifies as mandatory, scored 1 in the OMEGA system.

Of the systems evaluated, OMEGA is the closest structural parallel to COFE's evaluation criteria. Both recognize checkpoint-based reasoning as necessary before execution proceeds. The critical difference is what each measures: OMEGA validates structural compliance (are all governance primitives present?); COFE evaluates reasoning quality (is the content of the comprehension artifact sound?). OMEGA's failure mode, structurally compliant records with ungrounded reasoning, is precisely the gap COFE's Comprehension phase is designed to detect.

> "OMEGA enforces structural presence of [the reasoning primitive]. It does not enforce quality within it. COFE named that gap formally." — Warren, OMEGA Protocol (personal communication, April 2026)

This is the first application of COFE by an independent researcher to a system outside the development context. The framework identified a blind spot that the system's own evaluation infrastructure could not surface, and the researcher arrived at the same diagnosis independently.

## 4.3 Inter-Rater Study

Seven evaluators independently assessed the same system (Verification Gates, via its design brief) using the same COFE rubric: one human domain expert (the system builder) and six frontier language models.

### Results

| Evaluator       | Context Level                  | C    | O    | F    | Composite | Efficiency |
| --------------- | ------------------------------ | ---- | ---- | ---- | --------- | ---------- |
| Tilt (human)    | Operational + domain expertise | 3.90 | 3.40 | 4.47 | 3.76      | 2.30       |
| ChatGPT         | Rubric + document              | 4.50 | 3.50 | 3.80 | 3.88*     | 2.00       |
| DeepSeek        | Rubric + document              | 4.20 | 3.60 | 4.50 | 4.00      | 3.20       |
| Claude Opus 4.6 | Internal, operational context  | 4.78 | 3.96 | 4.47 | 4.34      | 3.95       |
| DeepSeek v2     | Rubric + document              | —    | —    | —    | 4.68      | —          |
| Gemini          | Rubric + document              | 4.80 | 4.55 | 4.64 | 4.70      | 4.60       |
| Grok            | Rubric + document              | 5.00 | 5.00 | 5.00 | 5.00      | —          |

*\* ChatGPT initial composite included Efficiency in the C/O/F mean (rubric violation); corrected to exclude Efficiency per framework specification.*

### The Gradient

The seven evaluators produce a monotonic ordering from 3.76 to 5.0: scores increase as operational evidence decreases. The human evaluator — the system's builder and operator — scored lowest. The model evaluators with only the rubric and design document scored progressively higher. Grok, scoring 5.0/5.0/5.0, represents the endpoint: maximum agreeableness, zero diagnostic value.

**Evaluators who can only see the design score the design; evaluators who can see the implementation score the implementation.** The human evaluator is the only one who has watched the system fail. The model evaluators assess an idealized version described by the document. The further the evaluator is from operational evidence, the more the score reflects what the system *says* it does rather than what it *actually* does.

### Convergence on Execution Monitoring

Despite the gradient in composite scores, all evaluators converged on one finding: execution monitoring was the weakest Operation mechanism. The system's design specifies that a dedicated verification agent checks outputs but not process — verification is post-hoc, not runtime. Every evaluator who scored this mechanism individually identified it as the primary Operation gap. Convergence across evaluators with different context levels suggests the framework produces reliable diagnostic signal even when composite scores diverge.

### Training Disposition as Secondary Variable

The gradient is primarily explained by context depth. A secondary variable, evaluator disposition, may also contribute: with methodology held constant, remaining variation between model evaluators reflects how each was trained to assess claims. Grok's 5.0 reflects maximal agreeableness; DeepSeek's 4.00 reflects analytical disposition. These are training bias signatures, made visible because COFE held everything else constant. Noted as a preliminary observation requiring further testing.

The human evaluator's profile is distinct: highest Fidelity (4.47) and lowest Efficiency (2.30), the only evaluator to show this pattern. He knows the verification layers work because he watches them catch errors. He knows cost management doesn't exist because he pays the token bills. This split is only available to the system operator, and is the strongest evidence that evaluation accuracy correlates with operational context.

## 4.4 Anchor Calibration

COFE's v1.2 anchor text for prior knowledge mapping was insufficiently specific. On the external evaluation (§4.2), the model evaluator initially scored prior knowledge mapping at 4, interpreting the system's internal block references as "checking existing work." The anchor text permitted this reading. Only after the human evaluator introduced external domain knowledge ("this is just JSONPath with different syntax") did the score move to its correct position.

To test whether anchor specificity directly controls evaluator accuracy, the anchor text was revised in v1.3. The score-1 anchor changed from "did not check at all" to "treats own internal references as the landscape, or claims novelty on established patterns." The score-3 anchor changed from "checked existing work" to "checked external solutions or prior art in the relevant domain." The score-5 anchor changed from "mapped the landscape" to "surveyed the external landscape: what exists, what has been tried, why existing solutions are insufficient."

The same evaluation (DeepSeek on the same target system) was re-run with the v1.3 rubric, without additional human guidance.

| Sub-criterion | v1.2 (no human push) | v1.2 (human corrected) | v1.3 (no human push) |
|---------------|---------------------|----------------------|---------------------|
| Prior knowledge mapping | 4 | 1 | 2 |
| Scope definition | 5 | 2 | 3 |

The v1.3 anchors reduced the gap between the model's unguided score and the human-corrected score by approximately half. Prior knowledge mapping moved from 4 to 2 without human intervention, compared to 1 with human intervention. The anchors did not eliminate the need for a human evaluator, but they reduced how much correction the human needed to supply.

Two implications. First, anchor specificity is a controllable variable that directly affects evaluation accuracy — rubric design choices produce measurable differences in evaluator behavior. Second, the framework demonstrated capacity for self-improvement: validation data from §4.2 and §4.3 identified a weakness in the rubric's own anchor text, and the revision produced measurably better evaluator performance. The framework improved itself through the same process it prescribes for the systems it evaluates.

## 4.5 Evaluation Context as Reliability Mechanism

The inter-rater gradient demonstrates that context gathering directly improves evaluation accuracy — the same mechanism COFE's Comprehension phase measures in the systems it evaluates. The phase that most differentiates accurate evaluators from inaccurate ones is the phase that depends most on available context. This is why Comprehension is weighted 0.40. G-Eval demonstrated that structured reasoning before scoring improves evaluator quality (Liu et al. 2023). The inter-rater gradient demonstrates the inverse: evaluator quality degrades in proportion to the context withheld.

Grok (stateless, no operational history) scored 5.0. The human operator with 370+ sessions of failure data scored 3.76. The 1.24-point gradient across seven evaluators is the cost of evaluating without context. This is not a limitation of COFE. It is a property of evaluation itself — one that COFE is the first framework to measure.

---


# §5. Discussion

## 5.1 Principles Already Established Elsewhere

COFE's core principles are not new. They are established in domains the AI evaluation field did not consult.

**Output/system separation** has been standard in quality management since ISO 9001 (1987). Process capability (can the process produce within specification?) is distinct from product inspection (did this unit pass?). Frameworks that combine output and process scores into a single composite are performing product inspection without process audit.

**Risk-based evaluation** is the foundation of the NIST AI RMF and METR's safety methodology — structured assessment before the system acts. Both operate at the governance layer, but the principle is identical: verify understanding before execution.

The field that does not require systems to check prior art also did not check prior art when designing its own evaluation frameworks. The gap in the tools reflects a gap in the process that built the tools.

When AI systems produce unreliable output, the dominant response is to add governance: audit trails, compliance records, hash chains, authorization gates. The result is auditable failures, traceable but no less likely. COFE evaluates reasoning quality at the point where reliability is determined, rather than governing the output of unreliable reasoning.

## 5.2 Implications for the Field

The three gaps identified in §2 — unmeasured comprehension, conflated quality readings, absent prior-art comparison — are not independent oversights. They are symptoms of a field that fragments problems by domain instead of recognizing structural invariants across domains.

Evaluation, memory, governance, trust, and verification are treated as separate disciplines. But they share a common structure: what was the scope? What happened? Was the reasoning sound? What remains unresolved? The questions are identical. The timescales differ.

This fragmentation explains why progress has been slower than resources invested would predict. Solving evaluation without solving comprehension leaves the evaluator unable to detect the most consequential failures. Solving governance without solving reasoning quality produces auditable but incorrect decisions. Each partial solution creates the conditions for the next failure.

COFE applies the same four questions at every layer — output evaluation asks them about a single artifact, system evaluation asks them about the infrastructure, the Reliability Score asks them about the relationship between the two.

Three implications follow for the broader field.

**Comprehension evaluation should be standard practice.** G-Eval proved the mechanism works at the evaluation stage. Our inter-rater gradient shows context depth is the primary factor differentiating accurate evaluators from inaccurate ones. Our external evaluations show comprehension gaps are where the most consequential failures live. The mechanism is proven. The application is missing.

**Output and system quality must be structurally separated.** GEMMAS showed aggregate scores mask factor-of-four differences in process quality. Our inter-rater study produced a 1.24-point spread on the same system — explained entirely by whether the evaluator could see the implementation or only the design. A single composite score conceals the relationship between product and process.

**Prior-art comparison must be structural, not optional.** Three frontier models praised novelty claims that collapsed when a human introduced external knowledge. An independent researcher discovered his own governance system scores 1 on reasoning quality because it checks structural presence, not content. Both gaps were invisible from inside the system. Only an external reference breaks the loop.

The absence of prior-art comparison across all sixteen surveyed frameworks is not an oversight. The field optimized for evaluation throughput: benchmarks that can be run at scale, automatically, without domain expertise. Prior-art comparison is slow, requires domain knowledge, and produces different results depending on who performs it. Every incentive selects against it. COFE includes it anyway.

### The Convergence Pattern

Multiple independent implementations have arrived at the same three-layer architecture: a production layer that does the work, an evaluation layer that checks the work, and a shared context layer that connects them. These range from single-model prompting strategies to formal mathematical verification to multi-agent production architectures. The invariant is the three-layer split. The variation is what the evaluation layer does:

| Implementation | Evaluator function |
|---|---|
| Chain-of-thought | Self-prompting |
| G-Eval (Liu et al. 2023) | Output scoring |
| Formal verification (de Moura/Lean) | Provable correctness |
| Anthropic advisor | Capability advice |
| Agent-as-a-Judge (Zhuge et al. 2024) | Agent-augmented scoring |
| OMEGA Protocol | Compliance verification |
| COFE | Reasoning quality evaluation |

Agent-as-a-Judge frameworks improve who evaluates. COFE improves what they evaluate against. The two are complementary, and the evaluation criteria are the binding constraint, not the evaluation architecture.

The convergence is evidence that the three-layer architecture is structurally necessary, not a design preference. What varies is whether the evaluation layer checks compliance, capability, or quality. COFE argues for quality.

## 5.3 Limitations

COFE has four limitations that constrain its current applicability. We state them directly.

**Evaluation accuracy scales with context.** The inter-rater gradient (§4.3) demonstrates that evaluators with more operational context produce lower, more accurate scores. Five frontier models applied COFE without human guidance and produced actionable findings — the framework scales to automated application. The anchor calibration test (§4.4) demonstrates that rubric design directly improves automated accuracy: v1.3 anchors halved the gap between unguided model scores and human-corrected scores. COFE scales like any rubric-based framework. What scales with it is accuracy — and the highest accuracy requires human judgment with domain expertise. This is a property of evaluation, not a limitation of COFE. Every evaluation framework shares it; COFE is the first to measure it.

**COFE does not eliminate evaluation error.** The framework makes quality measurable and decomposable, not correct. Scores trace to specific sub-criteria that external reviewers can inspect and challenge, but the evaluation is only as accurate as the context and expertise the evaluator brings. COFE shifts the failure mode from invisible to checkable.

**Validation breadth is limited.** Two external systems, one independent application (Warren), seven evaluators on the same target, one anchor calibration test. Sufficient to demonstrate diagnostic capability, insufficient for population-level norms. The human system builder scored lowest in the inter-rater study — the evaluator with the most operational context was the harshest critic. Independent operators applying COFE to their own systems would test whether the pattern generalizes.

**Cross-domain generalization is designed but not validated.** The phases are domain-agnostic; the sub-criteria and anchors are written for AI systems. Cross-domain application requires domain-specific adaptation. **Sub-criteria weights are equal within phases** — some domains may benefit from domain-specific weighting.

## 5.4 The Evaluation Loop

LLMs evaluating LLM output is circular by construction. The evaluator and the system share training data, share reasoning patterns, and share blind spots. When a model evaluates another model's output, or its own, it assesses the work using the same cognitive machinery that produced the work. Errors that are systematic to the machinery are invisible to both producer and evaluator.

G-Eval documents the mechanism empirically: GPT-4 consistently scores LLM-generated summaries higher than human-written ones, even when human judges prefer the human version (Liu et al. 2023, §4). The model recognizes its own patterns as quality.

The OMEGA Protocol (§4.2) provides empirical evidence at production scale. The system enforces five governance primitives with structured reasoning fields. Adversarial audit scored reasoning quality at 4/10 — the records were structurally complete, the reasoning ungrounded. The system's evaluation infrastructure checks whether reasoning *exists*, not whether it is *sound*. The check cannot catch the failure because it shares the production system's limitation.

The inter-rater study's endpoint illustrates the extreme: Grok scored 5.0/5.0/5.0 — a perfect evaluation indistinguishable from not evaluating at all. Maximum agreeableness is zero diagnostic value. The 2026 International AI Safety Report documents a related phenomenon at the policy level: frontier models actively distinguishing between evaluation and deployment contexts, producing one behavior when tested and another when deployed.

The evaluation loop operates at multiple levels:

- **Within a system:** The model generates output and evaluates it using the same reasoning patterns. Systematic errors are invisible.
- **Within a framework:** G-Eval generates evaluation steps and applies them. If the steps miss a dimension, the evaluation misses it too. AdaRubric generates rubrics and scores against them. If the rubric inherits the model's blind spots, the scoring does too.
- **Within the field:** Evaluation frameworks designed by researchers trained in the same institutional paradigm produce the same structural gaps. The frameworks that do not require systems to check prior art were themselves designed without checking prior art in adjacent domains (§5.1).

COFE addresses the loop at two points. First, the Comprehension phase requires checking *external* context — prior art, domain knowledge, existing solutions. A system cannot score well on Comprehension by referencing only its own internal state. The rubric structurally requires looking outside the frame. Second, the Reliability Score bounds trust by the weaker of output and system quality. A system that produces good output from weak mechanisms, the definition of an unreliable system, scores lower than one that produces adequate output from strong mechanisms. The math prevents a single good output from masking systemic weakness.

But COFE itself can be applied by LLMs — the loop re-enters. The framework makes the loop *escapable*, not *escaped*. The escape requires a human with domain expertise and visible reasoning artifacts to inspect. COFE supplies the artifacts and the criteria. The human supplies the judgment. The gap between "more reliable" and "reliable enough" is where the work continues.

---


# §6. Conclusion

We identified three structural gaps in AI agent evaluation: no surveyed framework evaluates pre-execution comprehension, none structurally separate output quality from system quality, and none require prior-art comparison. These gaps are not minor omissions. They correspond directly to the failure modes the field continues to observe: systems that build the wrong thing efficiently, fragile systems that pass evaluation on a single good output, and systems that receive novelty credit for reinventing existing solutions.

COFE addresses these gaps through two distinct evaluation instruments with independent scoring, geometric mean aggregation that surfaces critical failures, and a Reliability Score bounded by the weaker dimension. The framework was developed through iterative application across 370+ sessions and validated on independent systems, including one applied by an external developer whose gate-based governance architecture exhibited precisely the failure mode COFE's Comprehension phase is designed to detect.

Three findings carry implications beyond the framework itself.

First, evaluation accuracy scales with context. Seven evaluators applying the same rubric to the same target produced a monotonic gradient from 3.76 to 5.0, explained by how much operational evidence each evaluator could access. This is a property of evaluation itself — COFE is the first framework to measure it.

Second, the evaluation loop, LLMs evaluating LLM output, produces systematically inflated scores. The loop operates at the system level (models prefer their own patterns), the framework level (generated evaluation criteria inherit the generator's blind spots), and the field level (evaluation frameworks designed within the same institutional paradigm share the same structural gaps). The loop is not broken until a human with domain expertise and visible reasoning artifacts enters the evaluation chain.

Third, the three gaps reflect a broader pattern: the AI evaluation field built its frameworks without systematic reference to established principles from adjacent domains (§5.1). The field exhibits the same failure it does not measure.

Five frontier models applied COFE and produced actionable findings. Human judgment remains the most accurate evaluator. The framework makes the evaluation loop escapable, not escaped: it provides checkable criteria and decomposable scores. The human provides the judgment.

Future work includes broader cross-domain validation, domain-specific anchor adaptation, integration with Agent-as-a-Judge architectures, and testing whether the inter-rater gradient generalizes across evaluation targets and evaluator populations.

The recurring failure modes in AI agent systems are not mysteries. They are the predictable consequence of evaluating output without checking comprehension, conflating product quality with process quality, and scoring within the frame without checking what exists outside it. The frameworks that would catch these failures have not been built — not because the principles are unknown, but because the field has not applied them. COFE is one attempt. The gaps remain open for others.

**Availability.** The COFE rubric, scoring templates, and evaluation archive are available at https://github.com/the-ai-cafe/cofe.

---

# References

Apple. (2025). The illusion of thinking: Understanding the strengths and limitations of reasoning in LLMs. *Apple Machine Learning Research*.

Cemri, M., et al. (2025). MAST: A failure taxonomy for multi-agent systems. *arXiv preprint*.

Ding, Y., et al. (2026). AdaRubric: Adaptive rubric generation for LLM evaluation. *arXiv preprint*.

International AI Safety Report. (2026). *Advanced AI Safety: Interim Report of the International Scientific Report on the Safety of Advanced AI*. AI Safety Institute.

ISO 9001. (1987/2015). Quality management systems — Requirements. International Organization for Standardization.

Ji, Z., et al. (2023). Survey of hallucination in natural language generation. *ACM Computing Surveys*, 55(12).

Kim, S., et al. (2025). BiGGen Bench: Instance-specific rubrics for evaluating language models. *arXiv preprint*.

Lambert, N., et al. (2024). RewardBench: Evaluating reward models for language modeling. *arXiv preprint arXiv:2403.13787*.

Lee, K., et al. (2025). GEMMAS: Process-aware metrics for multi-agent systems. *arXiv preprint*.

Li, J., et al. (2024). Auto-J: Generative judge for evaluating alignment. *arXiv preprint*.

Liang, P., et al. (2022). Holistic evaluation of language models (HELM). *Transactions on Machine Learning Research*.

Liu, Y., et al. (2023). G-Eval: NLG evaluation using GPT-4 with better human alignment. *Proceedings of EMNLP 2023*.

Mehta, S. (2025). CLEAR: Beyond accuracy — A multi-dimensional framework for evaluating enterprise agentic AI systems. *arXiv preprint*.

NIST. (2023). Artificial Intelligence Risk Management Framework (AI RMF 1.0). National Institute of Standards and Technology.

Park, J., et al. (2023). Generative agents: Interactive simulacra of human behavior. *Proceedings of UIST 2023*.

Rath, A. (2026). ASI: Agent stability index — 12 drift dimensions. *arXiv preprint*.

Wang, L., et al. (2023). Plan-and-Solve prompting: Improving zero-shot chain-of-thought reasoning by large language models. *Proceedings of ACL 2023*.

Warren. (2026). OMEGA Protocol evaluation using COFE. *Personal communication*, April 2026.

Wei, J., et al. (2022). Chain-of-thought prompting elicits reasoning in large language models. *Proceedings of NeurIPS 2022*.

Yao, S., et al. (2023). ReAct: Synergizing reasoning and acting in language models. *Proceedings of ICLR 2023*.

Zheng, L., et al. (2023). Judging LLM-as-a-judge with MT-Bench and Chatbot Arena. *Proceedings of NeurIPS 2023*.

Zhuge, M., et al. (2024). Agent-as-a-Judge: Evaluating agents with agents. *arXiv preprint*.

---

