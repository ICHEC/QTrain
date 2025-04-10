# 1. Quantum Chemistry: Study a molecule


## Finding the ground state of a molecule via VQE

Based on example from https://github.com/qiskit-community/qiskit-nature-pyscf

## Introduction

This exercise provides a very small, illustrative prototype of what one does in the area of material science, chemistry and pharmacuticals for the purpose of property computations, predictions, and computational material discovery.

### Use case

What we're doing and why

### Theory

Explain classical and quantum solution

In this notebook, we investigate the ground-state electronic structure of dimers (molecules made up of two atoms) using both classical and quantum computational methods. Traditional quantum chemistry approaches such as Full Configuration Interaction (FCI) provide accurate results for small molecules, but quickly become too computationally expensive as the number of orbitals increases for larger molecules. To explore the potential of quantum computing in this domain, we employ the Variational Quantum Eigensolver (VQE) with a Unitary Coupled Cluster (UCCSD) ansatz, using Qiskit. By comparing the computed ground-state energies from HF, FCI, and VQE across different bond lengths, we assess the accuracy and feasibility of hybrid quantum-classical simulations for molecular electronic structure problems.

## Computational workflow
```{mermaid}
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

