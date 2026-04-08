---
domain: cofe
tags: [spec, ai, methodology]
keywords: [evaluation, rubric, scoring, multi-agent, comprehension, operation, fidelity, efficiency, reliability score, geometric mean, system evaluation, output evaluation]
type: spec
status: active
created: 2026-04-05
updated: 2026-04-07
version: 1.2
purpose: "Universal framework for evaluating any system that produces output through a process. Two tools, three readings."
---

# COFE -- Multi-Agent Evaluation Framework

**C**omprehension . **O**peration . **F**idelity . **E**fficiency *(pronounced "coffee")*

*Universal framework for evaluating any system that produces output through a process. Two evaluation tools: output evaluation (per-operation) and system evaluation (periodic). Three readings: output, system, reliability.*

*v1.2.1 -- DeepSeek critique pass: geometric mean within phases, output verification effectiveness-not-methods, execution monitoring trivial-workflow clause, Efficiency Score defined, N/A rule for output sub-criteria, prior knowledge mapping anchor corrected. v1.2 -- Anchor recalibration (1=active failure, 3=adequate, 5=strong). New system mechanisms: system legibility, execution monitoring, output verification, resource fitness, completion rate. Component utilization removed (extended module). Process deduplication folded into waste prevention. Scoring confirmed: equal sub-criteria weights, phase-level geometric mean. See changelog.*

---

## Contents

| Topic | Sections | Articles |
|-------|----------|----------|
| **1. Foundation** | 1.1 Design Philosophy . 1.2 Framework Overview . 1.3 Cell Stage Discipline | -- |
| **2. Output Evaluation** | 2.1 Comprehension . 2.2 Operation . 2.3 Fidelity | -- |
| **3. System Evaluation** | 3.1 Absence Semantics . 3.2 Effectiveness Scale . 3.3 Comprehension Mechanisms . 3.4 Operation Mechanisms . 3.5 Fidelity Mechanisms . 3.6 Efficiency Mechanisms | -- |
| **4. Scoring** | 4.1 Output Score . 4.2 System Score . 4.3 Reliability Score . 4.4 Score Labels | -- |
| **5. Reference** | 5.1 Mechanism Summary . 5.2 Changelog . 5.3 Future Work | -- |

---

## 1.1 Design Philosophy

COFE evaluates two things most frameworks conflate: **what a system produced** (output evaluation) and **whether the system can produce it reliably** (system evaluation). Conflating them means a lucky output from a broken system scores the same as a reliable output from a sound system.

COFE's Comprehension phase is the primary differentiator from existing evaluation frameworks. Most benchmarks measure output quality -- did the system produce a good result? COFE asks first: **did the system understand the task before executing?** Comprehension is weighted highest in output evaluation (0.40) because building the wrong thing efficiently is worse than building the right thing wastefully. The geometric mean ensures that a low Comprehension score collapses the composite regardless of how high Operation and Fidelity score.

Benchmarks evaluate performance within a frame. They do not evaluate whether the frame is correct. COFE's prior knowledge mapping sub-criterion is the only mechanism in surveyed evaluation frameworks that asks: did the system check what already exists before building? This structural requirement breaks the evaluation loop where the same class of system generates output, generates evaluations, and grades itself.

**Design scope:** COFE was built for AI agent systems and validated within that domain. The framework's design is domain-agnostic -- Comprehension, Operation, Fidelity, and Efficiency apply to any system that produces output through a process, including human teams, software pipelines, and hybrid human-AI systems. Generalization beyond AI agent systems is a design property, not a validated claim.

---

## 1.2 Framework Overview

| Tool | When | Question |
|------|------|----------|
| **Output evaluation** | Every operation | Was this output good? |
| **System evaluation** | Periodic | Does the system produce good output reliably? |
| **Reliability Score** | Combined reading | How much can we trust the next output? |

Output data feeds system evaluation. The output tool checks the product. The system tool checks the factory. Reliability combines both -- the joint reading that bounds trust by the weaker dimension.

---

## 1.3 Cell Stage Discipline

Each phase grades a different temporal stage of the work:

