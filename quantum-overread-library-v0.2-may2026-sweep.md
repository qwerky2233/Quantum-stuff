# Quantum Claim Overread Library — v0.2 (May 2026 Sweep Addendum)

> \*\*Living document.\*\* This file appends to `quantum-overread-library-v0.1.md`. Paste entries OVR-16 through OVR-21 into the Pattern Inventory of the base library immediately after OVR-15, then update the changelog.
> Last updated: 2026-05-31 | Maintainer: Task Node reviewer pool

\---

## May 2026 Sweep — Context and Selection Rationale

This sweep was seeded from 13 quantum and PQC teardowns completed in May 2026. Six patterns qualified for library entry under the existing addition criteria: each appeared in the "Likely Misinterpretation" section of at least two teardowns in the cycle, or represents a domain (post-quantum cryptography) whose absence from the library constitutes a systematic blind spot for PFT validator reviewers.

**Teardowns that seeded this sweep:**

|Teardown slug|Patterns triggered|
|-|-|
|teardown-fermi-hubbard-qctrl-2026-05-06|OVR-16, OVR-18|
|teardown-protein-folding-bfdcqo-trapped-ion-2026-04-30|OVR-16, OVR-17|
|teardown-hybrid-qml-timeseries-2026-05-26|OVR-16, OVR-17, OVR-19|
|teardown-qem-statistical-artefacts-2026-05-29|OVR-17, OVR-19|
|teardown-ibm-rlc-dae-solver-2025-05-13|OVR-18|
|teardown-riccati-rpa-quantum-2025-05-18|OVR-18|
|teardown-clinr-gse-hamiltonian-2025-05-11|OVR-20|
|teardown-fire-and-ice-partial-ft-2026-05-18|OVR-20|
|teardown-syndrome-resampling-qec-2026-05-08|OVR-20|
|teardown-pqc-lwe-limitations-2026-05-07|OVR-21|
|teardown-pqc-stabilizer-decoding-2026-05-12|OVR-21|

Four teardowns (teardown-lottery-bp, teardown-pqc-ttfb-cdn, teardown-riccati-rpa, teardown-pqc-stabilizer-decoding) raised domain-specific issues that are partially addressed by existing patterns (OVR-02, OVR-05) and did not produce a second occurrence of a new distinct pattern this cycle. They are noted in the changelog as candidates for a future sweep if they recur.

\---

## New Pattern Entries

\---

### OVR-16 · Correctness Frontier Erasure

**Claim type:** A quantum result is reported across a range that includes both a verified zone (where classical or independent validation is available) and an unverified zone (where no independent check exists), and the two zones are presented as visually or narratively continuous.

**What is usually shown:** A simulation heatmap, energy landscape, or wavefunction trajectory that extends past the point where classical methods can verify correctness. The paper explicitly labels the verification boundary — "indeterminate," "beyond classical reach," or equivalent — but the visual presentation (smooth curves, continuous color gradients, unbroken time-axis) makes the verified and unverified regions indistinguishable at a glance. The paper's own caveats are present but not foregrounded.

**What reviewers are tempted to credit:** That the quantum result is validated across the full displayed range, including the unverified extension. A headline percentage (e.g., "3000× faster") or agreement metric is derived at the verification boundary and then exported to describe the entire run.

**What is actually justified:** That the quantum hardware produces correct outputs up to the verified frontier. Beyond that point, the result is existence evidence — the hardware continues to produce outputs — but not validated evidence. The continuation may be scientifically interesting as an exploration of classically hard regimes, but it cannot be used to support advantage claims, timeline claims, or accuracy benchmarks.

**Red flags:** Visual continuity between verified and unverified regions with no boundary marker; advantage headline derived at the frontier but presented as applying to the full result; phrase "beyond the reach of classical methods" in abstract without clarifying this means unverifiable, not superior.

**Grading logic:**

* **Accept** if the submission correctly identifies the verification boundary, distinguishes demonstrated from unverified results, and does not import advantage claims from the unverified zone.
* **Defer** if the boundary is mentioned but the advantage headline conflates the two zones.
* **Reject** if the submission presents unverified continuation as validated performance, or uses results from the classically-intractable regime to support deployment or timeline claims.

