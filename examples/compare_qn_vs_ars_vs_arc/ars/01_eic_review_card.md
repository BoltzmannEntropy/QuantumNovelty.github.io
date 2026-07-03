## EIC Review Report

### Reviewer Identity
Editor-in-Chief of PRX Quantum, evaluating this manuscript on Hamiltonian simulation algorithms that combine Trotter formulas with linear combination of unitary (LCU) methods. PRX Quantum publishes high-impact research across quantum information science, quantum computing, and quantum technologies, with particular interest in algorithmic advances that bridge theoretical innovation with practical implementation.

### Overall Recommendation
**Minor Revision**

### Confidence Score
4 (High confidence - Quantum algorithms and Hamiltonian simulation are core topics for PRX Quantum)

### Summary Assessment

This paper proposes composite Hamiltonian simulation algorithms that combine Trotter formulas with LCU compensation to achieve improved time and accuracy scaling. The core idea—using Trotter circuits for the bulk of the simulation while employing LCU to compensate the leading-order Trotter error—is elegant and addresses a genuine need in the field. The authors demonstrate that by adding simple Pauli-rotation gates after each Trotter segment, one can achieve time scaling of t^(1+1/(2K+1)) compared to t^(1+1/K) for bare Kth-order Trotter, with exponentially improved accuracy dependence (logarithmic in 1/ε for PTSC algorithms).

The manuscript presents two algorithms: Paired Taylor-Series Compensation (PTSC) for generic Hamiltonians and Nested-Commutator Compensation (NCC) for lattice Hamiltonians with locality structure. The numerical comparisons suggest 2-4 orders of magnitude improvement over fourth-order Trotter in gate counts for practical scenarios. The work sits at a valuable intersection of theoretical rigor and practical applicability—requiring only one ancillary qubit and avoiding the multi-controlled gates that make post-Trotter methods challenging to implement near-term.

The contribution is timely given the push toward early fault-tolerant quantum computing. However, the presentation could be streamlined, and certain claims about practical advantages require stronger empirical grounding beyond analytical bound comparisons.

### Strengths (3-5 items)

1. **Elegant conceptual contribution**: The insight that Trotter errors can be efficiently compensated using LCU formulas—leveraging the order condition to eliminate leading terms and Euler's formula to pair remaining anti-Hermitian terms—represents a genuinely novel synthesis of two established approaches. The "best of both worlds" framing (commutator scaling from Trotter + high accuracy from LCU) is well-motivated and clearly demonstrated.

2. **Practical implementation focus**: The algorithms require only one ancillary qubit and simple Pauli-rotation gates, making them significantly more implementable than coherent LCU or QSP methods that need extensive ancilla systems and multi-controlled Toffoli gates. The random-sampling implementation (Sec. II.D, Figs. 5 and 7) provides explicit, efficient classical sampling procedures.

3. **Strong analytical framework with dual algorithms**: The paper provides rigorous performance guarantees (Theorems 1 and 2) while offering two complementary algorithms—PTSC for generic Hamiltonians achieving log(1/ε) accuracy scaling, and NCC for lattice Hamiltonians achieving nearly optimal system-size scaling O(n^(1+2/(2K+1))). This breadth addresses different use cases effectively.

4. **Compelling numerical evidence**: The gate-count comparisons in Figure 8 demonstrate substantial practical advantages—2 orders of magnitude improvement over fourth-order Trotter for generic Hamiltonians, and 3-4 orders of magnitude higher achievable accuracy for lattice models at fixed gate budget. The comparisons against state-of-the-art analytical bounds add credibility.

5. **Clear pedagogical presentation of key ideas**: Figures 3 and 4 effectively illustrate how the "pairing" idea progressively suppresses the 1-norm, making the core technical insight accessible even to readers less familiar with LCU methods.

### Weaknesses (3-5 items)

1. **Analytical bounds comparison may overstate practical advantage**: The numerical comparisons (Fig. 8) pit the authors' algorithms against analytical Trotter bounds from Refs. [12, 20], which are known to be loose. A fairer comparison would include empirical Trotter error measured through numerical simulation, or at minimum acknowledge that analytical bounds are pessimistic. The claimed "2-4 orders of magnitude" improvement may be smaller in practice. *Improvement: Include numerical simulations of actual Trotter errors for the benchmark Hamiltonians, or clearly caveat that comparisons are bound-to-bound rather than performance-to-performance.*

2. **Limited discussion of sampling overhead**: While the paper acknowledges the µ^4 sampling overhead (Proposition 1), it somewhat downplays this cost by "setting µ to be constant." For PTSC, this means µ^4 = 16 (with µ = 2), but the impact on total runtime relative to coherent Trotter implementation deserves more explicit discussion. Additionally, the variance of the random-sampling estimator and its impact on sample complexity for different observables is not thoroughly analyzed. *Improvement: Add a dedicated subsection quantifying when the random-sampling approach outperforms coherent Trotter, including explicit sample-complexity breakeven analysis.*

