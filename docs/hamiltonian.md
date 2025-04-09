# The Hamiltonian and Time Evolution in Quantum Mechanics

In quantum mechanics, the **state** of a physical system — such as a particle or an atom — is described by a mathematical object called a **state vector**, usually written as $ \ket{\psi(t)} $. This state can change or **evolve** over time, and the rules for how it changes are given by a fundamental equation in quantum mechanics called the **time-dependent Schrödinger equation**:

$$
i\hbar\frac{d}{dt}\ket{\psi(t)} = H(t)\ket{\psi(t)}
$$

Let’s break this down:

- $ \hbar $ is the **reduced Planck's constant**, which is Planck’s constant $ h $ divided by $2\pi$. It appears throughout quantum mechanics and ensures the equation has correct units.
- $ \ket{\psi(t)} $ is the state of the system at time $ t $. It's a vector in a complex vector space called **Hilbert space**.
- The operator $ H(t) $, called the **Hamiltonian**, plays a central role. It represents the **total energy** of the system — both kinetic (motion) and potential (position-dependent forces).
- The left-hand side of the equation is the **rate of change** of the state with respect to time, and the right-hand side tells us how the Hamiltonian causes this change.

In essence, the Schrödinger equation is the quantum version of Newton's laws — it tells us how a system evolves from one moment to the next.

---

## What is the Hamiltonian?

The **Hamiltonian** is a special kind of operator: it is **Hermitian**, which means it obeys the mathematical condition $H^\dagger=H$. This ensures that it has real eigenvalues and a complete set of orthonormal eigenstates. This is important because it means the outcomes of energy measurements (the eigenvalues) are always real numbers — just as we observe in experiments.

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

