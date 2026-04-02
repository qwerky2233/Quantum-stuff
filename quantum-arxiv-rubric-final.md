# Quantum Computing ArXiv Paper Rubric v1.1

### A Community Framework for Evaluating Scalability Realism and Blind Spots

---

> **Version:** 1.1 | **Status:** Living Document | **Last Updated:** April 2026
> **License:** CC BY 4.0 — freely share and adapt with attribution
> **Changelog:** v1.1 adds 6 new scoring dimensions (10–15) and replaces all generic worked examples with scored analyses of three landmark 2024–2025 papers.

---

## Why This Rubric Exists

Quantum computing research is advancing rapidly, but the gap between paper claims and practical scalability is often wide — and not always obvious to non-specialists. This rubric gives anyone a structured, repeatable method for reading ArXiv quantum computing papers critically. It focuses on what papers *omit* as much as what they *claim*, and deliberately avoids requiring deep physics expertise.

This is a **v1 framework**. It is intentionally incomplete. The goal is structural clarity and usability, not exhaustive coverage. Contributions, corrections, and new worked examples are welcome.

---

## How Scoring Works

Each dimension is scored on a **3-tier scale**:

| Score | Label | Meaning |
|-------|-------|---------|
| **3** | Strong | The paper explicitly addresses this dimension with sufficient detail for independent evaluation |
| **2** | Adequate | The paper partially addresses this dimension; gaps exist but the omissions are minor or acknowledged |
| **1** | Weak | The paper largely ignores this dimension, or makes claims that cannot be verified without missing context |

A paper's **Total Score** is the sum across all 15 dimensions (max: 45).

**Interpretation guide:**

- **37–45:** High confidence in claims; minor gaps only
- **27–36:** Moderate confidence; worth reading with caution in flagged areas
- **15–26:** Significant blind spots; claims should not be taken at face value
- **Below 15:** Fundamental transparency issues; treat as preliminary or promotional

> **Note on inapplicable dimensions:** Some dimensions don't apply to every paper type. If a dimension is genuinely out of scope (e.g., "Comparison to Dequantization Results" for a hardware engineering paper), score it **N/A** rather than penalizing the paper. Adjust the maximum score accordingly (subtract 3 per N/A dimension).

---

## The 15 Scoring Dimensions

---

### Dimension 1: Hardware Realism

**Why it matters:** Many quantum algorithms are designed for idealized "all-to-all" qubit connectivity or noise-free gates that don't exist in current hardware. A paper that doesn't specify which hardware platform it targets — or assumes capabilities no real device has — may be solving a problem that doesn't exist in practice.

**What to check:** Abstract, Introduction, Methods/Implementation. Look for: named hardware platforms (IBM Eagle, Google Willow, IonQ Forte), qubit counts, gate fidelities, whether algorithm requirements match real device specs.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper names a specific hardware platform or credibly bounded near-term architecture. Algorithm requirements (qubit count, gate set, coherence times) are explicitly matched to that platform's known specs. Any mismatch is acknowledged. |
| **2 — Adequate** | Paper references a general hardware type (e.g., "superconducting qubits") without naming a device. Requirements are partially specified. Reader can roughly assess plausibility. |
| **1 — Weak** | Paper assumes generic or idealized hardware. No real platform is referenced. Claims like "we assume perfect gates" or "all-to-all connectivity" appear without justification or caveat. |

**Red flags:** "We assume a noise-free quantum computer" / Qubit counts in the thousands with no mention of error correction / No reference to any existing hardware.

---

### Dimension 2: Resource Transparency

**Why it matters:** Quantum algorithms are often presented in terms of logical qubits and ideal gate counts, omitting the massive overhead required for fault-tolerant operation. Without full resource accounting, impressive-sounding results may require hardware decades away.

**What to check:** Methods, Complexity Analysis, Appendices. Look for: logical vs. physical qubit distinction, T-gate counts, circuit depth, total runtime estimates on realistic hardware.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper provides full resource estimates including physical qubit overhead, distinguishes logical from physical qubits, and gives circuit depth and runtime on a named hardware platform. |
| **2 — Adequate** | Paper gives logical qubit and gate counts but does not translate to physical resources. Some scaling analysis is present. |
| **1 — Weak** | Paper states only abstract complexity (e.g., "O(n log n) gates") with no concrete counts. Physical resource requirements are absent or hand-waved. |

**Red flags:** Only Big-O complexity with no concrete numbers / "Physical implementation details are left to future work" / No distinction between logical and physical qubits.

---

### Dimension 3: Connectivity Constraints

**Why it matters:** Most real quantum hardware has limited qubit connectivity. Algorithms requiring arbitrary two-qubit gates must use SWAP chains, which add gate overhead and errors. Papers that ignore this can understate true circuit costs by an order of magnitude.

**What to check:** Circuit diagrams, Methods, Hardware sections. Look for: assumed connectivity graph, SWAP overhead analysis, compilation benchmarks on restricted topologies.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper explicitly accounts for the target hardware's connectivity graph. SWAP overhead is quantified. Results are compiled and benchmarked on a realistic topology (e.g., heavy-hex, linear chain). |
| **2 — Adequate** | Paper acknowledges connectivity limitations and provides a qualitative discussion. SWAP overhead is estimated but not precisely quantified. |
| **1 — Weak** | Paper assumes all-to-all connectivity or ignores the issue entirely. No mention of compilation overhead or topology-specific costs. |

**Red flags:** "We assume all-to-all qubit connectivity" / No mention of hardware topology / Circuit diagrams showing arbitrary long-range connections.

---

### Dimension 4: Error Correction Overhead

