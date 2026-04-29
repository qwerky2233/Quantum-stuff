# Quantum Claim Overread Library — v0.1

> **Living document.** Append new patterns after each teardown cycle using the update template at the bottom.
> Last updated: 2025-04-29 | Maintainer: Task Node reviewer pool

---

## Purpose

This library exists because partial observability is the default condition for Task Node grading. When a contributor submits a technical markdown output about a quantum computing paper, the reviewer cannot re-run experiments, check raw data, or demand author clarification. The reviewer reads a claim, sees supporting evidence, and must decide: **accept, defer, or reject**.

The problem is that quantum computing is a field where the gap between what a result *proves* and what it *implies* is almost always larger than it looks. Papers are written to maximize apparent significance. Reviewers without domain-specific skepticism will over-credit routinely.

This library catalogs the specific overread patterns that recur across quantum paper teardowns: the places where a technically accurate claim is used to carry weight it does not support. Each pattern is described in terms of what evidence is typically shown, what reviewers are tempted to accept at face value, and what is actually warranted. Every pattern maps directly to an **accept / defer / reject** decision heuristic for use in Task Node grading.

**This is not an academic survey.** It is a reviewer calibration tool. Patterns are included because they appear repeatedly across teardowns — not because they are theoretically interesting. Evidence requirements for adding a new pattern are low: if a teardown flags it in the "Likely Misinterpretation" section more than once, it belongs here.

---

## How to Use This Library

