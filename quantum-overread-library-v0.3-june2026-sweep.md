---
title: Quantum Claim Overread Library — v0.3 (June 2026 Sweep Addendum)
maintainer: Task Node reviewer pool
last-updated: 2026-07-03
appends-to: quantum-overread-library-v0.2-may2026-sweep.md
---

# Quantum Claim Overread Library — v0.3 (June 2026 Sweep Addendum)

> **Living document.** This file appends to `quantum-overread-library-v0.2-may2026-sweep.md`, which in turn appends to `quantum-overread-library-v0.1.md`. Paste entries OVR-22 through OVR-26 into the Pattern Inventory of the base library immediately after OVR-21, then append the Decision Heuristic and Changelog rows below to their respective tables.
> Last updated: 2026-07-03 | Maintainer: Task Node reviewer pool

---

## June 2026 Sweep — Context and Selection Rationale

This sweep was seeded from 11 quantum teardowns completed in June 2026. The June set skewed heavily toward two subject areas that were thin or absent in prior sweeps: (a) LLM- and evolutionary-search-driven discovery of quantum artifacts (codes, encodings, weights, calibration procedures), and (b) qLDPC / QEC code construction. Five patterns qualified for library entry under the existing addition criteria — each recurred in the "Likely Misinterpretation," "Reusable Heuristics," or equivalent section of at least two teardowns in the cycle, or fills a systematic blind spot the prior library did not cover.

The dominant theme of the month is that the *source of a result* (a search engine, a classical simulator, a construction-time subroutine) is frequently conflated with the *validated capability* the headline implies. Three of the five new patterns are variants of that single confusion, separated because they trigger on different evidence and take different grading logic.

**Teardowns that seeded this sweep:**

| Teardown slug                                     | Patterns triggered              |
| ------------------------------------------------- | ------------------------------- |
| teardown-active-quantum-subspaces-2026-06-02      | OVR-25                          |
| teardown-scalable-qnn-training-2026-06-03         | OVR-24, OVR-25                  |
| teardown-qaoa-angle-setting-utility-scale-2025-06-05 | OVR-24                       |
| teardown-qasm-eval-2026-06-01                     | OVR-25                          |
| teardown-coset-qldpc-2026-06-18                   | OVR-26                          |
| teardown-pauli-propagation-noise-canceling-2026-06-22 | OVR-23                      |
| teardown-gse-evolved-encodings-2026-06-26         | OVR-22, OVR-23, OVR-24, OVR-26  |
| teardown-sce-qldpc-2026-06-26                     | OVR-22, OVR-26                  |
| teardown-qudit-iqp-calorimeter-2026-06-29         | OVR-23, OVR-27(cand.)           |
| teardown-vibe-calibration-2026-06-27              | OVR-22                          |
| teardown-riverone-2026-06-30                      | OVR-22, OVR-23, OVR-24, OVR-25  |

One pattern (aggregate-agreement-without-residual, provisionally OVR-27) appeared strongly in only one teardown (qudit-iqp-calorimeter) with a weaker echo in riverone; it is recorded in the changelog as a watch-list candidate rather than a full entry, pending a second clear occurrence. A cross-reference note on its relationship to OVR-19 is included below so reviewers can apply it in the interim.

---

## New Pattern Entries

---

### OVR-22 · Search-Engine Attribution Inflation

**Claim type:** A result produced by an automated search process — an LLM proposing mutations, an evolutionary loop, a fine-tuned agent executing a procedure — is credited to the search component's intelligence or novelty, when the load-bearing work was done by human-authored scaffolding (a grammar, a verifier, a fitness function, a decision tree, a construction template) that constrains the search to a space where most or all outputs are already valid and useful.

**What is usually shown:** A pipeline discovers or executes something real (a new code family, a compensation weight set, an end-to-end calibration run). The narrative foregrounds the search agent as the discoverer. The scaffolding — the algebraic template that makes every candidate valid, the exact verifier that filters outputs, the pre-hardened skill library — is described but not framed as the source of the result. Frequently the search component is a small or generic model, which the paper may even present as a strength ("cheap models suffice").