3. **NCC gate-count analysis incomplete for higher orders**: The paper primarily presents second-order NCC results (Fig. 8c), noting that "precise higher-order NCC gate count analysis" is left for future study. This is a notable gap given that the theoretical scaling improvements are more dramatic at higher K. *Improvement: At minimum, provide rough estimates or bounds for fourth-order NCC, or justify why second-order is the primary practical regime.*

4. **Presentation length and density**: At 23 pages (main text) plus extensive appendices, the manuscript is long and technically dense. Some material, particularly the detailed derivations in Sec. IV and V, could be condensed by moving intermediate steps to appendices. The paper would benefit from a more streamlined main text emphasizing key ideas and results. *Improvement: Target a main text closer to 15 pages, moving detailed proofs to supplementary material while maintaining rigor.*

### Detailed Comments

#### Journal Fit
The paper fits PRX Quantum's scope extremely well. Hamiltonian simulation is a core topic in quantum algorithms research, and the work addresses both theoretical foundations and practical implementation considerations. The balance between analytical rigor and near-term applicability aligns with the journal's mission to publish impactful quantum information science research. The readership—quantum computing theorists, quantum algorithm developers, and those working toward early fault-tolerant implementations—will find this directly relevant.

#### Originality
The conceptual contribution is original: while Trotter and LCU methods are both well-established, their systematic combination to compensate Trotter error using LCU formulas with constant 1-norm is novel. The "pairing" technique using Euler's formula to double the suppression order of the 1-norm remainder (Eq. 7 and Fig. 3c) is a clever technical contribution. The nested-commutator expansion maintaining locality structure (Sec. V) extends this to lattice systems in a non-trivial way. The work builds substantively on prior literature (Refs. [8, 13, 20, 25, 33-35]) while contributing genuinely new methodology.

#### Significance
If the claimed performance holds in practice, this work could influence how near-term and early fault-tolerant Hamiltonian simulation is performed. The algorithms offer an attractive middle ground: easier to implement than full post-Trotter methods, yet achieving better scaling than bare Trotter. For quantum chemistry applications (where high precision is needed) and lattice model simulations (where system size scales are relevant), the improvements could be practically meaningful. The impact is more incremental than transformative—refinement rather than paradigm shift—but valuable for the field's practical progress.

#### Structural Coherence
The paper is well-structured overall: Section II provides an accessible summary before technical details, and Theorems 1-2 clearly state the main results. However, the flow from PTSC (Sec. IV) to NCC (Sec. V) could be smoother—the connection between these algorithms and when to prefer one over the other is not immediately clear. The duplicate "Department of Computer Science" in the author affiliations (affiliation 4) should be corrected.

#### Title & Abstract
The title is descriptive and accurate. The abstract effectively conveys the key contributions: both algorithms, their advantages (improved time/accuracy scaling, easy implementation), and quantitative improvement claims. The phrase "high-precision Hamiltonian simulation" appropriately signals the paper's focus.

#### Conclusion
Section VI provides adequate conclusions and future directions. The acknowledgment that empirical performance verification remains future work (implicit in the analytical focus) could be made more explicit.

### Questions for Authors

1. How do the PTSC and NCC algorithms perform when Trotter error is measured empirically (through numerical simulation) rather than compared against analytical bounds? Can you provide evidence that the improvement persists in realistic comparisons?

2. For random-sampling implementations, what is the crossover point in circuit depth or observable accuracy where the µ^4 = 16 sampling overhead becomes disadvantageous compared to simply running more coherent Trotter steps?

3. The paper focuses on expectation value estimation. How do the algorithms perform for other common tasks, such as state preparation fidelity or sampling from the output distribution?

4. For NCC applied to non-homogeneous lattice Hamiltonians, the "padding" technique (Sec. II.D, Eq. 18) introduces additional terms. How much does this inflate the effective 1-norm compared to homogeneous cases?

### Minor Issues

- Affiliation 4 lists "Department of Computer Science" twice—please correct.
- Equation (21) appears to have a coefficient of 4 that seems inconsistent with standard Heisenberg Hamiltonian conventions—please clarify or correct.
- Figure 8 legend could be clearer about which curves use commutator bounds versus non-commutator bounds.
- Some notation inconsistencies: ε is used both for LCU accuracy and simulation accuracy; clarify usage or use distinct symbols.
- The sentence "This allowss us to achieve optimal gate complexity" (p. 16) has a typo ("allowss").

### Recommendation to Peer Reviewers

**For the Methodology Reviewer**: Please scrutinize the 1-norm bounds in Propositions 3-6 and the gate-complexity derivations in Theorem 1-2. Are the asymptotic scalings tight, or could they be improved? Verify that the sampling procedures (Figs. 5, 7, Algorithm 1) are computationally efficient as claimed.

**For the Domain Reviewer**: Assess whether the numerical comparisons in Fig. 8 represent fair benchmarking against state-of-the-art Trotter implementations. Evaluate whether practitioners would find the implementation requirements (one ancilla, Pauli rotations, classical sampling) genuinely easier than alternatives.

**For the Devil's Advocate Reviewer**: Stress-test the practical significance claims. The µ^4 sampling overhead and the comparison against loose analytical bounds are potential vulnerabilities. Push on whether the "2-4 orders of magnitude" improvements survive under more realistic assumptions.