
# Hamming Weight Scaling of Grover's Algorithm under Amplitude Damping

Research implementation and experimental study of **Grover's quantum search algorithm under amplitude damping noise** in the NISQ regime.

This repository contains simulation code, experiments, and analysis supporting the observation that Grover's success probability under amplitude damping noise follows an exponential decay law dependent on both **circuit depth** and **Hamming weight of the quantum state**.

---

# Overview

Grover's algorithm provides a quadratic speedup for unstructured search, requiring O(√N) oracle queries for a database of size N.

However, current quantum hardware operates in the **NISQ (Noisy Intermediate-Scale Quantum) regime**, where noise accumulates throughout circuit execution.

This project investigates how **amplitude damping noise (T1 relaxation)** affects Grover's algorithm and identifies a key scaling law governing algorithmic degradation.

---

# Key Result

Empirical simulations show that Grover's success probability approximately follows:

P_success(w, d) ≈ exp(-p * w * d)

where

- p = amplitude damping probability per gate  
- w = Hamming weight of the target state  
- d = circuit depth

This indicates that algorithmic performance decays exponentially with the product of **Hamming weight and circuit depth**.

---

# Core Insight: The Hamming Weight Barrier

Reducing the Hamming weight of the target state might appear to improve robustness against amplitude damping.

To test this hypothesis, three Grover implementations were evaluated:

1. Standard Grover search for |1111>
2. Encoded search mapping |1111> → |0001>
3. Gray-code encoding mapping |1111> → |1000>

Despite reducing the target Hamming weight from w=4 to w=1, **no measurable improvement in success probability was observed**.

This occurs because Grover's algorithm operates primarily in a superposition dominated by non-target states.

For an n-qubit system, the expected Hamming weight of the uniform superposition is:

<w> ≈ n/2

As a result, the algorithm maintains a high average excited-state population throughout most of its execution.

---

# Time-Averaged Hamming Weight

Density matrix tomography during circuit execution shows that:

<w(t)> ≈ n/2

for most of the algorithm runtime, regardless of the chosen encoding.

The refined decay law becomes:

P_success ≈ exp(-p * <w_avg> * d)

where <w_avg> ≈ n/2.

This implies that the algorithm's **superposition structure**, rather than the final target state, determines noise sensitivity.

---

# Experimental Setup

Simulations were performed using:

- Qiskit Aer simulator
- Density matrix simulation for tomography
- Shot-based measurement experiments
- Amplitude damping noise channel with p = 0.1

Experiments were conducted on systems of:

- 2 qubits
- 3 qubits
- 4 qubits

Noise channels tested:

- amplitude damping
- phase damping
- depolarizing noise
- readout error

Only amplitude damping exhibited strong Hamming-weight dependence.

---

# Key Observations

### Hamming Weight Scaling

Increasing the number of |1> qubits in the target state significantly reduces success probability.

Example:

| Target | Hamming Weight | Success |
|------|------|------|
| 11 | 2 | 0.71 |
| 111 | 3 | 0.23 |
| 1111 | 4 | 0.084 |

### Encoding Does Not Help

Encoding strategies that reduce target Hamming weight do not improve performance due to the algorithm's time-averaged population distribution.

### Noise Asymmetry

Amplitude damping is strongly basis dependent, unlike symmetric noise channels such as depolarizing or phase damping.

---

# Methodology

Key experimental components:

- Grover circuits with multi-controlled oracle implementations
- Controlled depth inflation (H → H^3) to isolate depth effects
- Density matrix tomography at intermediate circuit depths
- Computation of expected Hamming weight:

<w> = Tr(ρW)

where W is the diagonal operator encoding basis-state Hamming weights.

---

# Conjecture: Hamming Weight Barrier

The experimental results motivate the following conjecture:

Any amplitude amplification algorithm initialized in a uniform superposition must maintain a time-averaged Hamming weight proportional to the number of qubits.

Consequently, amplitude damping introduces an intrinsic exponential degradation mechanism for quantum search in the NISQ regime.

---

# Scientific Contributions

1. Empirical identification of a scaling law governing Grover's performance under amplitude damping.
2. Demonstration that encoding strategies fail to mitigate this noise mechanism.
3. Time-resolved analysis of Hamming weight dynamics during Grover iterations.
4. Proposal of the **Hamming Weight Barrier** conjecture for amplitude amplification algorithms.

---

# Future Work

- Validate results on real quantum hardware.
- Extend experiments to larger qubit systems.
- Test other amplitude amplification algorithms.
- Investigate error correction strategies that mitigate amplitude damping.

---

# Author

Nihar Kartikeya  
Department of Engineering Physics  
Indian Institute of Technology Hyderabad

---

# References

Grover, L. K. (1996). A fast quantum mechanical algorithm for database search.

Preskill, J. (2018). Quantum computing in the NISQ era and beyond.

Qiskit Contributors. Qiskit: An open-source framework for quantum computing.