**Seen in teardowns:** Fermi-Hubbard Q-CTRL dynamics beyond t≈5.2 presented as smooth validated output; protein-folding BF-DCQO energy landscapes on 61-qubit instances where consensus pipeline fails silently; hybrid QML hardware results cited as utility-scale evidence.

\---

### OVR-17 · Post-Processing Substitution

**Claim type:** A hybrid quantum-classical workflow reports a performance metric that is in fact produced primarily by the classical post-processing component. When the post-processing is run with a non-quantum (random or classical) seed in place of the quantum output, the reported benefit disappears or reduces substantially.

**What is usually shown:** A hybrid pipeline — quantum sampling or feature extraction followed by classical optimization, consensus, or regression — achieves a benchmark metric (solution energy, forecast accuracy, error rate). The quantum step is presented as the source of the improvement. The paper may include a comparison using a random or classical seed, but this comparison is buried in a supplementary figure or framed as a minor ablation rather than the central validity test.

**What reviewers are tempted to credit:** That the quantum circuit is the operative source of the claimed improvement, and that the result supports the general viability of quantum-assisted computation for the problem class.

**What is actually justified:** That the hybrid pipeline achieves the reported metric under the specific conditions tested. Attribution requires demonstrating that swapping the quantum step for a classical or random baseline degrades the output — not merely that the quantum step was present. If performance under a classical seed matches or nearly matches performance under the quantum output, the quantum circuit is not causally responsible for the result.

**Red flags:** Ablation comparing quantum vs. random seed absent or relegated to supplementary material; "per-sample" vs. "consensus" or "warm-start" vs. "cold-start" comparison shows near-equivalent performance; post-processing design is changed simultaneously with the quantum step, confounding attribution; the paper trains on a simulator and runs inference on hardware, decoupling training cost from hardware cost.

**Grading logic:**

* **Accept** if the submission includes an explicit ablation with a classical or random seed through the same post-processing pipeline, and the quantum seed provides a statistically significant improvement in that comparison.
* **Defer** if the ablation is present but uses a non-equivalent post-processing configuration, making attribution ambiguous.
* **Reject** if the submission presents a hybrid pipeline result as quantum advantage without a same-pipeline classical-seed comparison, particularly when the post-processing is known to be powerful (consensus, optimization, regression).

**Seen in teardowns:** BF-DCQO protein folding: per-sample repair removes quantum advantage (buried in Fig. 4); hybrid QML timeseries: KQRC-RM degrades below persistence baseline on hardware; QEM benchmark review: ZNE improvement is parameter-contingent and cannot be separated from post-processing choices without ablation.

\---

### OVR-18 · Oracle / Query Model Laundering

**Claim type:** A quantum speedup is computed in an oracle query complexity model where one or more oracles — block-encodings, QROM data loading, sparse matrix access, state preparation routines — are treated as free or O(polylog) operations. The physical cost of constructing or accessing those oracles is either omitted entirely or footnoted without bounding.

**What is usually shown:** A gate complexity or query complexity scaling (e.g., Õ(polylog N), Õ(V · m³)) with an explicit or implicit assumption that the relevant data structure can be accessed in sub-linear time. The paper may acknowledge the oracle assumption in a section on future work or limitations, but the main result and abstract do not quantify its cost. Resource estimates, if present, are expressed in terms of logical operations with oracle access already assumed.

**What reviewers are tempted to credit:** That the stated speedup is over a wall-clock or physical resource comparison with classical methods. The polylog or polynomial complexity is imported directly into timeline or deployment arguments.

**What is actually justified:** That the algorithm achieves the stated complexity *conditional on oracle access*. This is a standard result in quantum algorithms theory, but it becomes a misleading advantage claim when the oracle cost is not subtracted. For QROM, the circuit cost scales with data size and dominates over the algorithmic speedup at realistic problem scales. For block-encodings of sparse matrices, the encoding circuit cost scales with sparsity structure and is rarely polylog in practice. The speedup may be entirely absorbed by oracle construction overhead.