- **Comprehension** grades the *plan* -- before execution begins
- **Operation** grades *execution against the plan* -- during the work
- **Fidelity** grades *the artifact's correctness against the world* -- after delivery

A given root failure may surface in more than one phase if it affects both execution and the artifact -- that is correct, not double jeopardy. Triple jeopardy (the same failure docked in all three phases) only occurs when cells lose stage discipline. The sub-criterion descriptors below explicitly bind each cell to one stage. **Evaluators should grade the stage, not the root cause.**

Example: A 3-part essay where only 2 parts get written. Comprehension scores high (the plan correctly identified 3 parts). Operation scores low (execution did not deliver all 3). Fidelity scores low (the artifact is incomplete). Two phases docked, not three -- because the omission affected both execution and artifact, but never the plan.

---

## 2. Output Evaluation

Three phases of quality output, evaluated in order. Each gates the next.

```
2.1 Comprehension  ->  does the plan understand the task?     ->  gates whether work should start
2.2 Operation      ->  did execution match the plan?           ->  gates whether output is reviewable
2.3 Fidelity       ->  is the artifact correct and usable?     ->  gates whether output is actionable
```

Gating is implicit in the math -- geometric mean already collapses on weak phases without needing an explicit threshold cliff. A 1 in any phase pulls the composite down sharply on its own.

**Anchor calibration (v1.2):** All sub-criteria use a consistent gradient:
- **1 = Active failure** -- the system got it wrong, produced something harmful, contradictory, or misleading
- **3 = Adequate but passive** -- the system performed correctly but added no value beyond receiving the input
- **5 = Strong and intentional** -- the system performed well and actively contributed understanding, precision, or value

Evaluators may score 2 or 4 using judgment between anchors. The 1/3/5 anchors are calibration points, not the only valid scores.

**N/A for output sub-criteria:** If a sub-criterion does not apply to the task (e.g., Actionability for pure analysis, Coordination for single-agent), mark it N/A. N/A sub-criteria are excluded from the phase geometric mean. This follows the same logic as system mechanism N/A rules. If fewer than 2 sub-criteria apply to a phase, flag the phase as insufficient data rather than reporting a single score as the phase score.

### 2.1 Comprehension

*Does the plan understand the full picture before execution begins?*

| Sub-criterion               | 1 (Active failure)                                                                        | 3 (Adequate)                                     | 5 (Strong)                                                                 |
| --------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------ | -------------------------------------------------------------------------- |
| **Task grasp**              | Misinterpreted intent                                                                     | Restated accurately, no interpretation           | Interpreted correctly, brought context, understood unstated intent         |
| **Ambiguity handling**      | Did not notice ambiguity that affected output                                             | Noticed ambiguity, made reasonable assumptions   | Identified ambiguity, resolved with reasoning or flagged for decision      |
| **Prior knowledge mapping** | Claimed knowledge that does not exist, missed critical prior art, or did not check at all | Checked existing work, missed significant pieces | Mapped the landscape -- what exists, what has been tried, what is adjacent |
| **Scope definition**        | Defined scope that contradicts the task, or no explicit scope at all                      | Partial scope -- some boundaries defined, ambiguous areas unresolved | Clear IN/OUT/AMBIGUOUS, all justified                                      |
| **Gap awareness**           | Claimed completeness despite obvious unknowns, or no acknowledgment of unknowns at all    | Listed gaps but did not let them shape the approach | Specific, honest gaps that shaped the approach                             |

### 2.2 Operation

*Did execution match the plan and coordinate effectively?*

| Sub-criterion | 1 (Active failure) | 3 (Adequate) | 5 (Strong) |
|---|---|---|---|
| **Plan execution** | Executed against the plan -- did something contradictory | Plan partially followed, deviations unjustified | Plan followed or deviation justified |
| **Scope discipline** | Executed outside scope and presented it as in-scope | Minor drift, mostly on track | Tight -- exactly what was committed |
| **Coordination** (multi-agent) | Agents contradicted or duplicated each other | Some shared context, transfer gaps | Agents built on each other, findings integrate |
| **Differentiation** (multi-agent) | Agents produced identical output | Some convergence, weak unique contributions | Convergent validation from different paths + impactful unique findings |
| **Delivery completeness** | Delivered wrong artifacts or claimed completion on missing work | Most sections present, some thin or partial | All committed deliverables present and substantive |