**What reviewers are tempted to credit:** That an AI system autonomously discovered new science, or autonomously performed a complex task, and that the intelligence of the search component is what produced the result. Downstream this becomes "AI discovers better quantum codes than humans" or "an LLM calibrated a 100-qubit chip."

**What is actually justified:** That the *combined system* — human-authored scaffold plus search proposer — produced the result within the space the scaffold defines. Attribution to the search component specifically requires evidence that a weaker or null proposer (random mutation, a simpler heuristic, a smaller model) does substantially worse under the same scaffold. Where the scaffold guarantees validity by construction, "the model cannot produce an invalid output" is a statement about the scaffold, not the model. Where a procedure was iteratively hardened by humans across earlier phases, "no human in this run" is not "no human in the loop."

**Red flags:** Novelty framing centered on the search agent while a verifier/grammar/template guarantees output validity; no ablation replacing the search proposer with a weaker or random proposer under the same scaffold; "cheap/small model suffices" presented without asking what that implies about where the intelligence lives; "fully autonomous" final run that depended on a human-authored artifact refined in prior phases; the phrase "discovered" applied to outputs drawn from a space a human defined.

**Grading logic:**

- **Accept** if the submission attributes the result to the whole system, and either provides a proposer-ablation (weaker/random proposer degrades results under the same scaffold) or explicitly scopes the novelty to the scaffold rather than the search agent.
- **Defer** if the result is real but the proposer-vs-scaffold contribution is not separated, and the framing leans on the search agent's intelligence without support.
- **Reject** if the submission cites the result as autonomous AI discovery or autonomous task completion when the validity, structure, or success criteria were human-authored, and no ablation isolates the search component's causal contribution.

**Relationship to OVR-17:** OVR-17 (Post-Processing Substitution) is the same causal-attribution failure applied to a *quantum-vs-classical* seed within a hybrid pipeline; OVR-22 applies it to a *search-agent-vs-scaffold* split within a discovery or automation pipeline. Both are answered by the same instrument: swap the credited component for a null baseline through the identical downstream and see whether the result survives.

---

### OVR-23 · Simulation-Provenance Erasure

**Claim type:** A metric obtained under a simplified or non-physical evaluation setting — a classical simulator, a code-capacity noise model, a construction-time subroutine that is discarded before deployment, or a system size that was never actually sampled — is reported in a way that reads as a hardware or deployment result.

**What is usually shown:** A headline ratio or quality figure (qubit savings, logical-failure reduction, accuracy, correlation) obtained in a setting chosen for tractability. The paper typically discloses the setting honestly somewhere — an appendix, a limitations paragraph, a methods line — but the abstract and figures present the number without the provenance qualifier attached.

**What reviewers are tempted to credit:** That the number describes what the system does on real hardware, at the stated scale, in a physically realistic noise environment.

**What is actually justified:** That the number holds *in the setting that produced it*. Code-capacity results (i.i.d. Pauli noise, perfect syndrome extraction) do not predict circuit-level behavior. A metric computed at a qubit count that was never sampled (because direct simulation is intractable there) is a classically estimated proxy, not a measured output distribution. A quantum subroutine that runs only at construction time and is frozen into classical tensors before inference contributes zero inference-time quantum capability, and its hardware feasibility (e.g. amplitude-encoding state-preparation depth) is a separate, usually unaddressed, question. "Trained and the loss converged" is not "sampled and compared against ground truth."

**Red flags:** Headline ratio whose noise model is code-capacity or "standard depolarizing" only, cited without that qualifier; largest-scale result relegated to an appendix as "preliminary" or expectation-value-only; a quantum component that is thrown away before deployment described in the title as if it runs on hardware; a quoted qubit count that reflects the encoding scheme rather than any executed circuit; no separation between "loss converged at scale N" and "distribution sampled and validated at scale N."

**Grading logic:**

- **Accept** if the submission attaches the evaluation setting to every headline number, keeps code-capacity and circuit-level results clearly separated, and does not extend a construction-time or simulator-only result to a hardware claim.
- **Defer** if the provenance is disclosed but not carried into the abstract/headline framing, so the central number reads stronger than the setting supports.
- **Reject** if the submission presents a simulator-only, code-capacity-only, unsampled, or construction-time-discarded result as evidence of hardware performance, deployment readiness, or advantage.