**Why it matters:** Fault-tolerant quantum computing requires encoding each logical qubit in hundreds to thousands of physical qubits. Claims of quantum advantage without accounting for this overhead often describe advantages that vanish under realistic error correction costs.

**What to check:** Methods, Resource Analysis, Discussion. Look for: error correction scheme (surface code, color code), code distance, physical qubit overhead multiplier, break-even error rates.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper specifies the error correction scheme, code distance, and physical-to-logical qubit ratio. Claims are made in the context of fault-tolerant operation. Break-even error rates are identified. |
| **2 — Adequate** | Paper acknowledges that error correction will be required and provides rough estimates of overhead. Some error rate assumptions are stated. |
| **1 — Weak** | Paper operates entirely in the logical qubit abstraction without acknowledging physical error correction costs. Noise is ignored or addressed only superficially. |

**Red flags:** Results claimed for "near-term" devices that would actually require fault tolerance / No mention of physical error rates / Threshold error rates assumed far below current hardware capabilities.

---

### Dimension 5: Experimental Validation

**Why it matters:** Quantum computing claims span a spectrum from purely theoretical to experimentally demonstrated. Without knowing where a paper sits, it is easy to mistake a simulation for a hardware result, or a small-scale toy experiment for evidence of scalability.

**What to check:** Abstract, Results, Experiments. Look for: whether results are simulated or run on hardware, system size, number of qubits actually used, code/data availability.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Results are demonstrated on real quantum hardware at a scale relevant to the claimed application. Code and/or data are publicly available. Experiments are reproducible. |
| **2 — Adequate** | Results are demonstrated either on hardware at small scale or on a high-quality quantum simulator. Limitations are acknowledged. |
| **1 — Weak** | Results are purely theoretical or based on classical simulation of small systems. Extrapolation to practical scale is asserted but not evidenced. |

**Red flags:** "We simulate a 10-qubit system" followed by claims about 1000-qubit performance / No code or data availability statement / Noiseless simulation presented alongside hardware claims.

---

### Dimension 6: Classical Baseline Comparison

**Why it matters:** Quantum advantage is only meaningful relative to the best known classical algorithm. Many papers compare against naive classical approaches rather than state-of-the-art methods. A claimed speedup over an outdated benchmark may not represent a genuine advance.

**What to check:** Introduction (related work), Experiments, Discussion. Look for: named classical algorithms, whether the baseline is state-of-the-art, hardware used for the classical comparison, whether the comparison is fair.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper compares against a named, state-of-the-art classical algorithm on the same problem and infrastructure. Classical runtime and resources are explicitly stated. The comparison is fair and limitations are acknowledged. |
| **2 — Adequate** | A classical comparison is made, but the baseline may not be fully state-of-the-art, or comparison conditions differ in ways that are partially acknowledged. |
| **1 — Weak** | No classical comparison, or comparison is against a clearly suboptimal classical method. Claims of quantum advantage are not grounded in a rigorous baseline. |

**Red flags:** Comparison against brute-force search when polynomial-time classical algorithms exist / Classical comparison run on a laptop while quantum results are for full hardware / "Quantum advantage" claimed for a problem that is classically easy.

---

### Dimension 7: Scalability Argument Quality

**Why it matters:** A result on 10 qubits is not automatic evidence for what happens at 1000 qubits. Scaling arguments need to be explicit, technically grounded, and honest about where they break down.

**What to check:** Theory sections, Discussion, Conclusion. Look for: whether scaling trends are extrapolated from data or derived theoretically, what assumptions underlie the scaling claim, where the authors expect their approach to fail.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Scaling arguments are derived analytically or from multiple data points across system sizes. Limitations and failure modes of the scaling argument are explicitly identified. |
| **2 — Adequate** | Some scaling data or argument is present. Extrapolation is made with limited supporting evidence. Limitations are partially acknowledged. |
| **1 — Weak** | Scaling is asserted or extrapolated from a single data point. No failure modes are identified. Claims are made for practical scales with no path from the demonstrated result. |

**Red flags:** "Our algorithm scales as O(n²)" with results only shown for n=4 / No discussion of where noise will dominate at scale / Scaling claimed to hold "in principle" without empirical support.

---

### Dimension 8: Claim-Evidence Alignment

**Why it matters:** Abstract and conclusion claims in quantum papers frequently outpace what the body actually demonstrates. This dimension specifically checks whether headline claims are proportionate to the evidence.

**What to check:** Compare the Abstract and Conclusion to the Methods and Results. Look for: hedging language in the body that disappears in the abstract; scope creep between what was demonstrated and what is claimed.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Claims in the abstract and conclusion are precisely matched by the evidence in the body. Limitations are stated in prominent sections, not buried in appendices. |
| **2 — Adequate** | Minor overstatement in the abstract relative to the body. Limitations are present but understated. |
| **1 — Weak** | Significant gap between abstract/conclusion claims and what the paper actually demonstrates. Key caveats are absent from headline sections. |

**Red flags:** Abstract says "demonstrates quantum advantage," body says "suggests potential for advantage" / Conclusions extrapolate to real-world applications not studied in the paper / "Future work" section contains the actual requirements for the main claim to hold.

---

### Dimension 9: Reproducibility and Openness

**Why it matters:** Science depends on independent verification. Papers that provide no code, no data, and insufficient methodological detail cannot be checked or built upon.

**What to check:** Data/Code Availability Statements, Methods, Appendices. Look for: GitHub links, dataset availability, sufficient circuit detail to reproduce results.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Code and data are publicly available. Methods contain sufficient detail for independent reproduction. |
| **2 — Adequate** | Code or data is available but not both. Reproduction would require significant effort but is possible in principle. |
| **1 — Weak** | No code or data. Methods lack sufficient detail for reproduction. Contact-the-authors is the only option. |