See the [Reviewer Usage Guide](#reviewer-usage-guide) at the bottom. The short version:

1. Identify the type of claim the submission is built around.
2. Scan the pattern inventory for matching claim types.
3. Check whether the submission's supporting evidence actually supports the claim *at the level being credited*, or only at a weaker level.
4. Apply the accept / defer / reject logic for that pattern.

You do not need numeric thresholds. You need to know which pattern you are looking at.

---

## Pattern Inventory

---

### OVR-01 · Sparse Evidence Overread

**Claim type:** General advantage or mechanism claim backed by a single result, one hardware run, or a narrow pilot study.

**What is usually shown:** A single experimental run, a small-N benchmark on one hardware configuration, or a proof-of-concept on a toy instance. The result is real and correctly reported.

**What reviewers are tempted to credit:** That the claimed mechanism or advantage is established. Reviewers treat a single positive result as a confirmed finding rather than a preliminary signal.

**What is actually justified:** That the mechanism *plausibly exists* under the specific conditions tested. Nothing more. One result does not confirm reproducibility, generalizability, or robustness to system variation.

**Grading logic:**
- **Accept** if the submission correctly frames the result as preliminary and explicitly identifies what further validation would look like.
- **Defer** if the submission presents the result as established but the methodology is otherwise sound and the claim is merely stated too strongly.
- **Reject** if the submission presents a single sparse result as a confirmed finding and draws deployment or timeline implications from it.

**Seen in teardowns:** VQE pilot studies claiming "quantum advantage demonstrated," noise mitigation demonstrations on 5-qubit devices generalized to NISQ relevance.

---

### OVR-02 · Benchmark Mismatch

**Claim type:** Quantum performance claim compared against an inappropriate, outdated, or untuned classical baseline.

**What is usually shown:** A quantum algorithm that outperforms a specific classical algorithm on a specific instance size, with the comparison framed as evidence of quantum advantage.

**What reviewers are tempted to credit:** That the quantum approach is genuinely competitive with or superior to classical methods in the domain.

**What is actually justified:** That the quantum approach outperformed *that specific baseline* — which may be orders of magnitude weaker than the best available classical solver, may not have been optimized, or may represent classical performance from several years prior.

**Red flags:** Benchmark described as "standard" without citation; baseline is a textbook algorithm rather than state-of-the-art solver; problem instance is small enough that classical approaches are not expected to excel anyway; no acknowledgment of classical improvement trajectory.

**Grading logic:**
- **Accept** if the submission identifies the baseline explicitly, notes its limitations, and does not claim general advantage.
- **Defer** if the benchmark comparison is the central claim but the submission at least names the baseline and instance size clearly.
- **Reject** if the submission claims quantum advantage based on benchmark comparisons where the classical baseline is clearly untuned, outdated, or non-representative.

**Seen in teardowns:** QAOA optimization results compared against random or naive greedy solvers; QML classification results compared against untuned SVMs; quantum chemistry results benchmarked against methods pre-2018.

---

### OVR-03 · Hidden Scalability Assumption

**Claim type:** A result demonstrated at small qubit count is projected to scale favorably to problem-relevant sizes.

**What is usually shown:** An algorithm or circuit that works correctly on 5–50 qubits. Performance metrics are reported. A scaling argument is made — often in words, not with demonstrated data.

**What reviewers are tempted to credit:** That the result provides evidence for scalability to problem-relevant scale (hundreds to thousands of logical qubits for most industrially useful applications).

**What is actually justified:** That the result works at the demonstrated scale. Scaling arguments in quantum computing are almost always subject to noise, connectivity, and overhead assumptions that change qualitatively as system size grows. A scaling plot with 3–5 data points is not a scaling *proof*.

**Red flags:** "Scales as O(n²)" stated without demonstration above 20 qubits; circuit depth grows with system size but noise is not modeled at scale; SWAP overhead, qubit connectivity constraints, or error propagation not addressed in scaling analysis.

**Grading logic:**
- **Accept** if the submission claims only what was demonstrated and explicitly flags the gap to problem-relevant scale.
- **Defer** if scaling is asserted but with at least partial data support and acknowledged uncertainty.
- **Reject** if the submission treats a small-scale demonstration as evidence for scalability to practical problem sizes without demonstrating any trend.

**Seen in teardowns:** qLDPC codes demonstrated at 100 logical qubits with claims about million-qubit utility; VQA results at 12 qubits extrapolated to protein folding scale.

---

### OVR-04 · Mitigation Mistaken for Correction

**Claim type:** An error-mitigation technique is described or credited as if it provides fault-tolerant protection or equivalent guarantees.

**What is usually shown:** An error mitigation technique (ZNE, probabilistic error cancellation, symmetry verification, dynamical decoupling) that reduces observed error rates on a small circuit. The result is real and the technique is legitimate.

**What reviewers are tempted to credit:** That the system has meaningfully addressed the error problem — as if approaching fault-tolerant behavior.

**What is actually justified:** That errors were partially suppressed on a specific circuit under specific noise conditions. Mitigation does not provide threshold guarantees, does not scale deterministically, does not correct errors (only partially cancels their statistical effect), and typically comes with exponential sampling overhead that is not acknowledged.

**The distinction that matters:** Error *correction* encodes information redundantly and can, in principle, suppress errors below any desired threshold as resources scale. Error *mitigation* applies classical post-processing to reduce the impact of errors but cannot provide correction guarantees and does not suppress the logical error rate below a threshold.

**Grading logic:**
- **Accept** if the submission is precise about what mitigation provides vs. correction and does not conflate the two.
- **Defer** if the terminology is imprecise but the underlying result is correctly described.
- **Reject** if the submission explicitly or implicitly credits mitigation results as fault-tolerant behavior, or uses mitigation results to support timeline claims about practical quantum utility.

**Seen in teardowns:** ZNE demonstrations described as "error-corrected" in abstracts; dynamical decoupling results used to support claims about NISQ device readiness.

---

### OVR-05 · Certification Outside Deployment Regime

**Claim type:** A verification, benchmarking, or certification result from a controlled or idealized regime is applied as evidence of performance in a deployment-relevant regime.

**What is usually shown:** A certification protocol (randomized benchmarking, cross-entropy benchmarking, quantum volume measurement) run under controlled laboratory conditions produces a strong metric. The metric is then cited in support of broader performance claims.

**What reviewers are tempted to credit:** That the certification result provides direct evidence for performance on industrially relevant problem types.

**What is actually justified:** That the device meets the certified spec under the certified conditions. Certification regimes are almost never identical to deployment conditions: circuit depths differ, qubit connectivity patterns differ, thermal cycling and crosstalk characteristics differ at longer runtimes, and the problem structure (e.g., irregular entanglement patterns) is different from the randomized circuits used in benchmarking protocols.

**Grading logic:**
- **Accept** if the submission distinguishes between what the certification proves and what deployment performance requires, and identifies the gap.
- **Defer** if certification results are cited but the submission does not explicitly extend them to deployment claims.
- **Reject** if the submission cites a certification metric (quantum volume, RB fidelity, XEB score) as direct evidence of deployment-regime capability without addressing the regime gap.

**Seen in teardowns:** Quantum volume claims used to justify near-term utility without matching to target algorithm circuit structures; RB fidelity cited as evidence of error-corrected computation viability.

---

### OVR-06 · Simulated Performance Mistaken for Hardware Evidence

**Claim type:** A simulation result (classical simulation of quantum circuit, Clifford simulator, tensor-network simulator) is presented as evidence for what a quantum device would produce.

**What is usually shown:** A noise model is constructed, a simulated circuit is run, and the result is presented as representative of hardware performance. The paper may even frame results as "quantum" results with simulation as the methodology.

**What reviewers are tempted to credit:** That the result reflects what a real quantum device would actually do on the problem.

**What is actually justified:** That the system behaves as the noise model predicts. But noise models are not hardware. They miss correlated errors, drift, calibration variation, crosstalk, measurement errors not captured in depolarizing approximations, and tail events. Simulated performance systematically overstates hardware performance for non-trivial circuits.

**Red flags:** "Simulated with realistic noise model" as sole validation; no hardware comparison presented; noise model is Pauli-noise only; circuit complexity is deep enough that correlated errors would dominate on real hardware.

**Grading logic:**
- **Accept** if the submission clearly labels simulation as simulation and explicitly notes the hardware-simulation gap.
- **Defer** if simulation is the primary methodology but the submission acknowledges hardware validation as future work.
- **Reject** if the submission presents simulation results as hardware evidence, or uses simulated performance to justify hardware-dependent claims (timeline, utility, deployment).

**Seen in teardowns:** QML benchmark results from noise-model simulators cited as NISQ device performance; VQE ground state energy results from Qiskit Aer used to claim hardware utility.

---

### OVR-07 · Narrow Task Success Generalized into Broad Advantage

**Claim type:** Success on a specific, narrow task instance is generalized into a claim about advantage in the broader problem class.

**What is usually shown:** A quantum algorithm solves a specific instance of a problem (e.g., a particular graph, molecule, or optimization instance) with better performance than the classical baseline chosen.

**What reviewers are tempted to credit:** That quantum computing has demonstrated advantage for the problem class (e.g., "quantum chemistry," "combinatorial optimization," "graph partitioning").

**What is actually justified:** That the algorithm works for *that instance*. Problem instances are not generic samples. They are often chosen because they have structural features the quantum algorithm exploits. Generalizing from instance-level success to class-level advantage is unjustified without evidence that the instance is representative or that performance generalizes across the distribution.

**Red flags:** Single instance tested; instance size is small enough that classical methods are not computationally stressed; instance is synthetic rather than drawn from industrial problem distribution; no comparison against classically hard instances.

**Grading logic:**
- **Accept** if the submission claims only instance-level results and explicitly cautions against class-level generalization.
- **Defer** if the submission makes modest generalization claims but with some acknowledgment of instance-specificity.
- **Reject** if the submission generalizes a single instance result into domain-level or class-level advantage, especially if used to support timeline or deployment claims.

**Seen in teardowns:** Quantum portfolio optimization on 10-asset toy instances generalized to financial advantage; quantum graph problems on random regular graphs generalized to supply chain optimization.

---

### OVR-08 · Asymptotic Advantage Without Constant-Factor Accounting

**Claim type:** A quantum algorithm with a favorable asymptotic complexity is presented as superior to classical alternatives.

**What is usually shown:** Big-O complexity analysis showing that the quantum algorithm scales better than the classical best-known algorithm. Often accompanied by circuit depth or gate count analysis.

**What reviewers are tempted to credit:** That the quantum algorithm is practically faster or more efficient than classical approaches.

**What is actually justified:** That the quantum algorithm has a favorable *asymptotic* scaling. Practical quantum advantage requires that the crossover point (where quantum beats classical) falls within reachable system sizes given actual physical overhead. Quantum algorithms carry enormous constant-factor overhead from qubit connectivity, error correction, gate synthesis, state preparation, and measurement. These constants routinely push crossover points to billions or trillions of logical qubits — well beyond any near- or medium-term hardware trajectory.

**Grading logic:**
- **Accept** if the submission includes resource estimates (logical qubit count, circuit depth, T-gate count, magic state distillation overhead) and notes the crossover point explicitly.
- **Defer** if the asymptotic analysis is present and sound but the practical crossover is unaddressed.
- **Reject** if a favorable asymptotic complexity is used as the primary evidence for practical quantum advantage, without resource estimation or crossover analysis.

**Seen in teardowns:** Grover search applied to cryptographic or database problems cited as "quadratic speedup" without noting that classical methods are already extremely efficient; quantum linear algebra algorithms presented without noting QRAM assumptions.

---

### OVR-09 · QRAM Assumption Laundering

**Claim type:** A quantum machine learning or quantum linear algebra algorithm is claimed to provide exponential or polynomial speedup, with the speedup depending implicitly on QRAM (Quantum Random Access Memory) being available.

**What is usually shown:** An algorithm with polylogarithmic data access complexity. The speedup appears to be over classical algorithms operating on the same data. The assumption about how data is loaded into quantum memory is buried in assumptions, mentioned briefly, or omitted entirely.

**What reviewers are tempted to credit:** That the speedup is real and near-term relevant.

**What is actually justified:** That the speedup holds *if QRAM exists and can be efficiently implemented at scale*. QRAM is a hardware architecture that does not currently exist at useful scale, requires error correction overhead that scales with data size, and — for most problem structures — may require physical resources comparable to or exceeding what is gained by the speedup. Furthermore, "dequantization" results (Tang et al.) show that classical sampling methods often recover similar asymptotic performance.

**Grading logic:**
- **Accept** if the submission explicitly identifies QRAM as an assumption, describes the hardware requirements, and does not use the speedup to support near-term deployment claims.
- **Defer** if QRAM is mentioned in assumptions but the implications for practical timeline are not addressed.
- **Reject** if the claimed speedup depends on QRAM and the submission presents this as a near-term advantage, or omits QRAM as an assumption entirely.

**Seen in teardowns:** QML classification speedups that assume polylogarithmic data loading; quantum PCA and quantum recommendation system results that do not address dequantization literature.

---

### OVR-10 · Noise Model Optimism

**Claim type:** Hardware-level results are claimed to be "near fault-tolerant" or "approaching error-corrected" based on error rates, circuit fidelities, or gate counts that look good under current noise models.

**What is usually shown:** Gate fidelities at or above 99.9%, coherence times that exceed circuit depth requirements, noise model simulations that show small logical error rates. Results presented as evidence that the system is "close" to fault-tolerant operation.

**What reviewers are tempted to credit:** That fault-tolerant quantum computing is imminent or that the demonstrated hardware is nearly there.

**What is actually justified:** That the hardware performs well under *current characterization conditions*. The path from component-level fidelity to system-level fault tolerance is not incremental. At large scale, correlated errors, calibration drift, frequency collisions, crosstalk, and leakage errors all grow non-trivially. The threshold theorem's assumptions (independent errors, stable calibration, efficient syndrome extraction) are approximations that may not hold at system scale.

**Grading logic:**
- **Accept** if the submission acknowledges the gap between component-level fidelity and system-level fault tolerance, and identifies specific technical barriers remaining.
- **Defer** if the result is reported accurately but timeline claims are vague or hedged.
- **Reject** if component-level fidelity metrics are used to support near-term fault-tolerance or quantum utility claims without system-level analysis.

**Seen in teardowns:** Superconducting qubit two-qubit gate fidelities at 99.5% used to claim "fault-tolerant threshold achieved"; trapped-ion coherence time results used to project fault-tolerant timeline.

---

### OVR-11 · Verification Scope Narrowing (Unacknowledged)

**Claim type:** A verification or validation method is presented as applicable to a broad class of quantum computations, but was demonstrated only on a narrow class.

**What is usually shown:** A verification protocol (e.g., cross-entropy benchmarking, direct fidelity estimation, classical shadow tomography) demonstrated on specific circuit families, noise models, or qubit counts. The method is presented as "efficient" or "scalable" or "certifiable."

**What reviewers are tempted to credit:** That the method applies to general quantum computations of the type relevant to the reviewer's context.

**What is actually justified:** Verification was demonstrated for *that circuit family* on *those qubit counts* under *those noise conditions*. Verification methods have well-defined applicability domains that are routinely not stated in paper titles or abstracts.

**Why it matters for Task Node grading:** Reviewers are specifically assessing verification and benchmarking claims for post-fiat validator design relevance. Over-crediting verification scope is a direct path to building validation architectures on methods that don't transfer.

**Grading logic:**
- **Accept** if the submission is precise about the applicability domain and explicitly identifies circuits, noise regimes, or problem classes where the method would not apply.
- **Defer** if the scope is not fully characterized but the method is technically sound within the demonstrated domain.
- **Reject** if the verification method is presented as generally applicable to the class of computations the contributor is discussing, but was only demonstrated on structurally simpler circuits.

**Seen in teardowns:** XEB presented as a universal quality metric beyond random circuits; classical shadow tomography presented as scalable for general observables without noting shot complexity scaling with observable complexity.

---

### OVR-12 · Near-Term Heuristic Laundered as Asymptotic Result

**Claim type:** A variational or heuristic quantum algorithm (QAOA, VQE, VQD, ansatz-based method) is discussed in terms that imply provable performance guarantees.

**What is usually shown:** A variational algorithm that performs well on specific instances, with good empirical results. The discussion may include theoretical context about the algorithm's origins or relationship to adiabatic quantum computation.

**What reviewers are tempted to credit:** That the algorithm has theoretical performance guarantees — that it *will* produce good results across the relevant problem class, or that its performance is understood.

**What is actually justified:** Variational algorithms are heuristics. QAOA has no proven advantage for classically hard instances. VQE is not guaranteed to find the ground state for electronic Hamiltonians of problem-relevant size. Barren plateaus, local optima, and exponentially vanishing gradients are endemic to these methods at scale, and theoretical understanding of when and why they work remains incomplete.

**Grading logic:**
- **Accept** if the submission correctly identifies the algorithm as heuristic, acknowledges known failure modes (barren plateaus, local optima), and frames results as empirical.
- **Defer** if the algorithm's heuristic nature is implicit but the empirical framing is otherwise sound.
- **Reject** if the submission implies proven performance guarantees for variational algorithms, or uses variational algorithm results to support claims about asymptotic quantum advantage.

**Seen in teardowns:** QAOA p-level convergence results discussed as if implying convergence to global optima; VQE energy estimates on small molecules cited as evidence for "quantum chemistry advantage."

---

### OVR-13 · Advantage Claim Scoped to Regime Without Classical Competition

**Claim type:** Quantum advantage is demonstrated in a computational regime where classical algorithms are not expected to compete, and this is presented as evidence of practical quantum advantage.

**What is usually shown:** A quantum computation in the sampling, simulation, or many-body regime where classical simulation is known to be exponentially hard. The computation is performed, classical simulation fails to match, and this is framed as quantum advantage.

**What reviewers are tempted to credit:** That the demonstrated advantage is relevant to practically useful computations.

**What is actually justified:** That quantum hardware can perform computations that are hard to classically simulate. This does not imply that those computations are *useful* — they may be designed specifically to be hard to simulate without solving any problem of practical value.

**The Google Sycamore / random circuit sampling case:** Demonstrations of supremacy on random circuit sampling do not establish advantage on any useful computational problem. The regime of "classically hard but quantumly easy" does not overlap with the regime of "practically useful" without significant additional argument.

**Grading logic:**
- **Accept** if the submission correctly identifies the regime as demonstrating computational complexity separation without claiming practical utility.
- **Defer** if the claim is framed around "quantum advantage" generically but the specific regime is identified.
- **Reject** if supremacy or advantage in a classically-intractable regime is presented as evidence for practical quantum utility.

**Seen in teardowns:** Random circuit sampling results cited in discussions of near-term quantum utility for optimization; boson sampling demonstrations connected to claims about photonic quantum computing readiness.

---

### OVR-14 · Resource Estimate Omission (Physical Qubit Count)

**Claim type:** A fault-tolerant quantum algorithm is proposed and analyzed without reporting physical qubit requirements under realistic error correction overhead.

**What is usually shown:** Logical qubit counts, circuit depth in logical gates, or T-gate counts. Sometimes a surface code distance estimate. Physical resource requirements not stated or left as a secondary note.

**What reviewers are tempted to credit:** That the algorithm is near-term or medium-term feasible.

**What is actually justified:** Logical resource counts are the minimum floor. Physical qubit counts under surface code (the current best-practice error correction architecture) typically require 1,000–10,000 physical qubits per logical qubit depending on target logical error rate. A 100-logical-qubit algorithm at distance-15 surface code requires ~1.5–2 million physical qubits — a system that does not exist and may not for 10–15 years.

**Grading logic:**
- **Accept** if the submission reports or references physical qubit requirements explicitly and contextualizes them against current hardware trajectories.
- **Defer** if logical resources are reported and physical overhead is acknowledged as a gap.
- **Reject** if the submission discusses a fault-tolerant algorithm as feasible without reporting physical resource requirements or acknowledging the logical-to-physical translation overhead.

**Seen in teardowns:** Fault-tolerant quantum chemistry algorithms cited as "within reach" based on logical qubit estimates alone; quantum optimization algorithms with 50-logical-qubit requirements described without surface code overhead.

---

### OVR-15 · Application Label Overstretch

**Claim type:** An algorithm demonstrated on a mathematically clean proxy problem is labeled with the name of an industrial application domain.

**What is usually shown:** A quantum algorithm for a mathematical problem (MaxCut, graph partitioning, linear regression, portfolio variance) is benchmarked and shown to work. The paper's title, abstract, or conclusion connects this to an application domain (drug discovery, logistics, finance, materials design).

**What reviewers are tempted to credit:** That the quantum algorithm is meaningful for the named application domain.

**What is actually justified:** That the algorithm works on the mathematical proxy. Industrial problems in drug discovery, logistics, finance, and materials design differ from their mathematical abstractions in data scale, constraint structure, required precision, and solution quality tolerance. The gap between "solving MaxCut" and "routing supply chains" is not addressed by demonstrating MaxCut performance.

**Grading logic:**
- **Accept** if the submission is explicit that the application connection is illustrative and the mathematical problem is a simplified model.
- **Defer** if the application framing is present but not the central claim.
- **Reject** if the submission's central contribution depends on the application domain framing, and no argument is made for why the proxy captures the relevant structure.

**Seen in teardowns:** QAOA on random MaxCut instances cited in drug discovery context; quantum annealing on quadratic unconstrained binary optimization (QUBO) presented as logistics advantage.

---

## Reviewer Usage Guide

### Principles

**You are not doing a full technical review.** You are checking whether the submission's core claims are supported by the evidence it presents at the level it is claiming. A submission that makes a strong advantage claim backed by simulation-only evidence on 10 qubits is different from one that makes a narrow, qualified claim about a specific mechanism under specific conditions.

**Identify the claim type first.** Before checking evidence, identify what class of claim the submission is making. Is it a performance claim? A scalability claim? A certification claim? A timeline claim? Most overreads begin with a mismatch between claim type and evidence type.

**Scan the pattern inventory.** Once you know the claim type, scan the patterns above for a match. Many submissions combine multiple patterns — flag all that apply.

**Apply the pattern's grading logic.** Each pattern has an accept / defer / reject logic. Apply it to the specific claim. You do not need to make a binary decision; a submission can be partially accepted (core result accepted, advantage claim rejected).

**Flag the pattern, not the result.** Your role is not to determine whether the paper's underlying result is correct. It is to determine whether the submission is crediting that result with more than it proves. A true result can still be an overread if the conclusion drawn from it is unjustified.

### Decision Heuristics

| If you see this... | Likely pattern | Default posture |
|---|---|---|
| "Quantum advantage demonstrated" + simulation data only | OVR-06 | Defer / Reject |
| Performance benchmark with no classical baseline citation | OVR-02 | Defer |
| Small qubit count + scaling projection | OVR-03 | Defer |
| Error mitigation result + "near fault-tolerant" language | OVR-04 | Reject |
| Certification metric + deployment claim | OVR-05 | Defer / Reject |
| Variational algorithm + proven advantage language | OVR-12 | Reject |
| QRAM in assumptions + near-term speedup claim | OVR-09 | Reject |
| Application domain in title + toy instance in methodology | OVR-15 | Defer |
| Single result + broad advantage claim | OVR-01 | Defer |
| Logical qubit count + feasibility claim | OVR-14 | Defer |

### When to Defer vs. Reject

**Defer** when the underlying result is sound but the framing is too strong. The submission gets credit for the result, not for the conclusion. The contributor should revise the conclusion to match what was actually demonstrated.

**Reject** when the framing is not just too strong but constitutes a meaningful misrepresentation — where accepting the submission at face value would lead a reader to update beliefs or make decisions based on evidence that does not support those updates.

**Accept** when the submission correctly distinguishes between what was demonstrated and what it implies, and the implied conclusion is supported at the appropriate level.

### What Doesn't Count as an Overread

- Using forward-looking language if it is clearly labeled as speculation.
- Comparing against classical baselines if the comparison is explicit about what the baseline is and its limitations.
- Claiming novelty if the novelty is narrow and correctly described.
- Reporting asymptotic scaling if it is stated as asymptotic and physical overhead is acknowledged elsewhere.

---

## Changelog and Update Template

### Changelog

| Version | Date | Change |
|---|---|---|
| v0.1 | 2025-04-29 | Initial library. 15 patterns seeded from teardown synthesis. Reviewer guide drafted. |

---

### Monthly Update Template

When adding new patterns after a teardown cycle, use the following format. Paste this block below the last entry in the Pattern Inventory, increment the OVR number, and update the changelog.

```markdown
---

### OVR-[N] · [Pattern Name]

**Claim type:** [One sentence describing the class of claim this pattern applies to.]

**What is usually shown:** [2–3 sentences. What evidence is actually present in the paper? Be specific about what is real and correctly reported.]

**What reviewers are tempted to credit:** [1–2 sentences. What conclusion do reviewers tend to draw?]

**What is actually justified:** [2–3 sentences. What does the evidence actually support? Where is the gap?]

**Red flags:** [Optional. Specific textual or methodological markers that indicate this pattern is present.]

**Grading logic:**
- **Accept** if [condition].
- **Defer** if [condition].
- **Reject** if [condition].

**Seen in teardowns:** [Brief note on where this pattern appeared. Paper title or teardown slug preferred over vague reference.]
```

---

### Pattern Addition Criteria

A pattern qualifies for inclusion if:
- It appeared in the "Likely Misinterpretation" section of at least two teardowns in the current cycle, **OR**
- It led to a documented reviewer over-credit in a Task Node grading log, **OR**
- It represents a mechanism whose absence from this library would leave a systematic blind spot for reviewers evaluating a specific domain (e.g., quantum networking, quantum sensing, photonics).

Patterns should not be added based on theoretical interest alone. The library is a reviewer calibration tool, not a taxonomy of quantum computing epistemology.

---

*End of quantum-overread-library-v0.1.md*
