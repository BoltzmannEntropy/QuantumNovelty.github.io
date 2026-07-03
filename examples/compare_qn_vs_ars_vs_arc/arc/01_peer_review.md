# Peer Review: Simple and high-precision Hamiltonian simulation by compensating Trotter error with LCU

## Paper Summary

This paper proposes composite Hamiltonian simulation algorithms that combine Trotter formulas with Linear Combination of Unitaries (LCU) to achieve improved accuracy and time scaling. The key contributions are: (1) Paired Taylor-Series Compensation (PTSC) which exponentially improves accuracy scaling, and (2) Nested-Commutator Compensation (NCC) which maintains near-optimal system-size dependence while quadratically improving accuracy over Trotter methods.

---

# Reviewer A

## Expertise
Quantum algorithms, Hamiltonian simulation, fault-tolerant quantum computing

## Summary Assessment
This is a well-written theoretical paper that presents a clever combination of Trotter and LCU methods for Hamiltonian simulation. The mathematical framework is rigorous and the results appear technically sound. However, there are concerns about the experimental validation and some gaps between claims and evidence.

## Strengths

1. **Novel algorithmic contribution**: The idea of using LCU to compensate Trotter error rather than replacing Trotter entirely is creative and practically motivated. The "order pairing" technique using Euler's formula (Eq. 7) to double the order of µ_x - 1 is an elegant insight.

2. **Strong theoretical foundations**: The paper provides rigorous proofs for both PTSC (Theorem 1) and NCC (Theorem 2) algorithms with explicit gate complexity bounds. The analysis connecting commutator structure to system-size scaling is thorough.

3. **Practical implementation considerations**: The hierarchical sampling algorithms (Fig. 5, Fig. 7) and the "padding" technique for non-homogeneous Hamiltonians demonstrate attention to practical implementation details.

4. **Clear exposition**: The paper does an excellent job building intuition through the progressive examples in Fig. 3 and Fig. 4 before presenting general results. The comparison table (Table I) effectively summarizes the contributions.

5. **Significant complexity improvements**: The claimed 2-4 orders of magnitude improvement in accuracy for the same gate count (Fig. 8c) over state-of-the-art Trotter methods is substantial if accurate.

## Weaknesses

### Major Issues

1. **CRITICAL: Lack of experimental/numerical validation evidence**
   - The paper presents gate count comparisons in Fig. 8 but **no actual quantum simulation results** are provided. 
   - The gate counts appear to be analytical estimates based on complexity bounds rather than empirical measurements from implemented circuits.
   - **No evidence of actual code execution, numerical simulations, or statistical trials** is presented in the paper to validate the theoretical claims.
   - The paper should clarify whether Fig. 8 represents analytical bounds or measured gate counts from implementations.

2. **Missing implementation details for reproducibility**:
   - Algorithm 1 provides pseudocode for first-order NCC sampling, but no complete implementation details for higher-order algorithms.
   - The paper does not specify how the rotation angles θ in Pauli rotations are computed in practice with finite precision.
   - No discussion of numerical stability issues in the sampling procedures.

3. **Limited baseline comparisons**:
   - Comparison is primarily against fourth-order Trotter with analytical bounds from Ref. [12, 20].
   - No comparison with other recent post-Trotter methods like truncated Taylor series [25] or QSP [27, 28] in the main text (relegated to Appendix G).
   - The µ = 2 setting for fair comparison may not be optimal for all methods.

4. **Incomplete analysis of practical overheads**:
   - The µ⁴ sampling overhead (Proposition 1) means the method requires 16× more samples when µ = 2. This is mentioned but not adequately discussed in context.
   - The paper truncates at "the gate complexity estimation is then converted to" (line cut off), leaving analysis incomplete.

### Minor Issues

5. **Clarity issues in notation**: The vector notation (e.g., r⃗, p⃗) is sometimes inconsistent between sections.

6. **Missing error analysis**: While truncation error is bounded (Propositions 3, 4), the accumulation of errors over many segments is not rigorously analyzed beyond the product formula (Proposition 2).

## Questions for Authors

1. Have you implemented the algorithms and run numerical experiments? If so, what are the actual gate counts vs. the analytical bounds?

2. How does the performance degrade when using finite-precision rotation angles?

3. Can you provide explicit comparisons with QSP-based methods for the lattice Hamiltonian examples?

## Actionable Revisions

1. **Add numerical validation**: Implement the algorithms and provide actual circuit simulations validating the gate count estimates.

2. **Expand baseline comparisons**: Include direct comparisons with QSP and truncated Taylor series in the main text.

3. **Discuss the µ⁴ overhead more thoroughly**: Provide guidance on when the method is practically advantageous given this sampling cost.

4. **Complete the truncated analysis**: The paper appears cut off in Section IV C.

## Recommendation

**Score: 6/10 - Weak Accept**

The theoretical contributions are solid and the algorithmic ideas are novel. However, the lack of empirical validation beyond analytical gate count estimates is a significant weakness for a methods paper. The paper would be strengthened considerably by numerical experiments demonstrating the practical performance of the algorithms.

---

# Reviewer B

## Expertise
Quantum computing theory, complexity analysis, near-term quantum algorithms

## Summary Assessment
This paper presents theoretically interesting algorithms combining Trotter and LCU methods. While the mathematical framework is sophisticated, I have concerns about the completeness of the presentation and the gap between theoretical claims and supporting evidence.

## Strengths