**Relationship to OVR-16:** OVR-16 (Correctness Frontier Erasure) concerns results that continue past the point of *classical verifiability* on real hardware. OVR-23 concerns results that never ran on hardware (or in a realistic noise model) in the first place. Check OVR-16 when the hardware ran past the verifiable frontier; check OVR-23 when the "result" was produced by a simulator, a code-capacity model, or a discarded construction-time step.

---

### OVR-24 · Weak-Baseline Framing

**Claim type:** A method is reported as competitive with, matching, or beating "the classical baseline," but the comparator is chosen to be weak — matched to the quantum system's own narrow operating point rather than to the strongest available classical method, or left unoptimized — so parity is achieved by handicapping the comparison rather than by the method's strength.

**What is usually shown:** A results table where the method ties or edges out a classical alternative. The classical alternative is width-matched to the quantum system (same number of neurons as qubits), or is a default/textbook choice rather than an optimized solver, or is a strong method that the paper declines to benchmark head-to-head. Often the paper's own text or ablation names the stronger baseline while the headline comparison uses the weaker one.

**What reviewers are tempted to credit:** That the method is competitive with classical state of the art for the problem, and therefore that the quantum (or novel) component is earning its place.

**What is actually justified:** That the method is competitive with *the specific baseline shown*. Competitiveness against a width-matched or unoptimized baseline says nothing about competitiveness against the classical optimum, which is almost always at larger capacity or is a purpose-built algorithm. When the paper's own ablation identifies a stronger classical method that beats the quantum result (e.g. a larger-hidden-layer network, or a polynomial-time algorithm with a provable guarantee), the honest headline is "does not beat the best classical baseline," not "matches classical methods."

**Red flags:** Classical baseline width-matched to qubit count rather than tuned for best performance; "matches classical baselines" where the paper's own table shows a stronger classical entry winning; comparison against a default solver instead of the domain's best (e.g. QAOA approximation ratios not measured against Goemans–Williamson); the strongest classical competitor named in prose but "declined" for head-to-head benchmarking; speedup or accuracy stated without identifying what the optimal classical operating point would be.

**Grading logic:**

- **Accept** if the submission benchmarks against the strongest reasonable classical method at its best operating point, or explicitly states that the comparison is width/scope-matched and does not claim general competitiveness.
- **Defer** if the comparison is against a plausible but non-optimal baseline and the gap to the true classical optimum is acknowledged but not measured.
- **Reject** if the submission claims competitiveness or advantage on the strength of a baseline that its own text or a routine check reveals to be handicapped, unoptimized, or narrower than the available classical optimum.

**Scope note:** This pattern is specifically about the *matching axis* — capacity, operating point, optimization effort — not merely which baseline was named. The diagnostic question is always: what is the optimal classical baseline's configuration, and is that the comparator?

---

### OVR-25 · Benchmark-Realism Substitution

**Claim type:** A capability is claimed on the strength of a benchmark that is structurally incapable of testing that capability — a synthetic dataset built to have the property the method exploits, a template-derived task set with no held-out distribution, a code-completion benchmark standing in for open-ended design, or an easy missingness/noise assumption standing in for the hard real-world one.

**What is usually shown:** Strong benchmark numbers on a task that is framed using the vocabulary of a harder real-world problem. The dataset is synthetic, reduced, or template-generated; the test/train split is enforced at a shallow level (variant) but not a deep one (theme/distribution); the task tests completion or recognition where the claimed capability is design or generalization.

**What reviewers are tempted to credit:** That strong benchmark performance is evidence for the real-world capability the benchmark's framing invokes (NISQ noise mitigation, clinical imputation, HEP simulation, protocol design).

**What is actually justified:** That the method performs well *on that benchmark's distribution*. When the benchmark is constructed to contain the structure the method needs (a known high-order interaction, a fixed low missingness rate, a template family shared between train and test), success demonstrates the mechanism operating under favorable conditions — not that the capability transfers to the real distribution, which is typically harder (structured missingness, unconstrained prompts, full-resolution data, adversarial noise). A benchmark that tests templated completion cannot speak to open-ended design.