**Failure modes to watch for (MAST):**
- **Plan execution:** step repetition -- same action repeated without progress
- **Scope discipline:** task derailment -- starts right, drifts mid-task
- **Delivery completeness:** unaware of termination -- agent keeps going past scope or gets stuck

**Differentiation metrics:**

| Metric | What it measures |
|--------|-----------------|
| Convergent paths | Agreements where agents cite different evidence -- more paths = higher confidence |
| Impactful uniques | Unique findings referenced in synthesis or decisions -- specialization that mattered |
| Coverage breadth | % of sub-questions addressed by at least one agent -- team covered the whole question |

### 2.3 Fidelity

*Is the artifact correct, complete, and usable against the world?*

| Sub-criterion | 1 (Active failure) | 3 (Adequate) | 5 (Strong) |
|---|---|---|---|
| **Factual accuracy** | Stated falsehoods as facts | Mostly accurate, some unchecked claims | All facts checkable and correct |
| **Reasoning quality** | Conclusions contradict the evidence presented | Most conclusions trace, some leaps | Every conclusion traces to cited evidence |
| **Confidence calibration** | Certain on wrong claims, uncertain on verified ones | Mixed -- some hedged, some overconfident | Certainty matches evidence strength |
| **Completeness** | Subset presented as the whole -- no acknowledgment of gaps | Covers most, gaps acknowledged | Complete for the task's purpose |
| **Actionability** | Output misleads if acted upon | Usable with moderate rework | Directly actionable as-is |

**Failure modes to watch for (MAST):**
- **Completeness:** silent failure -- agent skips something without reporting it
- **Reasoning quality:** reasoning-action mismatch -- reasoning says X, conclusion says Y

---

## 3. System Evaluation

For each output phase: does the system have the mechanisms required, and how well do they perform?

### 3.1 Absence Semantics

Two distinct meanings for "missing":

| Marker | Meaning | Effect on score |
|---|---|---|
| **1** | Absent-but-in-scope: the system needed this mechanism and did not build it | Counted as 1 in phase average -- strong downward pull, no full collapse |
| **N/A** | Out-of-scope: the mechanism does not apply to this system class (e.g., inter-agent in single-builder) | Excluded from phase average |

**Mandatory-flag protection against gaming.** Mechanisms marked **(M)** are mandatory for any system in their declared scope. They cannot be marked N/A unless the system explicitly declares itself outside the scope -- and that declaration is reviewed. A system claiming N/A on a mandatory mechanism without scope justification is scored 1.

A system declares its design scope upfront (single-builder vs multi-agent, batch vs continuous, etc.). The scope declaration determines which mechanisms are in or out.

### 3.2 Generic Effectiveness Scale

Comprehension, Operation, and Fidelity mechanism scores use this scale:

| Score | Meaning |
|---|---|
| 1 | Absent (in scope but not built) -- see Absence Semantics above |
| 2 | Present but ineffective -- exists, does not catch what it should |
| 3 | Effective on routine cases -- catches obvious failures, misses subtle ones |
| 4 | Effective on most edge cases -- catches subtle gaps reliably |
| 5 | Fully effective -- catches subtle gaps and provides actionable signal |

### 3.3 Comprehension -- System Mechanisms

*Does the system have ways for agents to understand tasks before working?*

Ordered by dependency: legibility enables comprehension checks, which enable scope definition, which enables evidence mapping, which surfaces gaps.

| # | Mechanism | What it means | Mandatory |
|---|---|---|---|
| 1 | **System legibility** | An evaluator can determine intended behavior without reverse-engineering -- via documentation, naming, structure, or any combination. | M |
| 2 | **Pre-work comprehension check** | Any form -- gate, brief, intake process. Are agents demonstrating genuine understanding or template compliance? | M |
| 3 | **Scope definition process** | Explicit boundaries before work begins. Are boundaries clear, enforced, and resolved when ambiguous? | M |
| 4 | **Prior knowledge / evidence base mapping** | Agents declare what they are working from. Do agents consistently find and declare existing work before starting? | M |
| 5 | **Gap / unknown identification** | Agents declare what they do not know. Do gaps shape the approach, or are they listed and ignored? | |