---

### Dimension 10: Noise Model Realism

**Why it matters:** A simulation is only as good as its noise model. Papers that use idealized depolarizing noise at uniform rates will systematically overestimate algorithm performance compared to what a real device — with correlated errors, leakage, crosstalk, and spatially varying error rates — would produce. The gap between idealized and physically motivated noise models can be the difference between claimed advantage and no advantage at all.

**What to check:** Methods, Simulation Details, Supplementary Material. Look for: whether the noise model is a named standard (e.g., "circuit-level depolarizing noise at 0.1%"), whether error rates are taken from empirical calibration data, whether correlated or non-Markovian errors are discussed, and whether the choice of noise model is justified.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper uses a physically motivated noise model with empirically measured error rates from a named device. Correlated errors, leakage, and readout errors are addressed or their omission is explicitly justified. Results are tested under multiple noise regimes. |
| **2 — Adequate** | Paper uses a standard noise model (e.g., uniform depolarizing noise) with a stated error rate. The model is acknowledged as a simplification. Some sensitivity analysis is present. |
| **1 — Weak** | Paper uses noiseless simulation, or applies a noise model without specifying its parameters or physical motivation. No sensitivity analysis. Results are not tested under varying noise levels. |

**Red flags:** "We assume uniform depolarizing noise" with no citation for the assumed rate / No noise model at all in a simulation paper / Claims that are sensitive to error rate but no sensitivity analysis / Coherent errors (which are often harder to correct than incoherent ones) completely ignored.

---

### Dimension 11: Algorithmic Depth vs. Hardware Coherence Time

**Why it matters:** Every quantum gate takes time, and qubits lose their quantum information (decohere) over time. If a circuit is too deep to complete before the qubits decohere, the result is garbage — regardless of how good the algorithm is on paper. This is one of the most commonly omitted checks in near-term algorithm papers, and one of the easiest for non-specialists to test by looking up coherence times for the named hardware.

**What to check:** Methods, Circuit Implementation, Hardware Specs. Look for: total circuit execution time (gate count × gate time), qubit T1 and T2 times for the target platform, and whether the circuit fits within the coherence window. This can be cross-referenced against publicly available hardware specs (e.g., IBM Quantum calibration pages, Google Willow spec sheet).

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper explicitly computes or bounds total circuit execution time and confirms it fits within the coherence time of the target hardware. Gate times and coherence times are stated. |
| **2 — Adequate** | Circuit depth is stated but execution time vs. coherence time is not explicitly compared. Some discussion of decoherence limitations is present. |
| **1 — Weak** | Circuit depth or gate count is given with no reference to coherence time. No discussion of whether the circuit can physically complete before decoherence. |

**Red flags:** Deep circuits claimed to run on NISQ hardware with no mention of coherence time / Gate counts in the thousands on hardware with microsecond coherence / "We neglect decoherence in our analysis" with no justification / Algorithm shown only for system sizes where it would fit, without discussing what happens at practical scales.

---

### Dimension 12: Compilation and Transpilation Transparency

**Why it matters:** There are two very different versions of a quantum circuit: the idealized "logical" circuit the algorithm designer writes, and the compiled "physical" circuit that actually runs on hardware after routing, decomposition, and optimization. Gate counts and circuit depths can increase by 3–10× during compilation. A paper reporting pre-compilation results for a real hardware claim is measuring something that doesn't correspond to actual hardware execution.

**What to check:** Experiments, Methods, any "Implementation" subsection. Look for: whether reported gate counts and circuit depths are pre- or post-compilation, which compiler was used (e.g., Qiskit, Pytket, tket), whether compilation was done for the target hardware's native gate set and topology.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Results are reported post-compilation on the target hardware's native gate set and topology. The compiler, optimization level, and any manual optimizations are specified. Pre- and post-compilation metrics are both provided for comparison. |
| **2 — Adequate** | Paper clarifies whether results are pre- or post-compilation, and uses a real compiler, but does not fully specify compiler settings or show the compilation overhead. |
| **1 — Weak** | Results are reported without specifying whether they are pre- or post-compilation. No compiler is mentioned. Readers cannot determine what the actual hardware circuit would look like. |

**Red flags:** "We ran this circuit on IBM Quantum" without any mention of transpilation / Gate counts given that are clearly pre-compilation (e.g., arbitrary two-qubit gates between non-adjacent qubits) / No mention of which compiler or optimization level was used.

---

### Dimension 13: Benchmark Problem Representativeness

**Why it matters:** Quantum algorithm demonstrations often use problems that are specifically chosen — sometimes unconsciously — because they are easy for quantum computers and hard for classical methods. A benchmark problem that doesn't faithfully represent the target application can make a quantum approach look better than it really is. This is particularly prevalent in quantum optimization and quantum machine learning papers.

**What to check:** Problem Definition, Related Work, Discussion/Limitations. Look for: how the benchmark problem was selected, whether it is a recognized standard benchmark, whether it has known classical solutions, and whether the problem size is representative of real-world instances.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Benchmark problem is a recognized standard (e.g., MaxCut on random 3-regular graphs, molecular Hamiltonians of industrial interest). Problem selection is justified. The paper addresses whether the problem is a fair proxy for the stated application. |
| **2 — Adequate** | Benchmark problem is reasonable but not a widely recognized standard. Some justification for problem selection is given. Limitations of the benchmark are partially acknowledged. |
| **1 — Weak** | Benchmark problem appears purpose-built to favor the quantum approach. No justification for problem selection. No discussion of whether the benchmark is representative of real-world instances. |