**Red flags:** Synthetic dataset engineered to contain exactly the structure the method exploits; no held-out theme/distribution, only variant-level train/test separation; real-world dataset cited and figured while the trained system uses a drastically reduced projection of it; easy noise/missingness assumption (MCAR, fixed rate, i.i.d.) standing in for the hard real case (MAR/MNAR, drift, correlated); completion/recognition benchmark framed as evidence for design/reasoning capability.

**Grading logic:**

- **Accept** if the benchmark's realism is scoped honestly, the gap to the real-world distribution is stated, and the capability claim is confined to what the benchmark can actually test — ideally with a held-out distribution, not just held-out variants.
- **Defer** if the benchmark is synthetic or reduced but the paper acknowledges the realism gap and does not over-extend the claim.
- **Reject** if the submission uses performance on a structurally-easy or self-constructed benchmark to support a capability claim about the harder real-world problem the benchmark only names.

**Cross-reference — the "three-gate" discipline:** Where a paper offers a residual-screening or advantage criterion validated only on synthetic data (as in the active-quantum-subspaces case), the correct posture is to treat the criterion as a claims-audit tool and require re-application on the real distribution with the real noise model before crediting advantage. Passing a gate on synthetic data demonstrates the mechanism, not the advantage.

---

### OVR-26 · Uncertified-Quantity and Scope-Qualifier Stripping

**Claim type:** A reported figure is either an upper (or lower) bound presented as an achieved value, or a result whose stated novelty depends on a qualifier ("dense-molecular-graph," "competitive-with," "single-step") that gets dropped when the claim is summarized or cited.

**What is usually shown:** A code distance from a randomized decoder (an upper bound, not a certified value); a "first to achieve X" claim that is true only with a narrowing qualifier the abstract states but downstream citation omits; a "competitive with" result that migrates into "beats." The paper is often internally accurate — the qualifier or the bound-status is stated — but the load-bearing caveat is easy to strip on a fast read.

**What reviewers are tempted to credit:** That a bound is a measured value (so derived figures like kd²/n are treated as achieved), or that a qualified "first"/"competitive" claim is an unqualified one.

**What is actually justified:** That the quantity is an upper/lower bound and that any figure derived from it inherits that bound status ("at most this good"); and that the novelty claim holds only with every stated qualifier attached. "Competitive with the incumbent" is not "beats the incumbent." "First distance-5 *intrinsic dense-molecular-graph* code" is a different, weaker claim than "first distance-5 code." Non-monotonic or degrading finite-size scaling further undermines any implicit extrapolation of a bounded result to larger sizes.

**Red flags:** Distances or other quantities from randomized/heuristic estimators (e.g. QDistRnd) used as if certified; derived metrics (kd²/n, thresholds) built on bound-valued inputs without flagging; "first"/"novel"/"beats" claims whose truth depends on a qualifier that is present in the abstract but absent from the summary; "competitive with" quietly upgraded to "superior"; scaling asserted from a handful of sizes where the trend is flat or non-monotonic.

**Grading logic:**

- **Accept** if bounds are labeled as bounds wherever they (and their derived figures) appear, and every novelty/superiority claim carries its qualifiers explicitly.
- **Defer** if the bound-status or qualifier is stated once but not consistently carried into the headline figures, so a reader could reasonably over-read them.
- **Reject** if the submission treats an uncertified bound as an achieved value in a load-bearing figure, or asserts an unqualified "first"/"beats" claim that its own qualifiers or scaling data do not support.

**Relationship to OVR-20 and Heuristic "new codes exist vs. dominate":** OVR-20 governs bounded *operating regimes* (Clifford-only, noise windows). OVR-26 governs bounded *quantities* (uncertified distances) and stripped *novelty qualifiers*. A qLDPC construction paper will frequently trigger both: check OVR-20 for regime scope, OVR-26 for whether "competitive" has been upgraded to "dominant" and whether distances are certified.

---

## Decision Heuristic Table Update

Append these rows to the quick-reference table in the Reviewer Usage Guide.