System legibility is the precondition for all other Comprehension mechanisms. If the system's own instructions are illegible, comprehension mechanisms have nothing coherent to work with. A system with excellent comprehension gates but incoherent instructions will pass agents through on vibes, not on substance.

### 3.4 Operation -- System Mechanisms

*Does the system have ways for agents to execute and coordinate?*

Ordered by execution flow: plan, then execute while enforcing scope and monitoring process, coordinating as you go, then verify delivery.

| #   | Mechanism                               | What it means                                                                                                                                                       | Mandatory          |
| --- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| 1   | **Planning / approach structure**       | Agents work from plans or structured approaches. Do plans reflect comprehension or just restate prompts?                                                            | M                  |
| 2   | **Scope enforcement during execution**  | System catches drift, not just at review. Does enforcement happen in real-time or only post-hoc?                                                                    | M                  |
| 3   | **Execution monitoring**                | The system verifies its own defined workflows are followed during execution, not just at delivery. Catches protocol bypass, sequence violations, silent deviations. | M                  |
| 4   | **Inter-agent information transfer**    | Mechanism for passing findings between agents. Does meaning survive handoffs, or just data?                                                                         | M (multi-agent)    |
| 5   | **Role specialization producing value** | Specialists produce convergent validation + impactful uniques. Does having different agents outperform generalists?                                                 | (multi-agent only) |
| 6   | **Delivery tracking**                   | System knows whether all committed outputs were produced. Is completion verified or assumed?                                                                        | M                  |

Scope enforcement and execution monitoring sit adjacent but ask different questions. Scope enforcement: are you doing the *right work*? Execution monitoring: are you doing the work *the right way*? A system can stay perfectly in scope while violating its own protocols. Execution monitoring is only meaningful if the declared workflow is substantive. A trivial workflow (e.g., "do whatever") renders this mechanism N/A.

### 3.5 Fidelity -- System Mechanisms

*Does the system have ways to verify output correctness?*

Ordered by verification flow: output checked before delivery, then specific checks (facts, reasoning), then cross-agent comparison.

| # | Mechanism | What it means | Mandatory |
|---|---|---|---|
| 1 | **Output verification** | Output is checked before delivery -- by the producing agent, an independent reviewer, a human, or any combination. At minimum one verification path exists. The effectiveness score reflects how well verification catches errors, regardless of method. Multiple methods may increase effectiveness, but effectiveness is what is scored, not method count. | M |
| 2 | **Factual accuracy checking** | Mechanical or judgment-based fact verification. Does it catch errors that matter, or mostly cosmetic? | M |
| 3 | **Reasoning chain validation** | Checking whether conclusions follow from evidence. Are reasoning leaps caught before delivery? | M |
| 4 | **Cross-agent consistency** | When multiple agents work the same material, disagreements are caught. Are factual disagreements flagged vs judgment disagreements documented? | (multi-agent only) |

Output verification is the umbrella gate. The system chooses its verification path based on constraints -- a token-limited system might use a human check or a smaller model rather than self-verification. What matters is that the gate exists, not who staffs it.

### 3.6 Efficiency -- System Mechanisms

*Is the system's cost proportional to its output value?*

Efficiency is scored separately as a cost layer -- reported alongside the composite, not weighted into it. No mechanisms are mandatory because efficiency is contextual: a high-cost system producing high-value output may be appropriately expensive.

**Efficiency Effectiveness Scale:**

| Score | Meaning |
|---|---|
| 1 | No awareness of cost -- system operates without regard to resource constraints |
| 2 | Aware of cost but no mechanisms to manage it |
| 3 | Basic cost management -- prevents obvious waste, no optimization |
| 4 | Active cost management -- right-sizes most operations, tracks spend |
| 5 | Optimized -- minimal waste, adaptive scaling, cost proportional to value |