**Red flags:** "We study a specially structured problem that..." / Benchmark is a toy version of the stated application with different difficulty characteristics / Problem was tuned so that the quantum approach outperforms classical methods / Claims generalize to "all optimization problems" from a single benchmark.

---

### Dimension 14: Comparison to Dequantization Results

**Why it matters:** For quantum linear algebra and quantum machine learning papers specifically, there exists a body of "dequantization" results — classical algorithms that achieve comparable performance to quantum algorithms, often by exploiting sampling techniques. Many quantum speedup claims in these areas have been partially or fully matched by classical algorithms developed after the quantum paper was published. Papers that don't address known dequantization results for related problems may be claiming advantages that no longer exist or are much smaller than stated.

**Why it's a conditional dimension:** This dimension applies primarily to quantum linear algebra (quantum PCA, quantum recommendation systems, quantum linear systems), quantum machine learning (quantum kernel methods, quantum neural networks), and quantum sampling algorithms. For hardware papers, error correction papers, and quantum simulation papers, score this **N/A**.

**What to check:** Related Work, Discussion. Look for: citations to dequantization results, discussion of whether the problem's data access model would allow dequantization, and whether the claimed speedup survives under the same input model as the classical algorithm.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper explicitly addresses known dequantization results for related problems. The paper's data access model and problem constraints are carefully specified in a way that rules out known dequantization. |
| **2 — Adequate** | Paper mentions that dequantization is a concern for related algorithms. The paper's claimed speedup is in a regime or for a problem structure not yet dequantized, but this is not rigorously argued. |
| **1 — Weak** | Paper claims quantum speedup in a domain where strong dequantization results exist, without addressing them. The input model that the speedup assumes is not specified. |
| **N/A** | Not applicable (hardware, error correction, quantum simulation, or other non-linear-algebra context). |

**Red flags:** Quantum recommendation system or quantum PCA paper with no citation to Tang (2018) or its successors / Claims of exponential speedup over classical in a domain where polynomial classical algorithms are known / Speedup depends on quantum RAM (QRAM) access without acknowledging the physical cost of QRAM.

---

### Dimension 15: Commercial vs. Academic Hardware Distinction

**Why it matters:** Cloud quantum computers (IBM Quantum, Amazon Braket, IonQ Cloud) and custom research hardware represent dramatically different experimental contexts. Cloud hardware is subject to job queuing, variable calibration state, limited circuit depth and shot counts, and middleware noise from compilation layers. Custom research hardware often has dedicated access, optimized calibration, and hand-tuned circuits. A result on custom hardware may not transfer to cloud hardware, and the distinction matters for assessing what the community can actually reproduce or build on.

**What to check:** Experiments, Hardware/Methods, Acknowledgments. Look for: whether hardware is cloud-accessed or custom/dedicated, how circuits were submitted (which cloud API version, which backend), calibration state and date of experiments, and whether results would be reproducible by other researchers with cloud access.

| Score | Criteria |
|-------|----------|
| **3 — Strong** | Paper clearly identifies whether hardware is cloud or custom. For cloud hardware, calibration data and backend access dates are provided. For custom hardware, access constraints and differences from cloud hardware are discussed. Reproducibility by the community is explicitly addressed. |
| **2 — Adequate** | Paper identifies the hardware platform and access method (cloud or custom) but doesn't fully specify calibration state or reproducibility constraints. |
| **1 — Weak** | Paper claims results on quantum hardware without distinguishing cloud vs. custom access, or without specifying which backend, calibration, or access level was used. Results are not reproducible without direct institutional collaboration. |

**Red flags:** "We ran experiments on IBM Quantum" without specifying which backend, which calibration, and when / Results on dedicated, hand-tuned custom hardware presented as generally representative of the hardware class / No discussion of how community members could reproduce results.

---

## Scoring Summary Sheet

Copy and use this template when evaluating a paper:

```
Paper Title:
ArXiv ID:
Evaluator:
Date:

| #  | Dimension                                    | Score (1-3 or N/A) | Key Notes |
|----|----------------------------------------------|--------------------|-----------|
|  1 | Hardware Realism                             |                    |           |
|  2 | Resource Transparency                        |                    |           |
|  3 | Connectivity Constraints                     |                    |           |
|  4 | Error Correction Overhead                    |                    |           |
|  5 | Experimental Validation                      |                    |           |
|  6 | Classical Baseline Comparison                |                    |           |
|  7 | Scalability Argument Quality                 |                    |           |
|  8 | Claim-Evidence Alignment                     |                    |           |
|  9 | Reproducibility & Openness                   |                    |           |
| 10 | Noise Model Realism                          |                    |           |
| 11 | Algorithmic Depth vs. Coherence Time         |                    |           |
| 12 | Compilation & Transpilation Transparency     |                    |           |
| 13 | Benchmark Problem Representativeness         |                    |           |
| 14 | Comparison to Dequantization Results         |                    |           |
| 15 | Commercial vs. Academic Hardware Distinction |                    |           |
|----|----------------------------------------------|--------------------|-----------|
|    | TOTAL                                        |             / 45   |           |

Overall Assessment:
Primary Blind Spot(s):
Recommended Use: [ ] Cite as evidence  [ ] Reference with caution  [ ] Do not cite without caveats
```

---

## Worked Examples: Three Landmark 2024–2025 Papers

> These examples apply the rubric to real, high-profile papers in good faith for educational purposes. Scores reflect rubric criteria applied to publicly available information about each paper — not a judgment of the underlying research quality or the researchers themselves. Community members are encouraged to challenge these scorings with evidence.

---

### Example 1: Google Willow — Quantum Error Correction Below the Surface Code Threshold

