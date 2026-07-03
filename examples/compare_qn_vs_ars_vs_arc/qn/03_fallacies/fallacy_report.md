## Section 1: Markdown findings

### Finding 1

- **Fallacy:** cherry-picked-baseline
- **Severity:** medium
- **Location:** Section II.E, Table I, and throughout the numerical comparisons
- **Evidence:** "For a fair comparison, we set the 1-norm of all LCU formulas µ to be constant... We compile their quantum circuits to CNOT gates, single-qubit Clifford gates, and single-qubit Z-axis rotation gates Rz(θ) = eiθZ. Here, we mainly compare the number of Rz(θ) gates since they are the most resource-consuming part on a fault-tolerant quantum computer."
- **Why it's the fallacy:** The paper compares primarily against fourth-order Trotter with analytical bounds from Ref. [12] and [20], but does not compare against other recent hybrid or randomized Trotter methods that may offer competitive performance. The comparison with QSP in the appendix uses a specific qROM implementation from Ref. [52], but other block-encoding schemes or more recent QSP variants (e.g., quantum eigenvalue transformation with different ancilla strategies) are not considered. The baseline selection favors scenarios where the proposed method excels (high accuracy ε requirements, moderate system sizes).
- **Suggested fix:** Include comparisons with additional state-of-the-art methods such as higher-order randomized Trotter methods from Ref. [47] (Cho et al., cited in the paper itself as concurrent work), and discuss why certain baselines were omitted. Acknowledge that different methods may be preferable in different parameter regimes.

---

### Finding 2

- **Fallacy:** asymptotic-only-claim
- **Severity:** medium
- **Location:** Theorem 1 and Theorem 2 statements, Section IV.C and Section V.C
- **Evidence:** "From Theorem 1 we can see that, by introducing the LCU compensation, the time scaling of the Kth-order Trotter-LCU algorithm improve the bare Kth-order Trotter time scaling from 1 + 1/K to 1 + 1/(2K + 1). Moreover, the accuracy scaling is exponentially improved."
- **Why it's the fallacy:** The theoretical claims focus heavily on asymptotic scaling improvements (e.g., time scaling exponents, accuracy scaling), but the numerical demonstrations are limited to relatively small system sizes (n = 12, 20, 50) and modest simulation times. The asymptotic regime where the improved exponents dominate over constant factors is not clearly demonstrated. The paper acknowledges practical constants (e.g., the µ⁴ sampling overhead from Proposition 1) but does not systematically show when the crossover point occurs where the new method outperforms baselines.
- **Suggested fix:** Add explicit crossover analysis showing at what system size, time, or accuracy threshold the asymptotic improvements begin to dominate. Provide a table or figure showing the regime of practical advantage versus the regime where constant factors dominate.

---

### Finding 3

- **Fallacy:** conflated-regimes
- **Severity:** medium
- **Location:** Section II.E, Figure 8, and Section VI Conclusion
- **Evidence:** "From Fig. 8(c), we can see that, while enjoying near-optimal system-size scaling similar to the fourth-order Trotter algorithm which is currently the best one for lattice Hamiltonians, the second-order NCC algorithm shows better accuracy dependence than fourth-order Trotter algorithm. Particularly, using the same gate number as the fourth-order Trotter, we are able to achieve a 3 to 4 orders of magnitude higher accuracy ε."
- **Why it's the fallacy:** The numerical results shown in Figure 8(c) are for n = 12 and n = 50 qubit Heisenberg chains, which are relatively small systems. The extrapolation to larger, more practically relevant system sizes (hundreds or thousands of qubits for fault-tolerant quantum computing) is implicit but not demonstrated. The claim of "3 to 4 orders of magnitude higher accuracy" is based on these small-system numerics, and it's unclear whether this advantage persists at larger scales where the nested commutator structure may become more complex.
- **Suggested fix:** Explicitly state that the numerical demonstrations are limited to small-to-moderate system sizes and discuss how the advantage may scale. Consider adding extrapolation analysis or explicitly noting that the 3-4 orders of magnitude improvement is demonstrated only for the tested system sizes.

---

### Finding 4

- **Fallacy:** hasty-generalization
- **Severity:** medium
- **Location:** Section VI Conclusion, and Section II.E
- **Evidence:** "These algorithms provide an easy-to-implement approach to achieve a low-cost and high-precision Hamiltonian simulation."
- **Why it's the fallacy:** The claim of "easy-to-implement" is supported primarily by the fact that only one ancilla qubit is needed and Pauli rotation gates are used. However, the classical sampling procedure (Algorithm 1, Algorithm 2, Fig. 5, Fig. 7) involves non-trivial multinomial sampling, light-cone tracking, and padding procedures. The paper demonstrates the algorithm primarily on idealized lattice Hamiltonians (Heisenberg model) and 2-local generic Hamiltonians. Generalization to more complex Hamiltonians (e.g., quantum chemistry with many-body terms, non-local interactions) is discussed briefly in Appendix E but without numerical validation.
- **Suggested fix:** Temper the "easy-to-implement" claim by explicitly discussing the classical preprocessing complexity, or provide numerical demonstrations for more complex Hamiltonian structures beyond the idealized test cases.