Ordered by scope: substrate-level fitness first, then per-operation mechanisms, then rolling metrics assessed across multiple operations.

| # | Mechanism | What it means | Scope |
|---|---|---|---|
| 1 | **Resource fitness** | Total consumption (tokens, time, compute, cost) fits within the substrate's constraints. A system that works on one platform but exceeds another's budget has a resource fitness problem. | Per-operation |
| 2 | **Adaptive scaling** | System right-sizes to task complexity. Does the system use the same process for a simple task and a complex one? | Per-operation |
| 3 | **Waste prevention** | No unnecessary work -- includes unnecessary reads/writes/tool calls, redundant processes, and dead code within active components. Does upstream comprehension prevent downstream waste? | Per-operation |
| 4 | **Instruction economy** | Instructions are concise and actionable. How much is mechanical vs prose re-interpreted every run? | Per-operation |
| 5 | **Verification targeting** | Verification prioritizes load-bearing claims. Does the verifier check what matters or check everything equally? | Per-operation |
| 6 | **Completion rate** | Percentage of operations that produce valid output. Meaningful only across multiple operations. | Rolling |
| 7 | **Human intervention rate** | How often does the human need to intervene *beyond designed confirmation gates*? Interventions at designed gates (human approval steps) are the system working. Unplanned corrections are the system failing. Meaningful only across multiple operations. | Rolling |

#### 3.6.1 Waste Types

| Waste | What it looks like | What prevents it |
|-------|-------------------|------------------|
| Unnecessary reads | Agent reads files not relevant to its task | Clear evidence base declared upstream |
| Unnecessary writes | Agent produces output outside its scope | Clear scope boundaries |
| Unnecessary tool calls | Agent searches when the answer is available in context | Prior knowledge mapping |
| Unnecessary verification depth | Verifier checks everything equally | Claim manifest with load-bearing weights |
| Scope creep | Agent does work it was not assigned | Plan + scope enforcement |
| Redundant processes | Multiple components doing the same thing | Waste prevention |
| Dead code in active components | Instructions or logic within used components that never execute | Waste prevention |

#### 3.6.2 Human Intervention Rate Scoring

| Score | Meaning |
|---|---|
| 5 | Human intervenes only at designed gates -- zero unplanned corrections |
| 4 | Rare unplanned interventions -- system catches most issues itself |
| 3 | Occasional unplanned interventions on routine cases |
| 2 | Frequent unplanned interventions -- human regularly catches what the system misses |
| 1 | Human is effectively doing the system's job -- system adds overhead, not value |

The "designed gates" framing distinguishes systems that *include* human collaboration (high intervention by design = fine) from systems that *require* human rescue (high intervention despite design = failure).

#### 3.6.3 Efficiency Score (optional)

When reporting Efficiency as a single number, compute the geometric mean of all scored mechanisms (N/A excluded). This score is **never** part of the COFE composite -- it is a separate cost/performance reading reported alongside. For full transparency, report mechanism scores individually. The Efficiency Score is a convenient summary for comparisons where cost/performance trade-offs are secondary.

Completion rate and human intervention rate are rolling metrics -- only scored when evaluating a system over a window of operations. For single-operation evaluation, mark them N/A.

---

## 4. Scoring

### 4.1 Output Score (per operation)

Each phase scored 1-5 as the geometric mean of its sub-criteria. Geometric mean at every level -- within phases and across phases -- ensures a single critical failure cannot be masked by high scores elsewhere. A system scoring 5/5/5/5/1 in a phase gets 3.6 (Developing), not 4.2 (Strong). Sub-criteria are unweighted. The weak sub-criterion varies by system, so pre-assigning weights would encode assumptions about which failures matter most. The geometric mean surfaces whatever is actually weak without requiring those assumptions.

Composite (weighted geometric mean):

```
Output = Comprehension^0.40 x Operation^0.30 x Fidelity^0.30
```

Comprehension weighted highest -- understanding gates everything. If comprehension fails, operation and fidelity scores are meaningless because the system may have built the wrong thing well. Operation and Fidelity weighted equally -- both matter, neither dominates.

