# 1. Quantum Chemistry: Study a molecule


## Finding the ground state of a molecule via VQE

Based on example from https://github.com/qiskit-community/qiskit-nature-pyscf

## Introduction

This exercise provides a very small, illustrative prototype of what one does in the area of material science, chemistry and pharmacuticals for the purpose of property computations, predictions, and computational material discovery.

### Use case

In this exercise, we investigate the **ground-state electronic structure** of **diatomic molecules (dimers)** using a combination of classical and quantum computational methods.Specifically, we calculate the ground-state energy of several small molecules across a range of bond lengths. These calculations help us understand how the **electronic structure evolves with molecular geometry**, which is important for studying chemical bonding and molecular stability.

#### Quick defintions
**Ground state energy**
Some definition here

**Electronic structure**
some definition here


The goal is to compare the accuracy and performance of different approaches:
These approached use different type of either formalism, or approximations to formulate
the problem to a minimization problem.

  1. **Hartree-Fock (HF)** as a mean-field classical approximation,
  2. **Full Configuration Interaction (FCI)** as an exact (but expensive) classical method,
  3. **Variational Quantum Eigensolver (VQE)** as a hybrid quantum-classical algorithm that is promising for near-term quantum devices.

First two of the above methods are examples of classical computation, while the third one utilises quantum computing. The HF is an approximate method, which is usually very inexpensive compared to the FCI, which is a lot closer to exact solution.

By applying these methods to the same set of molecular systems and geometries, we can compare their outcome, and use the comparison evaluate the **viability of quantum algorithms like VQE** for practical molecular simulations, especially in the context of bond dissociation and chemical reactivity — tasks that are known to challenge classical methods.

Accurate ground-state energy calculations are essential for predicting molecular behavior, and hybrid quantum-classical algorithms offer a potential pathway to overcome classical scaling limits in quantum chemistry.


### Theory

Explain classical and quantum solution

In this notebook, we investigate the ground-state electronic structure of dimers (molecules made up of two atoms) using both classical and quantum computational methods. Traditional quantum chemistry approaches such as Full Configuration Interaction (FCI) provide accurate results for small molecules, but quickly become too computationally expensive as the number of orbitals increases for larger molecules. To explore the potential of quantum computing in this domain, we employ the Variational Quantum Eigensolver (VQE) with a Unitary Coupled Cluster (UCCSD) ansatz, using Qiskit. By comparing the computed ground-state energies from HF, FCI, and VQE across different bond lengths, we assess the accuracy and feasibility of hybrid quantum-classical simulations for molecular electronic structure problems.

## Computational workflow

Following flowchart shows the computational workflow and illustrates how we map the problem of computing molecular energy to execution of a quantum circuit, and how it yields the ground state energy.

```{mermaid}
:align: center
%%{
    init: {
        'theme': 'base',
        'themeVariables': {
        'primaryColor': '#BB2528',
        'primaryTextColor': '#fff',
        'primaryBorderColor': '#7C0000',
        'lineColor': '#F8B229',
        'secondaryColor': '#006100',
        'tertiaryColor': '#fff'
        }
    }}%%
graph TD
subgraph Problem
direction LR;
    A((Define Molecule))
    A --> B(Select active orbitals & electrons)
end
    
subgraph Mapping
direction LR;
    C(Choose Qubit Mapping: Jordan-Wigner/Parity)
    C --> D(Select Ansatz: UCCSD, PUCCD, etc. This defines circuit structure.)
end
    Problem --> Mapping
    
subgraph Circuit
direction LR;
    E(Initialize Parameters - all zero)
    E --> F(Initial state = Hartree-Fock state)
    F --> G(Run Quantum Circuit & Measure)
end
    Mapping --> Circuit
    
    Circuit --> H(Compute Energy i.e. Expectation Value of Hamiltonian)
    H --> I{Convergence?}
    I -- No --> J(Update Parameters via Classical Optimizer) --> Circuit
    I -- Yes --> K(Return Optimized Energy & Parameters);
```

A brief introduction to what Hamiltonian is can be found [here](./hamiltonian.md).


### LiH As example

In the actual exercise, we use **LiH** as an example of a diatomic molecule.

- Electronic structures of lithium and hydrogen are as follows
    - Li: $1s^22s^12p^0$
    - H:  $1s^1$

To map this problem to quantum computing, we first try to find out the number
of `active orbitals` that we need to include.

* The inner 1s orbital of Li is frozen, i.e. these two electrons are paired as $\ket{\uparrow\downarrow}$. We do not include these in the simulation, to reduce the number of qubits required.
* For the five spatial orbitals that we are including in our simulation ($1\times$ Li 2s, $3\times$  Li 2p and $1\times$ H 1s) we need two qubits each, one to represent occupied/unoccupied and one to encode spin up/down

- In total: 5 active orbitals * 2 qubits each = 10 qubits
- We have two valence electrons, one from the outer shell of the lithium atom and one from hydrogen atom
-  We assume that these are split evenly between spin up and down, i.e. one alpha and one beta electron

Once we decided the active space, we need to map this problem to a qubit problem.


In the exercose we show two mappers, giving us two different ways to convert the electronic structure of a molecule from a fermionic problem to a qubit problem.

#### Jordan-Wigner Mapper: Simple, intuitive
- 1 spin orbital $\rightarrow$ 1 qubit
- unoccupied $\rightarrow\ket{0}$; occupied $\rightarrow\ket{1}$
- spin down $\rightarrow\ket{0}$; spin up $\rightarrow\ket{1}$


#### Parity Mapper: Can reduce number of qubits required in certain cases
- the Parity Mapper transforms the problem into global parity information instead of directly encoding individual occupation numbers
- each qubit stores the total parity of all previous orbitals.

In qiskit programming, it is as easy as follows -

```python
mapper = JordanWignerMapper()
mapper = ParityMapper(num_particles=(n_alpha, n_beta))
```

### Comparing Hartree-Fock approximation to VQE output

Converged SCF energy = Hartree-Fock energy, in Hartrees, from mean-field approximation, ignoring electron correlation.

- CASCI E = Complete Active Space Configuration Interaction (CASCI) energy.
- CASCI improves upon HF by allowing a full CI expansion within the active space (selected orbitals/electrons).
- Since CASCI includes static correlation effects, it should always be lower (more negative) than the SCF energy.

Points to note:
- number of qubits $q_i$ (number of wires)= 2 * (number of orbitals)
- basic gates: CNOT (entangling), Pauli (X, i.e. bit-flip), parameterised rotations R_Z(t[j])
- t[j] are the VQE parameters that are optimised during the algorithm


### Classical Solution
We want to compare our solution from the quantum algorithm (VQE) with the best classical solution. FCI (Full Configuration Interaction) provides a numerically exact solution for the ground state by solving the electronic Schrödinger equation. This is acheieved by fully diagonalizing the Hamiltonian in the complete active space of Slater determinants. This approach however is very computationally expensive — scaling exponentially with the system size. FCI is only possible for small single atoms or very small molecules with ~12 electrons or fewer.

## Bond length variation

Some information here.



## Conclusions

Real world results and experience

## References

1. https://www.ichec.ie/qpfas