---

### Finding 5

- **Fallacy:** pareto-cherry-picked-axes
- **Severity:** medium
- **Location:** Table I, Figures 8, 13-17, and Section II.E
- **Evidence:** "In Table I, we compare the implementation complexity and the gate complexity in a single round of experiment of the 0th-order PTSC, Kth-order PTSC, and Kth-order NCC algorithms to previous Hamiltonian simulation algorithms."
- **Why it's the fallacy:** The comparison metrics emphasize gate complexity (specifically Rz and CNOT counts) and accuracy scaling, but downplay or omit other important dimensions: (1) the µ⁴ sampling overhead is mentioned but not factored into the total cost comparisons in Table I; (2) classical preprocessing time for sampling is not compared; (3) qubit count comparison is shown only in Fig. 15 for QSP but not systematically tabulated; (4) the "implementation hardness" column in Table I uses qualitative labels ("Easy" vs "Hard") without precise definitions. The axes chosen for comparison favor the proposed methods.
- **Suggested fix:** Include the µ⁴ sampling overhead explicitly in the complexity comparisons when relevant, or present a more comprehensive resource table that includes total sample complexity, classical computation overhead, and ancilla requirements on equal footing.

---

## Section 2: Machine-readable JSON

```json
{
  "findings": [
    {
      "name": "cherry-picked-baseline",
      "category": "quantum-cs",
      "severity": "medium",
      "location": "Section II.E, Table I, Appendix G",
      "evidence": "For a fair comparison, we set the 1-norm of all LCU formulas µ to be constant... We compile their quantum circuits to CNOT gates, single-qubit Clifford gates, and single-qubit Z-axis rotation gates Rz(θ) = eiθZ. Here, we mainly compare the number of Rz(θ) gates since they are the most resource-consuming part on a fault-tolerant quantum computer.",
      "suggested_fix": "Include comparisons with additional state-of-the-art methods such as higher-order randomized Trotter methods from Ref. [47] and discuss why certain baselines were omitted. Acknowledge that different methods may be preferable in different parameter regimes."
    },
    {
      "name": "asymptotic-only-claim",
      "category": "quantum-cs",
      "severity": "medium",
      "location": "Theorem 1, Theorem 2, Section IV.C, Section V.C",
      "evidence": "From Theorem 1 we can see that, by introducing the LCU compensation, the time scaling of the Kth-order Trotter-LCU algorithm improve the bare Kth-order Trotter time scaling from 1 + 1/K to 1 + 1/(2K + 1). Moreover, the accuracy scaling is exponentially improved.",
      "suggested_fix": "Add explicit crossover analysis showing at what system size, time, or accuracy threshold the asymptotic improvements begin to dominate over constant factors. Provide a table or figure showing the regime of practical advantage."
    },
    {
      "name": "conflated-regimes",
      "category": "quantum-cs",
      "severity": "medium",
      "location": "Section II.E, Figure 8(c), Section VI",
      "evidence": "From Fig. 8(c), we can see that, while enjoying near-optimal system-size scaling similar to the fourth-order Trotter algorithm which is currently the best one for lattice Hamiltonians, the second-order NCC algorithm shows better accuracy dependence than fourth-order Trotter algorithm. Particularly, using the same gate number as the fourth-order Trotter, we are able to achieve a 3 to 4 orders of magnitude higher accuracy ε.",
      "suggested_fix": "Explicitly state that numerical demonstrations are limited to small-to-moderate system sizes (n ≤ 50) and discuss how the advantage may scale to larger systems relevant for fault-tolerant quantum computing."
    },
    {
      "name": "hasty-generalization",
      "category": "general",
      "severity": "medium",
      "location": "Abstract, Section VI Conclusion",
      "evidence": "These algorithms provide an easy-to-implement approach to achieve a low-cost and high-precision Hamiltonian simulation.",
      "suggested_fix": "Temper the 'easy-to-implement' claim by discussing the classical preprocessing complexity of the sampling procedures, or provide numerical demonstrations for more complex Hamiltonian structures beyond the idealized Heisenberg and 2-local test cases."
    },
    {
      "name": "pareto-cherry-picked-axes",
      "category": "quantum-cs",
      "severity": "medium",
      "location": "Table I, Figures 8, 13-17, Section II.E",
      "evidence": "In Table I, we compare the implementation complexity and the gate complexity in a single round of experiment of the 0th-order PTSC, Kth-order PTSC, and Kth-order NCC algorithms to previous Hamiltonian simulation algorithms.",
      "suggested_fix": "Include the µ⁴ sampling overhead explicitly in complexity comparisons, and present a more comprehensive resource table that includes total sample complexity, classical computation overhead, and ancilla requirements on equal footing with gate counts."
    }
  ]
}
```