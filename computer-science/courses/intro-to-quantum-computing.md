# Introduction to Quantum Computing

These are my notes for Qiskit's introductory course available on [YouTube](https://www.youtube.com/playlist?list=PLOFEBzvs-VvrXTMy5Y2IqmSaUjfnhvBHR).


## 1 - Qubits, Quantum States, Quantum Circuits, Measurements

### A - From bits to qubits

Classical computing depends on classical states of exclusive 0 or 1. In quantum mechanics a state can be in superposition (both 0 and 1).

Superpositions allows for calculations on multiple states at the same time. Quantum algorithms are consequently exponentially faster compared to several classical algorithms. 

However, measuring the superposition state causes one state to collapse which complicates designing effective quantum algorithms which leverage *interference effects*.

Dirac notation is the mathematical expression of quantum state.

- **Ket Notation**: Represents a column vector (state vector).
  - Example: \( | \psi \rangle \)

- **Bra Notation**: Represents a row vector (the conjugate transpose of the ket vector).
  - Example: \( \langle \psi | \)

### Example Quantum States

- **Zero State**: 
  - \( |0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \)

- **One State**: 
  - \( |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix} \)

- **Superposition State**: 
  - \( |\psi\rangle = \alpha |0\rangle + \beta |1\rangle \)
  - Where \( \alpha \) and \( \beta \) are complex numbers such that \( |\alpha|^2 + |\beta|^2 = 1 \).

### Inner Product

The inner product of two states \( | \phi \rangle \) and \( | \psi \rangle \) is written as:
- \( \langle \phi | \psi \rangle \)

### Outer Product

The outer product of two states \( | \phi \rangle \) and \( | \psi \rangle \) is written as:
- \( | \phi \rangle \langle \psi | \)

### Orthogonal States

Two states are orthogonal if their inner product is zero. For example, the zero state \( |0\rangle \) and the one state \( |1\rangle \) are orthogonal:

- \( \langle 0 | 1 \rangle = 0 \)

This means that the states \( |0\rangle \) and \( |1\rangle \) are mutually exclusive and can be perfectly distinguished from each other.