| If you see this...                                                              | Likely pattern | Default posture |
| ------------------------------------------------------------------------------- | -------------- | --------------- |
| Search/LLM/agent credited for a result, no proposer-vs-scaffold ablation        | OVR-22         | Defer / Reject  |
| Headline ratio from a simulator, code-capacity model, or unsampled/discarded step | OVR-23       | Defer / Reject  |
| "Matches classical" against a width-matched or unoptimized baseline             | OVR-24         | Defer / Reject  |
| Strong numbers on a synthetic/templated benchmark framed as a real-world capability | OVR-25     | Defer / Reject  |
| Bound (e.g. QDistRnd distance) used as achieved value, or "competitive" read as "beats" | OVR-26 | Defer           |

---

## Reviewer Micro-Checklist (June additions, condensed)

Five questions to run against any June-style discovery/automation/construction paper, each mapped to its pattern. Each is phrased so the answer "not addressed" is itself a finding.

1. **Attribution (OVR-22):** If you replaced the clever component (LLM, evolutionary proposer, quantum subroutine) with a null or weaker one under the identical scaffold, would the result survive? Is that ablation present?
2. **Provenance (OVR-23):** For every headline number, in what setting was it produced — real hardware, a realistic noise model, a simulator, code-capacity, or a step discarded before deployment? Was that size actually sampled, or only trained/estimated?
3. **Baseline (OVR-24):** What is the strongest classical method's optimal configuration for this task, and is that the comparator — or was the baseline matched to the quantum system's narrow operating point?
4. **Benchmark (OVR-25):** Is the benchmark structurally capable of testing the claimed capability, or is it synthetic/templated/reduced in a way that guarantees the property being "demonstrated"? Is there a held-out distribution, not just held-out variants?
5. **Certification & scope (OVR-26):** Are reported quantities certified or bounds? Do the novelty/superiority claims survive once every stated qualifier is reattached?

---

## Changelog Update

Append the following row to the Changelog table in the base library.

| Version | Date       | Change                                                                                                                                                                                                                                                                                                                                          |
| ------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| v0.3    | 2026-07-03 | June 2026 sweep. 5 new patterns (OVR-22 through OVR-26) seeded from all 11 June teardowns. Covers: search-engine attribution inflation, simulation-provenance erasure, weak-baseline framing, benchmark-realism substitution, and uncertified-quantity/scope-qualifier stripping. Three of the five (OVR-22/23/24) formalize the month's dominant confusion — conflating the *source* of a result (search agent, simulator, weak baseline) with the *validated capability* claimed. Decision heuristic table and reviewer micro-checklist extended. One watch-list candidate recorded below (OVR-27, aggregate-agreement-without-residual) pending a second occurrence. Why this strengthens the guide: the prior library (OVR-01–21) was oriented toward physics/algorithm and PQC claims; the June additions close a blind spot for the fast-growing class of "AI-discovered / AI-automated quantum artifact" papers and for construction/simulation results dressed as hardware results, giving reviewers named patterns and a single reusable instrument (null-baseline substitution) for the most common June overreads. |

---

## Watch-List Candidate (not yet a full entry)

**OVR-27 (candidate) · Aggregate-Agreement Metric Without Residual Error.** A high aggregate agreement statistic (e.g. a correlation coefficient r ≈ 0.97 between target and generated correlation matrices) is presented as strong fidelity, when it only establishes correct *ranking* of relationships and is silent on *magnitude* fidelity; the companion element-wise residual (e.g. mean absolute difference |Λ| ≈ 0.25, indicating systematic underestimation) tells the fuller and less flattering story. Seen strongly in teardown-qudit-iqp-calorimeter-2026-06-29, with a weaker echo in teardown-riverone-2026-06-30. **Interim guidance:** when a paper reports only a correlation/agreement coefficient for a matrix- or distribution-level comparison, request the companion residual/magnitude-error metric before crediting fidelity; a coefficient without a residual is reporting the flattering half of the comparison. This overlaps with OVR-19 (Statistical Infrastructure Failure) in spirit — both concern under-powered evidence dressed as a conclusion — but is distinct in that the metric shown is not inferential at all; it is a descriptive statistic that answers a different question than the one the fidelity claim needs. Promote to a full entry on a second clear occurrence in a future sweep.