**Red flags:** Algorithm complexity stated in oracle query model; QROM, block-encoding, or state preparation treated as O(1) or O(polylog); "exponential speedup" in abstract without resource estimate; no circuit depth or T-gate count for any representative problem instance; classical comparison cited as worst-case theoretical bound rather than benchmarked against an optimized solver.

**Grading logic:**

* **Accept** if the submission explicitly characterizes the physical cost of constructing the oracle or block-encoding for a representative problem instance and demonstrates that this cost does not absorb the speedup.
* **Defer** if oracle cost is acknowledged as future work and the algorithmic result is presented modestly as a theoretical foundation.
* **Reject** if the submission uses an oracle-model speedup to support near-term deployment claims, timeline projections, or practical advantage arguments without bounding oracle construction cost.

**Seen in teardowns:** IBM RLC/DAE solver: polylog(N) speedup stated without resource estimate; spectral gap condition and oracle access unverified for realistic VLSI netlists; classical comparison is worst-case model, not benchmark against SPICE. Riccati/RPA quantum solver: resolvent norm M\_γ enters cost as M\_γ³ and is uncharacterized for any real molecular system; QROM integral loading treated as O(polylog).

**Note on relationship to OVR-09:** OVR-09 covers the specific case of QRAM assumption laundering in quantum machine learning speedups. OVR-18 generalises this to all oracle-model laundering regardless of whether the oracle is QRAM, block-encoding, sparse access, or state preparation. Both patterns should be checked when a polylog speedup appears.

\---

### OVR-19 · Statistical Infrastructure Failure

**Claim type:** A quantum improvement claim — reduced error rate, enhanced accuracy, demonstrated advantage — is supported only by descriptive statistics without inferential testing, in an experimental context where hardware temporal drift, parameter sensitivity, or small shot counts make statistical artifacts plausible.

**What is usually shown:** A mean value, error bar, or point comparison demonstrating that a quantum-assisted method outperforms a baseline. The improvement is real in the reported run. Inferential statistics (p-values, confidence intervals, effect sizes) are absent. Hardware drift is not controlled or randomized. The parameter choices that govern the quantum technique (scale factors, extrapolation method, circuit folding strategy, calibration snapshot) are not varied.

**What reviewers are tempted to credit:** That the reported improvement is reproducible and distinguishable from statistical noise. Error bars are treated as confidence intervals even though they reflect shot statistics, not run-to-run variation or parameter sensitivity.

**What is actually justified:** That the technique produced the reported metric on that run, under those parameters, at that calibration snapshot. A 2026 systematic review of 81 QEM papers found that 75% use only descriptive statistics and 70% do not control for hardware drift. The same review demonstrated empirically that varying unchosen parameters within literature-standard ranges shifts experimental conclusions from significant improvement to significant degradation in 12% of configurations, and that hardware drift alone induces 3.4× variation in apparent effect size within 48 hours on a single device — reducing 30 nominal repetitions to approximately 1.8 effective independent observations.

**Red flags:** No p-values or confidence intervals; single calibration window; no sensitivity analysis over technique parameters (e.g., ZNE scale factors, QML training seed, circuit depth); consecutive shots treated as independent; "30 repetitions" without autocorrelation check; improvement margin is small relative to known hardware drift range.

**Grading logic:**