**Citation:** Google Quantum AI and Collaborators. *Quantum error correction below the surface code threshold.* Nature 638, 920–926 (December 2024). ArXiv preprint: arXiv:2408.13687 (August 2024).

**Why this paper was a big deal:** For 30 years, achieving "below-threshold" error correction — where adding more qubits makes errors go *down* rather than up — was the key unsolved milestone for scalable quantum computing. Willow was the first demonstration of exponential error suppression as logical qubit size increased (from 3×3 to 5×5 to 7×7 physical qubit grids). The paper also reported a random circuit sampling benchmark that Google claims would take 10 septillion years on a classical supercomputer.

**Paper profile:** Experimental hardware paper reporting surface code error correction on a 105-qubit superconducting processor. Two main results: (1) below-threshold QEC, and (2) a random circuit sampling (RCS) benchmark. Custom hardware at Google's Santa Barbara facility; not a cloud device.

| # | Dimension | Score | Notes |
|---|-----------|-------|-------|
| 1 | Hardware Realism | 3 | Named hardware (Willow, 105-qubit), specific gate fidelities, T1/T2 coherence times, grid topology explicitly described |
| 2 | Resource Transparency | 3 | Physical qubit counts, logical qubit encoding ratios, code distance, and syndrome cycle overhead all explicitly stated. Path to 1000 physical qubits per logical qubit acknowledged |
| 3 | Connectivity Constraints | 3 | Square grid topology with ~3.5 average connectivity stated. Surface code is designed around this topology; paper accounts for it natively |
| 4 | Error Correction Overhead | 3 | Surface code scheme, code distances 3/5/7, physical-to-logical ratios, and Λ (error suppression factor) of 2.14 for d=7 all reported. Break-even error rates identified |
| 5 | Experimental Validation | 3 | Real hardware results on custom Willow processor. Full calibration data and QEC data corpus released publicly. Code available |
| 6 | Classical Baseline | 2 | RCS classical comparison uses the best-known tensor network contraction algorithm, which is state-of-the-art. However, the benchmark problem (RCS) is not a useful computational task — the comparison is against a hardness argument, not a real application. Acknowledged in the paper |
| 7 | Scalability Argument | 2 | Below-threshold result provides a strong scaling argument for logical error suppression. Paper honestly notes that achieving one error per ten million steps requires ~1000 physical qubits per logical qubit — a major remaining gap |
| 8 | Claim-Evidence Alignment | 3 | Claims are milestone-framed and match the demonstrated results. "Below threshold" is precisely defined and demonstrated. The 10-septillion-year claim is clearly labeled as benchmark-specific |
| 9 | Reproducibility & Openness | 3 | Full dataset and stim circuits released publicly. Third-party researchers have already published independent analyses of the data corpus |
| 10 | Noise Model Realism | 3 | Hardware-characterized noise model built from empirical calibration data. Multiple decoder types tested. Correlated errors across the chip (microwave crosstalk) are identified and discussed as a scalability concern |
| 11 | Depth vs. Coherence Time | 3 | T1 improved 5× to ~100 microseconds. Real-time decoder latency of 63 μs is explicitly stated and compared to syndrome cycle time. Circuit fits within coherence window |
| 12 | Compilation Transparency | 3 | Circuits are native to the hardware's CZ gate set and grid topology. No compilation ambiguity — the circuits are designed for this specific device |
| 13 | Benchmark Representativeness | 2 | The QEC demonstration uses the surface code, a standard and well-studied benchmark. The RCS benchmark is a genuine hardness demonstration but not a practically useful task; this is acknowledged. Honest about what it does and does not show |
| 14 | Dequantization Comparison | N/A | Hardware/error correction paper; dequantization is not applicable |
| 15 | Commercial vs. Academic Hardware | 3 | Custom dedicated hardware at Google's facility. Paper explicitly distinguishes this from cloud access. Data is available for community replication on simulators; hardware replication requires similar custom facilities |
| **TOTAL** | | **38 / 42** | |

**Summary judgment:** This is one of the highest-scoring papers this rubric is likely to encounter. The Willow paper sets the standard for experimental transparency in quantum hardware. The only notable gaps are the scalability caveat (honestly acknowledged — achieving useful fault tolerance still requires a 10–100× improvement in scale) and the RCS benchmark's lack of practical utility. The paper is a genuine milestone demonstration, not a quantum advantage claim for a useful task, and it is careful to say so.

**Primary blind spot:** The classical baseline score reflects a genuine limitation of the RCS benchmark itself rather than a flaw in how the paper handles it. The paper cannot claim a useful quantum speedup because RCS is not a useful problem. This is the right honest framing, and it scores accordingly.

**Recommended use:** Cite as evidence of below-threshold error correction. Do not cite as evidence of practical quantum advantage over classical computers for any application.

---

### Example 2: IBM Bivariate Bicycle Codes — High-Threshold and Low-Overhead Fault-Tolerant Quantum Memory

**Citation:** Bravyi, S., Cross, A. W., Gambetta, J. M., Maslov, D., Rall, P., & Yoder, T. J. *High-threshold and low-overhead fault-tolerant quantum memory.* Nature 627, 778–782 (March 2024). ArXiv: arXiv:2308.07915.

**Why this paper was a big deal:** The surface code has dominated quantum error correction research for two decades, but it is extremely qubit-inefficient — requiring hundreds to thousands of physical qubits per logical qubit. IBM's Bivariate Bicycle (BB) codes are a class of quantum LDPC codes that achieve comparable error thresholds (~0.7%) while encoding logical qubits using roughly 10× fewer physical qubits. The flagship example: 12 logical qubits encoded in 144 data qubits (288 physical qubits total), versus 1,452–2,028 qubits for equivalent surface codes. This paper directly underpins IBM's Starling roadmap for fault-tolerant quantum computing by 2029.