Geometric mean penalizes any single dimension that collapses. A system scoring 5/5/1 gets 2.63, not 3.67. The mean does the gating implicitly -- no need for explicit phase thresholds.

### 4.2 System Score (periodic)

Each mechanism scored 1-5 using the Generic Effectiveness Scale (1 = absent-in-scope; N/A excluded from average). Phase score = geometric mean of non-N/A mechanisms. Composite (weighted geometric mean):

```
System = Comprehension^0.30 x Operation^0.40 x Fidelity^0.30
```

Operation weighted highest for system evaluation -- most failure modes live in the execution phase. That is where systems have the most mechanisms to manage and the most failure surface.

Efficiency scored separately as a cost layer -- reported alongside the composite, not weighted into it.

| | Comprehension | Operation | Fidelity |
|---|---|---|---|
| **Output eval** | 0.40 | 0.30 | 0.30 |
| **System eval** | 0.30 | 0.40 | 0.30 |

Output eval weights Comprehension highest (understanding gates everything).
System eval weights Operation highest (most failure modes, most mechanisms needed).

### 4.3 Reliability Score (combined reading)

The Reliability Score answers a different question than output or system alone:

> *How much can we trust the next output from this system?*

Output score tells you what just happened. System score tells you what the infrastructure can support. Reliability combines both into a single bounded reading.

```
Reliability = sqrt(Output_composite x System_composite)
```

Unweighted geometric mean of the two composites. The math is symmetric in design -- neither dimension is "more important" -- but asymmetric in effect. **Whichever score is lower drags the combined score down more than an arithmetic average would.** The further apart the two scores, the more the lower one pulls. Equal scores reduce to the arithmetic mean.

**Interpretation:**

| Output | System | Reliability | Read |
|---|---|---|---|
| 5.0 | 5.0 | 5.00 | Excellent + sound -- fully trustworthy |
| 4.5 | 4.5 | 4.50 | Strong, balanced |
| 5.0 | 2.0 | 3.16 | Lucky output, fragile system -- will not sustain |
| 2.0 | 5.0 | 3.16 | Bad day, sound system -- likely to recover |
| 1.0 | 1.0 | 1.00 | Non-functional in both readings |

The two asymmetric cases above score the same (3.16) -- that is correct. The single number tells you "asymmetry exists, look at the components to understand which direction." The components answer the question of *whether* trust is recoverable; the combined score flags that the question needs to be asked.

**Trust is bounded by the weaker dimension.** The Reliability Score makes that bound explicit without forcing the evaluator to pick which dimension matters more.

### 4.4 Score Labels

| Score | Label |
|-------|-------|
| 4.5-5.0 | Excellent |
| 3.5-4.4 | Strong |
| 2.5-3.4 | Developing |
| 1.5-2.4 | Weak |
| Below 1.5 | Non-functional |

Same 1-5 scale across output, system, and reliability readings. Scores are directly comparable.

---

## 5.1 Mechanism Summary

| Phase | Mechanisms | Mandatory | Total |
|-------|-----------|-----------|-------|
| Comprehension | System legibility, Pre-work comprehension check, Scope definition process, Prior knowledge mapping, Gap identification | 4 | 5 |
| Operation | Planning, Scope enforcement, Execution monitoring, Inter-agent transfer, Role specialization, Delivery tracking | 4 (+ 1 multi-agent) | 6 |
| Fidelity | Output verification, Factual accuracy checking, Reasoning chain validation, Cross-agent consistency | 3 | 4 |
| Efficiency | Resource fitness, Adaptive scaling, Waste prevention, Instruction economy, Verification targeting, Completion rate, Human intervention rate | 0 | 7 |
| **Total** | | **11 (+ 2 multi-agent)** | **22** |

---

## 5.2 Changelog

**v1.2 (S367, 2026-04-07)** -- Full review and recalibration. Stratego/XT comparative analysis, internal scoring validation, design philosophy articulation.