* **Accept** if the submission reports inferential statistics (hypothesis tests with multiple-comparison correction), documents hardware drift control or randomized execution order, and demonstrates parameter robustness across at least a small sensitivity grid.
* **Defer** if only descriptive statistics are reported but the effect is large (Cohen's d > 1.0), the methodology is otherwise sound, and drift or parameter sensitivity is acknowledged as a limitation.
* **Reject** if the headline metric depends on a single calibration snapshot with no drift control and no inferential testing, particularly when the claimed improvement margin is small (< 10%) or when the technique has known parameter sensitivity (ZNE, variational circuits, projected kernels).

**Seen in teardowns:** QEM statistical artefacts: 75% of 81 reviewed papers use only descriptive statistics; 12% of parameter configurations flip from significant improvement to significant degradation; drift produces 3.4× variation in ZNE apparent effect size in 48 hours. Hybrid QML timeseries: 62% MAE reduction presented without inferential testing; training done on simulator with parameters transferred to hardware; shot budget not disclosed.

\---

### OVR-20 · Partial Fault-Tolerance Scope Erasure

**Claim type:** A result demonstrated within a bounded, explicitly-stated operating regime — a noise window, a circuit class (e.g., Clifford-only), a code size limit, a postselection constraint, or a depth budget — is cited or summarized as if it applies to the broader fault-tolerant quantum computing context without restriction.

**What is usually shown:** A paper establishes a result that is real and significant within its stated scope: an error-mitigation technique that halves logical error on single-step Clifford circuits; a partial fault-tolerance scheme that outperforms full FT within a bounded noise range; a post-processing improvement that lowers logical error rates without changing the physical error floor. The paper's own scope is documented — sometimes in the title or abstract, sometimes only in a later section. The result migrates into downstream citation stripped of its scope conditions.

**What reviewers are tempted to credit:** That the result establishes a general capability for fault-tolerant or near-fault-tolerant quantum computation, or that it supports timeline claims about the proximity of practical quantum utility.

**What is actually justified:** That the technique works within the demonstrated regime. The gap between the demonstrated regime and the full fault-tolerant context is typically material: Clifford circuits are a strict subset of universal computation; a noise pseudothreshold that works at p ≈ 10⁻³ may not hold at the lower noise rates needed for deep algorithms; a postselection constraint (e.g., pR/pL < 300) limits circuit depth to hundreds of logical operations, not the millions required for commercially relevant algorithms. Each of these scope conditions changes what can be concluded about the technique's ultimate utility.

**Red flags:** "Partial fault-tolerant," "pre-fault-tolerant," or "NISQ-compatible" in title but cited in context implying full FT; Clifford-only result applied to circuits requiring T gates or magic state injection; code distance or noise threshold result cited without noting the regime's upper bound; postselection overhead bound cited as a minor footnote; result cited for timeline or roadmap arguments without noting it is a regime-bounded result.

**Grading logic:**

* **Accept** if the submission correctly identifies the regime boundary and does not extend the result beyond it; timeline or deployment claims are scoped to the demonstrated operating window.
* **Defer** if the scope is stated but not foregrounded, and the submission's central claim remains within the demonstrated regime even if its framing is slightly too broad.
* **Reject** if the submission cites a bounded-regime result as evidence for full fault-tolerant capability, or uses it to support timeline or deployment claims that require the technique to function outside its demonstrated operating window.

**Seen in teardowns:** CliNR/GSE Hamiltonian: 54% error reduction is single-step Clifford-only, non-Clifford extension explicitly deferred; cited without Clifford restriction. Fire-and-ice partial FT: paper explicitly states circuits are not fault-tolerant; pR/pL < 300 caps usable circuit depth; T-gate overhead unresolved; cited as evidence of near-FT progress. Syndrome resampling: lowers logical error rate but does not reduce physical error floor; sample-scaling cost buried; cited as threshold improvement without noting it does not reduce hardware requirements.

\---

### OVR-21 · Hardness-Contingency Erasure in Security Claims

**Claim type:** A post-quantum security claim treats a hardness assumption as settled or unconditional when it is in fact contingent on an engineering bottleneck (such as QRAM state preparation cost) or an unproven complexity-theoretic relationship, and this contingency is not disclosed.

**What is usually shown:** A system or protocol is described as "quantum-secure," "post-quantum ready," or "quantum-resistant" on the basis of a lattice-based or LWE-derived scheme (e.g., ML-KEM, ML-DSA, sympLPN). The claim draws implicitly or explicitly on the assumption that the relevant hardness problem (LWE, LPN, sympLPN) cannot be efficiently solved by a quantum adversary. The paper or submission may be technically accurate about current computational limits while failing to disclose that the security guarantee is contingent on those limits not changing.

**What reviewers are tempted to credit:** That adopting a NIST-standardized or lattice-derived PQC scheme closes the quantum threat surface. "PQC-ready" is treated as a binary property rather than a function of hardness assumptions, engineering trajectories, and key lifecycle decisions.

**What is actually justified:** That the scheme provides computational security against known quantum adversaries under current hardware constraints. LWE-based security is formally contingent on QRAM state preparation remaining costly: the GKZ quantum learning algorithm breaks LWE in polynomial time given quantum samples; the sole obstacle is the physical cost of coherent state preparation over classical data, which is an active engineering research target, not a fixed theoretical barrier. Additionally, no LWE-family problem has been proven to lie outside BQP — the security is empirical confidence accumulated over years of cryptanalysis, not a mathematical lower bound. The gap between "no known efficient quantum algorithm" and "provably quantum-secure" is not acknowledged in most PQC deployment claims.

**Red flags:** "Post-quantum secure" stated without specifying the hardness assumption; QRAM bottleneck cited as a permanent safety floor rather than a shrinking engineering constraint; "NIST standardized" used as equivalent to "proven quantum-resistant"; harvest-now-decrypt-later threat not addressed for long-lived key material; claim does not specify key rotation schedule or cryptographic agility infrastructure; "quantum-native" framing of a sympLPN-based scheme presented as providing formal quantum resistance beyond what the classical sympLPN reduction establishes.

**Grading logic:**

* **Accept** if the claim specifies the underlying hardness assumption explicitly (e.g., LWE at named parameters, sympLPN(n, p) at named values), distinguishes computational confidence from proven lower bound, and addresses key lifecycle risk for long-lived material.
* **Defer** if the scheme is correctly identified but the contingency of QRAM-based attacks is not addressed, or if "PQC-ready" is claimed without specifying agility infrastructure or key rotation policy.
* **Reject** if the claim asserts unconditional quantum resistance for any LWE/LPN/sympLPN-based scheme; if the GKZ or equivalent quantum learning attack is not acknowledged as a theoretical path to breakage; if NIST standardization is cited as a proof of quantum hardness rather than as a best-available standard under current constraints; or if the claim is that adopting PQC migration closes the quantum risk surface rather than narrowing it.

**Seen in teardowns:** PQC/LWE limitations: LWE security is QRAM-contingent, not theoretically unconditional; Shannon/Fano bound proves noise does not erase the secret; GKP structural equivalence to LWE noise is documented. PQC stabilizer decoding: sympLPN is shown to support full Cryptomania but has received a fraction of the cryptanalytic depth of LWE; "quantum-native" framing will be misread as providing formal quantum resistance beyond what the classical reduction establishes; no concrete parameters exist for any sympLPN-based scheme.

**Scope note:** This pattern applies specifically to claims in the security and cryptographic layer of PFT validator architecture, node identity, key management, and communication protocols. It does not apply to pure quantum computing performance claims, which are covered by OVR-01 through OVR-20.

\---

## Decision Heuristic Table Update

The following rows extend the quick-reference table in the Reviewer Usage Guide. Append them to the existing table in the base library.

|If you see this...|Likely pattern|Default posture|
|-|-|-|
|Advantage metric derived at verification boundary, applied to full result|OVR-16|Defer / Reject|
|Hybrid pipeline result without same-pipeline classical-seed ablation|OVR-17|Defer|
|Polylog speedup + oracle/QROM/block-encoding treated as free|OVR-18|Defer / Reject|
|Improvement claim + descriptive statistics only + single calibration window|OVR-19|Defer / Reject|
|Clifford-only or partial-FT result cited for full fault-tolerant context|OVR-20|Defer|
|"Post-quantum secure" without specifying hardness assumption or QRAM risk|OVR-21|Defer / Reject|

\---

## Changelog Update

Append the following row to the Changelog table in the base library:

|Version|Date|Change|
|-|-|-|
|v0.2|2026-05-31|May 2026 sweep. 6 new patterns (OVR-16 through OVR-21) seeded from 11 of 13 May teardowns. Covers: correctness frontier erasure, post-processing substitution, oracle model laundering, statistical infrastructure failure, partial-FT scope erasure, and hardness-contingency erasure (PQC domain). Decision heuristic table extended.|

\---

## 