1. **Addresses a real practical problem**: The tension between Trotter's implementation simplicity and post-Trotter's accuracy is a genuine challenge. The proposed hybrid approach is well-motivated.

2. **Comprehensive theoretical analysis**: The paper provides complexity bounds for multiple algorithm variants (0th, Kth-order PTSC; Kth-order NCC) with explicit dependence on all relevant parameters.

3. **Commutator structure exploitation**: The NCC algorithm's ability to maintain O(n^{1+2/(2K+1)}) scaling for lattice Hamiltonians while improving accuracy is theoretically appealing.

4. **Practical sampling procedures**: The hierarchical sampling avoiding full expansion of nested commutators (Sections II.D) is important for practical implementation.

5. **Good presentation quality**: Figures 3-4 provide excellent visual intuition for the order-pairing technique.

## Weaknesses

### Critical Issues

1. **PAPER APPEARS INCOMPLETE**
   - The paper text cuts off mid-sentence: "Moreover, the accuracy scaling is exponentially im-" at the end of Section IV.C
   - Section V (Nested-Commutator Compensation detailed analysis) is referenced but not provided
   - Section VI (Conclusion) is missing
   - Multiple appendices (A-H) are referenced but not included
   - This makes it impossible to fully evaluate the NCC algorithm claims

2. **NO EXPERIMENTAL EVIDENCE PROVIDED**
   - The paper makes quantitative claims ("2 orders of magnitude smaller," "3 to 4 orders of magnitude higher accuracy") 
   - Fig. 8 shows gate count comparisons but the methodology for generating these plots is unclear
   - **No code, no numerical experiments, no statistical analysis** is provided to support the claims
   - The "gate counting method for fourth-order Trotter with random permutation is based on the analytical bounds in Ref. [12]" - so this is purely theoretical comparison

3. **Missing critical algorithmic details**:
   - How is the truncation order s_c chosen in practice? The bound in Eq. 85 involves Lambert W function - is this computed exactly?
   - The "variant circuit" in Fig. 2(c) with measurement and reset is claimed equivalent to Fig. 2(b) - no proof provided
   - Error accumulation across segments needs more rigorous treatment

### Methodology-Evidence Consistency Issues

4. **Claims without supporting evidence**:
   - Claim: "2 orders of magnitude smaller than the best analytical bound of fourth-order Trotter formula" (Abstract)
   - Evidence: Fig. 8(a,b) shows this, but methodology is analytical bounds comparison, not empirical measurement
   
   - Claim: "3 to 4 orders of magnitude higher accuracy with the same gate costs" (Abstract)
   - Evidence: Fig. 8(c) - again, based on analytical bounds from Ref. [20], not empirical tests

5. **Venue appropriateness**: This paper is marked for PRX Quantum, which is appropriate for theoretical quantum computing papers. However, even for a theory venue, numerical validation of complexity bounds is standard practice.

### Minor Issues

6. **Notation overload**: The paper uses many subscripts and superscripts (e.g., F_{1,s}^{(nc)}, V_1^{(p)}, R_{1,s}) which can be difficult to track.

7. **Limited discussion of constants**: The constants in complexity bounds (e.g., κ_K = 2 × 5^{K/2-1}) grow quickly with K; practical implications not discussed.

8. **Missing related work**: No comparison with randomized compiling methods or other error mitigation approaches that could achieve similar goals.

## Questions for Authors

1. Can you provide the missing sections (V, VI) and appendices?

2. Have you implemented the algorithms? What are the actual vs. predicted gate counts?

3. For Fig. 8, are these analytical bounds or measured values? What is the exact methodology?

4. How sensitive is the performance to the choice of µ? Is µ = 2 optimal?

## Actionable Revisions

1. **CRITICAL: Complete the paper** - Sections V, VI and appendices are essential for evaluation.

2. **Add numerical experiments**: Even for a theory paper, implementing the sampling algorithms and comparing actual circuit depths would strengthen claims significantly.

3. **Clarify Fig. 8 methodology**: Explicitly state whether values are analytical bounds or empirical measurements.

4. **Provide complexity comparison table with explicit constants**: The asymptotic notation hides potentially large constants.

5. **Discuss practical limitations**: When does the µ⁴ overhead make the method impractical? What system sizes are needed for advantages to manifest?

## Recommendation

**Score: 5/10 - Borderline (leaning reject)**

The theoretical ideas are interesting and potentially impactful. However, the paper is clearly incomplete (missing sections, truncated text), and there is no experimental validation of the theoretical claims. The gap between the sophisticated mathematical analysis and the lack of any empirical evidence is concerning. 

For PRX Quantum, I would expect at least numerical validation of gate counts on representative Hamiltonians. The current submission appears to be a draft that was submitted prematurely.

**Conditional recommendation**: If the authors can provide (1) complete paper with all sections, (2) numerical validation experiments, and (3) clarified methodology for Fig. 8, this could be a solid contribution suitable for publication.

---

# Summary of Critical Issues Requiring Revision

| Issue | Severity | Reviewer |
|-------|----------|----------|
| Paper incomplete (missing Sections V, VI, appendices) | CRITICAL | B |
| No experimental/numerical validation | MAJOR | A, B |
| Fig. 8 methodology unclear (analytical vs empirical) | MAJOR | A, B |
| Text cuts off mid-sentence | CRITICAL | B |
| Missing baseline comparisons with QSP | MODERATE | A |
| µ⁴ overhead implications not discussed | MODERATE | A, B |