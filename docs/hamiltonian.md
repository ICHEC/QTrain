# Scynergy 2025

## First link with material
- (TBD) Specific items from https://github.com/LuxProvide/PennyLane-GPU 
- All training material should be in Qiskit
- Topics for introduction


## Prerequisite (read beforehand)

- Algebra (missing)
    - Vectors:
    [Vectors, chapter 1, Essence of linear Algebra (Video)](https://youtu.be/fNk_zzaMoSs?si=YgNzHf28LbB4xiyA)

    - Linear combinations, span and matrices:
    https://www.youtube.com/watch?v=k7RM-ot2NWYlist=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&index=2

    - Linear transfromations and matrices:
    https://www.youtube.com/watch?v=kYB8IZa5AuElist=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&index=3

    - Matrix mutiplication:
    https://www.youtube.com/watch?v=XkY2DOUCWMU&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&index=4

    - Eigenvectors and eigenvalues:
    https://www.youtube.com/watch?v=PFDu9oVAE-g&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&index=14

- Complex numbers (missing)
    - Complex number overview:
    [Introduction to Partial Differential Equations](https://math.libretexts.org/Bookshelves/Differential_Equations/Introduction_to_Partial_Differential_Equations_(Herman)/08%3A_Complex_Representations_of_Functions/8.02%3A_Complex_Numbers)

    - Imaginary numbers:
    https://www.youtube.com/watch?v=hqr1DtXXHpY&list=PLHJcI57De8cp_iiPlKUDhNOexGCQYkxL3

    - Complex numbers
    https://www.youtube.com/watch?v=bmsapLZM2Uo&list=PLHJcI57De8cp_iiPlKUDhNOexGCQYkxL3&index=2

    - https://www.youtube.com/watch?v=V7mECV0M1ys&list=PLHJcI57De8cp_iiPlKUDhNOexGCQYkxL3&index=5

    - [Linear gradient (2D), Optimisation related:](http://www.cedar.buffalo.edu/~srihari/CSE676/4.2%20Gradient-based%20Optimization.pdf)

- Qubits, Gates, circuits (missing)
    - [Bits to Qubits](https://ichec.github.io/ct4106/lecture-03/from-bits-to-qubits.html)
- Hamiltonian - introduction (missing)
    - https://www.youtube.com/watch?v=BusR0WQ_Gxo

- Classical SVM (?, missing)
- Live tutorial
- Qiskit introduction (adapt from Pennylane one)
- Define and print basic circuits manually
- Device simulators (ideal vs noisy vs fake)
- Run simple circuit examples (missing?)
- Basic end-to-end example
- Grover?, but simpler/shorter, more showing the different stages of the end-to-end pipeline (data preprocessing, quantum computation, result interpretation)

> [NOTE] closing jupyter lab does not stop the job on the HPC note – need to document (using scancel)


# The Hamiltonian and Time Evolution in Quantum Mechanics

In quantum mechanics, the **state** of a physical system — such as a particle or an atom — is described by a mathematical object called a **state vector**, usually written as $ \ket{\psi(t)} $. This state can change or **evolve** over time, and the rules for how it changes are given by a fundamental equation in quantum mechanics called the **time-dependent Schrödinger equation**:

$$
i\hbar\frac{d}{dt}\ket{\psi(t)} = H(t)\ket{\psi(t)}
$$

Let’s break this down:

- $ \hbar $ is **Planck’s constant divided by $2\pi$**. It appears throughout quantum mechanics and ensures that equations have the correct units.
- $ \ket{\psi(t)} $ is the state of the system at time $ t $. It's a vector in a complex vector space called **Hilbert space**.
- The operator $ H(t) $, called the **Hamiltonian**, plays a central role. It represents the **total energy** of the system — both kinetic (motion) and potential (position-dependent forces).
- The left-hand side of the equation is the **rate of change** of the state with respect to time, and the right-hand side tells us how the Hamiltonian causes this change.

In essence, the Schrödinger equation is the quantum version of Newton's laws — it tells us how a system evolves from one moment to the next.

---

## What is the Hamiltonian?

The **Hamiltonian** is a special kind of operator: it’s **Hermitian**, which means it oberys the mathematical condition $H^\dagger=H$. This ensures that it has real eigenvalues and a complete set of orthonormal eigenstates. This is important because it means the outcomes of energy measurements (which are associated with these eigenvalues) are always real numbers — just as we observe in experiments.

If you've studied classical physics, you might recognize the Hamiltonian from **classical mechanics**, where it's a function representing the total energy of a system in terms of coordinates and momenta. In quantum mechanics, the Hamiltonian plays a similar role, but it’s an **operator** rather than a function.

---

## Time Evolution When the Hamiltonian is Constant

If the Hamiltonian does **not** change with time — such as when a particle (e.g. electron) moves in a fixed external field (e.g. the field produced by a proton) — we say it is **time-independent**, and the evolution of the state has a particularly elegant form.

Let’s say the Hamiltonian $ H $ has a set of **eigenstates** $ \ket{E_n} $, each with a corresponding **energy eigenvalue** $ E_n $. These eigenstates satisfy:

$$
H\ket{E_n} = E_n\ket{E_n}
$$

Any state $ \ket{\psi(0)} $ can be written as a **superposition** (a linear combination) of these eigenstates:

$$
\ket{\psi(0)} = \sum_n \braket{E_n|\psi(0)} \ket{E_n}
$$

Then, at any later time $ t $, the state evolves as:

$$
\ket{\psi(t)} = \sum_n e^{-iE_n t/\hbar} \braket{E_n|\psi(0)} \ket{E_n}
$$

Here’s what’s happening:
- Each energy component of the state simply picks up a **phase factor** $ e^{-iE_n t/\hbar} $ over time.
- The **probability** of measuring a particular energy doesn’t change — only the **relative phases** between components do.
- These phase changes lead to **interference effects**, which are responsible for much of the rich behavior in quantum systems.

---

## Summary

- The Schrödinger equation describes how quantum states evolve over time.
- The Hamiltonian is the operator associated with the system’s energy and drives the time evolution.
- When the Hamiltonian is time-independent, the solution is a simple sum over energy eigenstates with time-dependent phase factors.
- This framework underpins many areas of quantum physics, from atomic structure to quantum computing.