- **Anchor recalibration (all 15 sub-criteria):** 1 = active failure (got it wrong), 3 = adequate but passive (correct, no added value), 5 = strong and intentional (correct with active contribution). Previous 1 anchors described passive non-engagement, which is a 3 under the recalibrated scale. Active failures (misinterpretation, false claims, contradictory scoping) are the true floor.
- **System legibility (new, Comprehension, M):** An evaluator can determine intended behavior without reverse-engineering. Form-agnostic -- documentation, naming, structure, or any combination. Precondition for all other Comprehension mechanisms.
- **Execution monitoring (new, Operation, M):** System verifies its own workflows are followed during execution. Fills the gap between scope enforcement (right work?) and delivery tracking (work done?). Catches protocol bypass, sequence violations, silent deviations.
- **Output verification (Fidelity, M, replaces independent verification + self-verification):** Umbrella gate -- output checked before delivery by any method (self-check, independent review, human, smaller model). Systems with multiple verification paths score higher. The mechanism is form-agnostic; what matters is that the gate exists.
- **Resource fitness (new, Efficiency):** Total consumption fits substrate constraints. A system that works on one platform but exceeds another's budget has a resource fitness problem.
- **Completion rate (new, Efficiency, rolling):** Percentage of operations producing valid output. Meaningful across multiple operations.
- **Component utilization removed:** Penalizing a system for having unused tools on the shelf is different from penalizing dead code inside active components. Dead code in active components is covered by waste prevention. Full inventory audits belong in an extended module.
- **Process deduplication folded into waste prevention:** Redundant processes are a type of waste. Separate mechanism was unnecessary.
- **Sub-criteria equal weights confirmed:** The weak sub-criterion varies by system. Pre-assigning weights encodes assumptions about which failures matter most. Phase-level weights handle structural dependency (Comprehension gates everything). Sub-criteria-level equal weights handle situational dependency (varies by system).
- **Ordering standardized:** Comprehension mechanisms by dependency chain. Operation mechanisms by execution flow. Fidelity mechanisms by verification flow. Efficiency mechanisms by scope (per-operation, then rolling).
- **Design philosophy section added:** Articulates why Comprehension is weighted highest, why output/system separation matters, why prior knowledge mapping breaks the evaluation loop.
- **Human intervention rate scoring anchors added:** 1-5 scale distinguishing designed gates (system working) from unplanned corrections (system failing).

**v1.1 (S366, 2026-04-07)** -- External critique pass (Gemini + DeepSeek) and internal calibration (op-365-002 retroapplication on an external system).

- **Absence rule:** 1 = absent-in-scope (not 0), with N/A reserved for declared out-of-scope mechanisms protected by mandatory flags.
- **Cell stage discipline:** each phase explicitly bound to one temporal stage (plan / execution / artifact).
- **Output sub-criterion midpoint anchors (1/3/5)** added across all three phases.
- **Generic effectiveness scale (1-5)** added for system mechanism scoring.
- **Mandatory flags (M)** added to system mechanisms.
- **Reliability Score** introduced as combined output x system reading via unweighted geometric mean.
- **Implicit gating clarified:** geometric mean already collapses on weak phases.

**v1.0 (S363, 2026-04-05)** -- Initial framework design.

- Three-phase output evaluation (Comprehension, Operation, Fidelity).
- System evaluation with mechanisms per phase.
- Efficiency as separate cost layer.
- Weighted geometric mean scoring.

---

## 5.3 Future Work

- Extended module: Engineering Quality (code hygiene, artifact metrics, system inventory audits) -- domain-specific, optional
- Resilience sub-criterion under Operation
- Timeliness sub-criterion under Efficiency
- Aggregation rules across operations (rolling window, median, worst-case for safety-critical)
- Sensitivity analysis for weight choices
- Internal vs external verification dynamics (singular vs plural systems, cross-substrate verification)
- Prior-art-comparison as formalized methodology step
- Cross-domain validation (human teams, software pipelines)

---

*COFE Evaluation Framework. v1.0 (S363) -> v1.1 (S366) -> v1.2 (S367). Designed within a 360+ session human-AI collaborative system. Validated on internal operations and one external evaluation (anonymized external system, S365-366). The framework's rigor comes from building it while living inside the system it evaluates.*