**Paper profile:** Primarily a theoretical/simulation paper introducing a new error correction code family. Results are obtained by numerical simulation (not hardware); the paper explicitly positions itself as a quantum memory (storage) demonstration, not a full computation demonstration.

| # | Dimension | Score | Notes |
|---|-----------|-------|-------|
| 1 | Hardware Realism | 2 | Paper targets superconducting qubits and motivates the code's degree-6 Tanner graph as suited to superconducting coupler architectures. Does not name a specific IBM processor or demonstrate on hardware; this is honest framing for a theoretical paper |
| 2 | Resource Transparency | 3 | Explicit resource counts: [[144, 12, 12]] code requires 288 physical qubits total, depth-7 syndrome measurement circuit. Encoding rate and code distance computed. Comparison table against surface code provided |
| 3 | Connectivity Constraints | 3 | Tanner graph connectivity is the central object of the paper. The degree-6 graph with thickness 2 is specifically motivated by 2D superconducting architectures. Long-range connections acknowledged as a key implementation challenge |
| 4 | Error Correction Overhead | 3 | Error threshold of 0.7% stated. Pseudo-threshold per code size computed. Physical-to-logical qubit ratios explicitly compared to surface code. Logical error rate curves shown numerically |
| 5 | Experimental Validation | 2 | No hardware demonstration in this paper (honest framing — it's a "quantum memory" theory paper). Simulation code is publicly available on GitHub. Subsequent work by Wang et al. (2026) demonstrated a small BB code on hardware |
| 6 | Classical Baseline | 3 | Classical comparison is appropriate: paper compares BB codes to the surface code (the established state-of-the-art), not to an arbitrary classical baseline. The specific advantage claimed (10× qubit overhead reduction) is clearly stated and bounded |
| 7 | Scalability Argument | 3 | Numerical data shown across multiple code sizes. Threshold behavior extrapolated from data with error bars. The paper is honest that this is a *memory* paper — scaling arguments for full computation are left to future work |
| 8 | Claim-Evidence Alignment | 3 | Claims are precisely scoped: "fault-tolerant *memory*" not "fault-tolerant *computation.*" The paper explicitly states this distinction in the opening and conclusion. Quantum advantage over classical is not claimed |
| 9 | Reproducibility & Openness | 3 | Simulation code publicly available on GitHub (sbravyi/BivariateBicycleCodes). Code parameters tabulated. Independent teams have reproduced and extended the results |
| 10 | Noise Model Realism | 2 | Uses standard circuit-level depolarizing noise model, which is the field standard for error correction simulation. Not calibrated to empirical hardware data — uniform error rate assumed. The authors acknowledge this is a simplification |
| 11 | Depth vs. Coherence Time | 2 | Depth-7 syndrome measurement circuit is stated. No comparison to hardware coherence times because no hardware target is named. Paper leaves this analysis to implementation papers |
| 12 | Compilation Transparency | 2 | Circuit structure (nearest-neighbor CNOT gates, degree-6 connectivity) is described. Actual compilation to any specific hardware gate set and topology is left to subsequent work. Paper is honest that non-local connections are a hardware challenge |
| 13 | Benchmark Representativeness | 3 | Error correction code benchmarks are well-established (pseudo-threshold, logical error rate vs. physical error rate curves). Surface code is the appropriate comparison baseline. The 12-logical-qubit example is not cherry-picked |
| 14 | Dequantization Comparison | N/A | Error correction paper; dequantization not applicable |
| 15 | Commercial vs. Academic Hardware | N/A | No hardware experiments in this paper; the distinction does not apply |
| **TOTAL** | | **35 / 39** | |

**Summary judgment:** This is a rigorous, well-scoped theoretical paper that sets a new benchmark for error correction efficiency. Its key strength is the precise scoping of its claims — it is a memory paper, not a computation paper, and it says so prominently. The main gaps are what you would expect from a simulation paper that does not yet have a hardware demonstration at the relevant scale. Critically, the non-local connectivity required by BB codes is acknowledged as a challenge, but no concrete compilation or transpilation analysis for real hardware is provided; that work was left to follow-on papers and IBM's subsequent roadmap documents.

**Primary blind spots:** Noise model realism (idealized uniform noise vs. calibrated hardware noise) and hardware compilation transparency are the honest limitations. The paper is transparent about both. The large-scale hardware implementation remains undemonstrated — though this is an active area with experimental progress following publication.

**Recommended use:** Cite as evidence of a theoretical advance in error correction efficiency. Do not cite as evidence that BB codes work on real hardware at the scale claimed — that demonstration is still in progress as of April 2026.

---

### Example 3: Microsoft Majorana 1 — Interferometric Single-Shot Parity Measurement and Topological Qubit Roadmap

**Citation (Nature paper):** Microsoft Azure Quantum. *Interferometric single-shot parity measurement in InAs-Al hybrid devices.* Nature 638, 651–655 (February 2025).
**Citation (roadmap paper):** Aasen, D. et al. (181 authors). *Roadmap to fault tolerant quantum computation using topological qubit arrays.* arXiv:2502.12252 (February 2025, updated July 2025).

**Why this paper was a big deal:** Microsoft's entire quantum computing strategy is built on a different physical architecture from IBM (superconducting) and Google (superconducting) or IonQ (trapped ions). They are betting on *topological qubits* based on Majorana zero modes — exotic quantum states that are theoretically resistant to certain types of noise at the hardware level. If topological qubits work as theorized, they could drastically reduce the error correction overhead needed for fault-tolerant computing. The February 2025 announcement claimed the first experimental demonstration of this architecture, accompanied by the Nature paper and the arXiv roadmap. However, the announcement was immediately controversial among physicists, with multiple experts arguing the Nature paper does not actually prove what was claimed.

**Paper profile:** Two linked documents — a short experimental Nature paper demonstrating parity measurements on a prototype device, and a lengthy roadmap paper describing a four-generation device plan. We score both together, with the understanding that the headline claims came from the press release and blog post, not exclusively from the papers themselves.

| # | Dimension | Score | Notes |
|---|-----------|-------|-------|
| 1 | Hardware Realism | 2 | The tetron architecture using InAs-Al heterostructures is specifically described. The roadmap paper provides a detailed device schematic. However, the Nature paper demonstrates only a *single-qubit prototype* — the hardware exists at lab scale, and significant material science challenges are acknowledged in the roadmap |
| 2 | Resource Transparency | 2 | Roadmap paper provides a four-generation device plan (1 qubit → 2 qubits → 8 qubits → 27×13 array). Resource requirements for each stage are described. Full resource analysis for fault-tolerant computation at scale is deferred to future work |
| 3 | Connectivity Constraints | 3 | Measurement-based qubit operations via interferometric loops are the core architecture. The roadmap discusses connectivity extensively. The tetron layout and lattice surgery operations directly address connectivity constraints of this qubit type |
| 4 | Error Correction Overhead | 2 | Roadmap discusses the path to error correction through a topological surface code analog. The topological protection is claimed to reduce physical error rates inherently, potentially reducing correction overhead. However, no end-to-end error correction demonstration exists at time of publication |
| 5 | Experimental Validation | 2 | Nature paper demonstrates single-qubit parity measurements with Z-type measurement error rates of ~0.5% and X-type at ~16% — a 30× asymmetry that represents a significant unresolved challenge. Only one prototype device demonstrated. No multi-qubit operation shown in the Nature paper itself |
| 6 | Classical Baseline | N/A | Hardware/architecture paper; classical baseline comparison is not applicable |
| 7 | Scalability Argument | 1 | The roadmap describes a four-generation path plausibly. However, as of publication, only the single-qubit level is experimentally demonstrated. The 16% X-type measurement error rate is far above what would be needed for the subsequent generations. The gap between demonstration and the claimed roadmap is very large |
| 8 | Claim-Evidence Alignment | 1 | This is the paper's most significant rubric failure. Microsoft's press release announced "the world's first quantum processor powered by topological qubits" and "peer-reviewed confirmation" of Majorana zero modes. Multiple prominent physicists immediately contested these characterizations, arguing the Nature paper demonstrates parity measurements consistent with topological behavior but does not prove topological protection or the existence of Majorana zero modes. Microsoft's own spokesperson acknowledged the Nature paper was submitted a year before the roadmap claims existed. The headline claims substantially outpace what either paper demonstrates |
| 9 | Reproducibility & Openness | 2 | Nature paper is published with experimental data. Roadmap paper is detailed but describes future work. Station Q conference demonstrations were private — not publicly reproducible. The materials stack required is highly specialized and not available to outside researchers |
| 10 | Noise Model Realism | 2 | The topological protection argument is that the architecture is inherently noise-resistant. This is theoretically well-motivated but not yet empirically demonstrated at the relevant scale. The 16% X-type measurement error rate in the actual device is not modeled against a specific physical noise source in the Nature paper |
| 11 | Depth vs. Coherence Time | 2 | Z-type parity lifetime of ~12.4 milliseconds is long; X-type lifetime of ~14.5 microseconds is shorter. Roadmap acknowledges the asymmetry as a challenge. Circuit depths for the subsequent roadmap stages are not computed against these measured coherence times |
| 12 | Compilation Transparency | 2 | Topological qubit operations are fundamentally measurement-based rather than gate-based, making traditional compilation analysis inapplicable. The roadmap describes the operation set in detail. However, the resources for implementing these measurements in terms of conventional classical control hardware are not fully analyzed |
| 13 | Benchmark Representativeness | 2 | Parity measurement fidelity is the right benchmark for this stage of development. The 0.5% Z-type error rate is promising. The 16% X-type error rate is the key challenge and is clearly reported |
| 14 | Dequantization Comparison | N/A | Hardware architecture paper |
| 15 | Commercial vs. Academic Hardware | 2 | Custom research hardware at Microsoft Station Q (UCSB). Not cloud-accessible. Paper acknowledges this is research-stage. However, the press release framing suggested commercial relevance ("years, not decades") that the papers themselves do not support |
| **TOTAL** | | **26 / 36** | |

**Summary judgment:** The Majorana 1 announcement is a case study in the gap between scientific papers and their public presentation. The underlying research — demonstrating parity measurements in a prototype topological device — is genuinely interesting and represents years of materials science work. Scored on its own terms as a prototype device paper, it is a reasonable contribution. Scored against the claims made in the press release and blog post (which is how it entered the public discourse), it is significantly overclaimed. The 16% X-type measurement error rate, the absence of multi-qubit demonstrations, and the community skepticism about whether topological protection is actually confirmed are all substantial unresolved issues. The roadmap paper is ambitious and detailed, but a roadmap is not a demonstration.

**Primary blind spots:** Claim-evidence alignment is the critical failure. The scalability argument (Dimension 7) fails not because the roadmap is implausible in principle, but because the current demonstrated hardware is extremely far from the roadmap's requirements. This is an architecture to watch, not one to cite as validated.

**Recommended use:** Cite as evidence of early-stage experimental progress toward topological qubit architectures. Do not cite as evidence that topological qubits work, that Majorana zero modes have been confirmed, or that Microsoft has demonstrated fault-tolerant operation of any kind. Monitor follow-up papers for the predicted multi-qubit demonstrations.

---

## Usage Guide for Non-Specialists

### Step 1: Orient Yourself Before Scoring

Before applying the rubric, read the abstract and conclusion of the paper first — then read the introduction last. This order helps you notice the gap between headline claims and actual results before the paper has a chance to build up your confidence in those claims. Make a quick note of what the paper says it *proves*, and what the paper says it *demonstrates*. These are often different things.

Most ArXiv quantum papers follow a standard structure: Abstract → Introduction → Methods/Circuit Design → Results/Experiments → Discussion → Conclusion → Appendices. The most important sections for this rubric are Methods and Results, which often contain the caveats that the abstract omits.

### Step 2: Use the Sections Efficiently

You don't need to read a quantum computing paper from start to finish to apply this rubric. Each dimension has natural homes in the paper:

- **Hardware Realism, Connectivity, Coherence Time (D1, D3, D11):** Check the Methods section and any "Implementation Details" or "Circuit Compilation" subsections. For coherence time checking, look up the named hardware's public specs (IBM Quantum calibration, Google Willow spec sheet) and compare to the stated circuit depth.
- **Resource Transparency, Error Correction (D2, D4):** Look for a "Resource Analysis," "Fault-Tolerance Analysis," or Appendix. If none exists, that is itself important information.
- **Experimental Validation, Commercial vs. Academic Hardware (D5, D15):** The Results section and any "Experiments" subsection will tell you whether results are simulated or hardware-based. Check the Acknowledgments for which hardware facility was used. The Data/Code Availability statement tells you about reproducibility.
- **Classical Baseline, Dequantization (D6, D14):** Check the Introduction (related work) and any comparison tables in Results.
- **Noise Model, Compilation Transparency (D10, D12):** Look for "Simulation Details" or "Implementation" subsections. Check whether gate counts are described as pre- or post-compilation.
- **Benchmark Representativeness (D13):** Check the Discussion and Related Work for justification of the benchmark choice.
- **Claim-Evidence Alignment (D8):** Compare the Abstract and Conclusion directly to the Results section. Read the press release or blog post alongside the paper if one exists.

### Step 3: Score What the Paper Says, Not What You Think It Should Say

This rubric scores *transparency and completeness*, not correctness. A paper that clearly states "we assume uniform depolarizing noise at 0.1%, which is a simplification that may overestimate performance" scores higher than a paper that uses the same assumption without acknowledgment. Honesty about limitations is the primary signal this rubric rewards.

If a paper omits a dimension entirely with no acknowledgment, score it a 1. If a dimension is genuinely not applicable (e.g., "Comparison to Dequantization Results" for a hardware paper), note this in your scoresheet and score it N/A rather than penalizing the paper for work it wasn't trying to do.

### Step 4: Focus on the Highest-Signal Dimensions

The dimensions that most reliably separate credible papers from overclaimed ones in current quantum computing research:

**For NISQ and near-term algorithm papers:** Focus on D10 (Noise Model Realism), D11 (Depth vs. Coherence Time), D12 (Compilation Transparency), and D6 (Classical Baseline). These four dimensions catch most of the overclaiming in this category.

**For quantum advantage and speedup claims:** Focus on D6 (Classical Baseline), D13 (Benchmark Representativeness), D14 (Dequantization), and D8 (Claim-Evidence Alignment). Quantum machine learning papers in particular deserve intense scrutiny on D13 and D14.

**For hardware and error correction papers:** Focus on D5 (Experimental Validation), D7 (Scalability Argument), D8 (Claim-Evidence Alignment), and D15 (Commercial vs. Academic Hardware). The Majorana 1 case shows how a real hardware result can be overstated in its headline framing.

### Step 5: Share and Iterate

Submit your completed scoresheet to the community discussion threads. The most valuable contributions are: (a) identifying papers that score surprisingly high or low; (b) proposing new red flags or criteria within existing dimensions; (c) flagging dimensions where the rubric criteria don't map well to specific subfields; and (d) noting when a paper's score should be updated based on follow-up papers or community response. This rubric will be updated based on community feedback.

---

## Suggested Additions for v2

- **Quantum Volume and CLOPS Benchmarks:** Does the paper report standardized hardware benchmarks (Quantum Volume, Circuit Layer Operations Per Second) that allow cross-platform comparison?
- **Mid-Circuit Measurement and Feed-Forward:** Papers claiming adaptive or measurement-based computation should specify whether real-time classical feed-forward is supported on the target hardware and at what latency.
- **Magic State Factory Overhead:** For fault-tolerant computation papers using Clifford + T gate sets, is the cost of magic state distillation factored into resource estimates?
- **Qubit Leakage:** Does the noise model account for leakage to non-computational states (|2⟩, |3⟩), which is a significant error source in superconducting qubits often omitted from noise models?

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | April 2026 | Initial release. 9 dimensions, 3 generic worked examples, usage guide. |
| 1.1 | April 2026 | Added 6 new dimensions (D10–D15). Replaced all generic worked examples with scored analyses of Google Willow (Nature Dec 2024), IBM Bivariate Bicycle Codes (Nature Mar 2024), and Microsoft Majorana 1 (Nature Feb 2025). Updated usage guide for new dimensions. |

---

## Contributing

To propose changes, add worked examples, or flag errors:

- Open a discussion thread in the community forum
- Tag with `#quantum-rubric` and `#v1-feedback`
- For disputed paper scores, include specific citations to the paper text supporting an alternative score

---

*This rubric is provided under CC BY 4.0. Share freely with attribution.*
