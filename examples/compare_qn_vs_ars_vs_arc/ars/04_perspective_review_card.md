## Perspective Review Report (Peer Reviewer 3)

### Reviewer Identity

I am a researcher in quantum computing systems engineering and practical quantum algorithm implementation, with expertise in bridging theoretical algorithm design and near-term hardware constraints. My perspective comes from the practical implementation side of quantum computing—understanding what makes algorithms deployable on actual quantum devices versus theoretically elegant. I evaluate this paper through the lens of experimental realizability, resource estimation accuracy, and the gap between asymptotic complexity claims and constant-factor practical performance.

### Overall Recommendation
Accept

### Confidence Score
3

### Summary Assessment

This paper presents Trotter-LCU algorithms that combine product formulas with linear combination of unitaries to achieve improved Hamiltonian simulation performance. From a practical implementation perspective, the work addresses a genuine need: bridging the gap between high-precision post-Trotter methods that require complex coherent implementations and simpler Trotter methods with poor accuracy scaling. The random-sampling implementation using only one ancillary qubit is particularly valuable for near-term and early fault-tolerant devices. However, the paper's practical claims require careful scrutiny. The gate count comparisons in Figure 8, while showing 2-4 orders of magnitude improvements, rely on analytical bounds rather than empirical circuit simulations. The "easy to implement" framing may understate challenges in classical preprocessing, measurement overhead, and the practical implications of the µ⁴ sampling overhead. The paper would benefit from more explicit discussion of the practical regimes where these algorithms would outperform alternatives—particularly considering that the overhead factors hidden in asymptotic notation matter enormously for near-term applications. Despite these concerns, the core contribution of achieving commutator scaling with improved accuracy dependence through a simple random-sampling scheme represents genuine practical value.

### Strengths (3-5 items)

1. **Minimal Quantum Resource Requirements**: The algorithm requires only one ancillary qubit for random-sampling implementation, directly addressing a key constraint for near-term quantum devices. This is a significant practical advantage over coherent LCU implementations that require O(log Γ) ancilla qubits for amplitude encoding.

2. **Practical Circuit Structure**: The modification to existing Trotter circuits is additive—appending controlled-Pauli or controlled-Pauli-rotation gates after each Trotter segment. This means practitioners can leverage existing optimized Trotter circuit compilers without fundamental restructuring.

3. **Efficient Classical Sampling Algorithms**: The hierarchical sampling procedure (Figures 5, 7, and Algorithm 1) achieves O(K(log L + log κK)) time complexity per sample, which is practically efficient for lattice Hamiltonians. The explicit algorithmic descriptions enable direct implementation.

4. **Commutator Scaling Preservation**: The NCC algorithm retains the favorable system-size scaling of Trotter methods (near-linear in n for lattice systems) while improving accuracy dependence. This addresses a practical limitation of generic post-Trotter methods that scale as O(n²).

### Weaknesses (3-5 items)

1. **Sampling Overhead May Dominate in Practice**: The µ⁴ factor in sample complexity (Proposition 1) is described as acceptable when µ is "constant," but the paper sets µ = 2 for numerical comparisons, yielding a factor of 16 overhead. For fair comparison with Trotter methods, this overhead should be explicitly quantified in all gate count comparisons. The statement that this "provides an unbiased estimation" of observables glosses over the variance increase that directly impacts practical shot counts.

2. **Classical Preprocessing Costs Unaddressed**: The nested-commutator compensation algorithm requires computing commutator norms (∥Cs∥₁) and implementing the "padding" procedure for inhomogeneous Hamiltonians. For Hamiltonians beyond the simple Heisenberg model, these classical preprocessing costs could be substantial. The paper does not bound or discuss these costs, which is problematic for practitioners evaluating total resource requirements.

3. **Gate Count Comparisons Use Analytical Bounds**: Figure 8 compares against "the best analytical bound of fourth-order Trotter formula" rather than empirically optimized circuits. Randomized compilation techniques, adaptive Trotter ordering, and other practical optimizations can significantly reduce Trotter circuit depth beyond analytical worst-case bounds. The claimed "2 orders of magnitude" improvement may be optimistic.

4. **Regime of Practical Advantage Unclear**: The paper does not clearly delineate when these algorithms would be preferred over alternatives. For short times, standard Trotter suffices. For very high precision, coherent LCU/QSP may be better despite hardware complexity. The "sweet spot" where random-sampling Trotter-LCU provides practical advantage needs explicit characterization in terms of (t, ε, n, L) parameter regimes.

### Detailed Comments

#### Assumption Audit

- **Explicit assumptions**: The paper assumes single-qubit Z-rotation gates (Rz(θ)) are the dominant cost on fault-tolerant quantum computers, which is standard but masks significant variation—magic state distillation costs depend heavily on the rotation angle precision, not just gate count. The assumption that Hamiltonians decompose into efficiently implementable two-local terms (Eq. 10) is stated but may not hold for all practical applications (e.g., molecular Hamiltonians with many-body interactions).

- **Implicit assumptions**: The paper implicitly assumes that the classical random sampling can be performed efficiently in parallel with quantum circuit execution ("we can perform the classical sampling during the quantum circuit implementation or even generate the sampled Pauli matrices before the implementation"). This requires tight classical-quantum integration that may not be available on all platforms. Additionally, the comparison framework assumes observable estimation as the end goal—for applications requiring the full quantum state (e.g., as input to subsequent algorithms), the random-sampling approach is inappropriate.

- **Paradigmatic assumptions**: The paper operates within the paradigm that asymptotic complexity improvements translate to practical advantage. This paradigm often fails for quantum algorithms where constant factors, compilation overhead, and error correction costs dominate. The "orders of magnitude" claims would be more compelling if validated against concrete problem instances with explicit resource accounting.

#### Cross-Disciplinary Connections

- **Parallel research**: In quantum error correction and fault-tolerant compilation, similar "compilation by randomization" ideas appear in randomized compiling and probabilistic error cancellation. The relationship between this work and these error mitigation techniques is not discussed but could be valuable—particularly whether Trotter-LCU has favorable noise resilience properties.

- **Borrowing opportunities**: From the perspective of optimization algorithms, this work could benefit from adaptive methods that dynamically adjust the Trotter order K or truncation order sc based on intermediate measurement results. The fixed-order approach may leave performance on the table compared to adaptive strategies.

- **Methodological borrowing**: Monte Carlo variance reduction techniques from computational statistics (importance sampling, control variates, stratified sampling) could potentially reduce the µ⁴ sampling overhead. The paper's random sampling approach is basic; sophisticated sampling strategies might improve practical performance.

#### Practical Impact

- **Real-world application**: For early fault-tolerant quantum computers with limited ancilla qubits, this algorithm provides a concrete path to improved Hamiltonian simulation accuracy. The explicit sampling algorithms (Algorithm 1, Figures 5, 7) are implementation-ready.

- **Implementation feasibility**: The main barriers are: (1) the "padding" procedure for NCC requires Hamiltonian-specific preprocessing; (2) the Hadamard-test-type circuit requires mid-circuit measurement and reset (Figure 10c), which has hardware-dependent fidelity; (3) the µ⁴ sampling overhead means practical experiments require 16× more shots than standard Trotter at minimum.

- **Stakeholders**: The paper primarily addresses algorithm designers and quantum software developers. Hardware considerations (decoherence during the extended circuit, measurement/reset fidelity) are not discussed. Experimentalists implementing these algorithms would need guidance on error sensitivity.

#### Broader Implications

- **Ethical dimensions**: No significant ethical concerns. The work is fundamental algorithm research.

- **Social impact**: As with all quantum simulation algorithms, success would accelerate materials science, drug discovery, and other applications. No differential social impact concerns.

- **Future directions**: From an implementation perspective, the most valuable follow-up would be: (1) empirical validation on actual or simulated quantum hardware with noise; (2) integration with quantum error correction to understand logical error rates; (3) comparison against other random-sampling algorithms (qDRIFT, etc.) on specific benchmark Hamiltonians with full resource accounting.

### Cross-Disciplinary Reading Recommendations

- **Campbell, E. (2019). "Random Compiler for Fast Hamiltonian Simulation." Physical Review Letters 123, 070503.** — The qDRIFT algorithm takes a different random-sampling approach; comparison would clarify the relative advantages of Trotter-LCU versus pure randomized methods.

- **Koczor, B. (2021). "Exponential Error Suppression for Near-Term Quantum Devices." Physical Review X 11, 031057.** — Discusses variance reduction for expectation value estimation via randomization; techniques may apply to reduce the µ⁴ overhead.

- **van den Berg, E., et al. (2023). "Probabilistic error cancellation with sparse Pauli-Lindblad models on noisy quantum processors." Nature Physics 19, 1116.** — Demonstrates practical random-sampling quantum algorithms on real hardware; provides context for implementation challenges.

- **Childs, A. M., & Su, Y. (2019). "Nearly optimal lattice simulation by product formulas." Physical Review Letters 123, 050503.** — The tight Trotter bounds for lattice systems that this paper builds upon; important for understanding where the baseline comparisons come from.

### Questions for Authors

1. For the numerical comparisons in Figure 8, have you considered comparing against empirically optimized Trotter circuits (e.g., with randomized ordering or recompilation) rather than analytical worst-case bounds? The claimed improvement magnitude may be sensitive to this choice.

2. What is the practical variance of the observable estimator as a function of µ? The µ⁴ scaling suggests significant shot overhead; explicit numerical examples showing the required shot count for practical precision (e.g., 1% relative error on energy) would help practitioners evaluate the algorithm.

3. For the NCC algorithm on general (non-lattice) Hamiltonians, what is the classical preprocessing complexity to compute the commutator norms and implement the padding procedure? Is this tractable for, e.g., molecular Hamiltonians with hundreds of qubits?

4. How do these algorithms compare to qDRIFT and related randomized product formula methods in the same (n, t, ε) parameter regimes? A direct comparison would help position this work within the broader landscape of randomized Hamiltonian simulation.

### Minor Issues

- The notation Pol(s; λx) in Eq. (4) should be Poi(s; λx) for consistency with later usage.
- The term "Hamiltonian simulation" in the abstract could clarify whether coherent simulation or observable estimation is the primary goal, given the random-sampling focus.
- The Lambert W function reference (Lemma 7, Appendix F) is somewhat technical for the main text discussion; a simpler bound approximation might improve accessibility.
- Figure 6 caption refers to "virtual ancilla qubits" but the physical interpretation of "padding" could be clearer for implementers.