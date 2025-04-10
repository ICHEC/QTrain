# 2. Quantum machine learning for Earth observation

## Topic 2: QSVM4EO

This notebook will present the an example of using quantum-enhanced support vector machines (QSVM) for classification tasks on multi-spectral Earth Observation (EO) data. The main topics covered are introduction to classical SVM, where quantum computation could be used in the SVM calculations, the intricacies of preparing and encoding the classical data into useful quantum states and training QSVMs on gate-based quantum software simulators. Finally, some results on comparing classical and quantum-enhanced SVM models will be presented.

QSVM code is adapted from Qiskit QSVM example: https://qiskit-community.github.io/qiskit-machine-learning/tutorials/03_quantum_kernel.html


## Introduction

### Use case

What we're doing and why

### Theory
Explain classical and quantum solutions...

```{figure} ./images/SVM_margin.png
:align: center

```

Dual Lagrangian formulation:

$$
\mathcal{L}(\boldsymbol{\alpha})=\sum_{n=1}^{N} \alpha_{n}-\frac{1}{2} \sum_{n=1}^{N} \sum_{m=1}^{N} y_{n} y_{m} \alpha_{n} \alpha_{m} \mathbf{x}_{n}^{\top} \mathbf{x}_{m}
$$

```{figure} ./images/SVM_kernel.png
:align: center
```

$$
\mathcal{L}(\boldsymbol{\alpha})=\sum_{n=1}^{N} \alpha_{n}-\frac{1}{2} \sum_{n=1}^{N} \sum_{m=1}^{N} y_{n} y_{m} \alpha_{n} \alpha_{m} \mathbf{z}_{n}^{\top} \mathbf{z}_{m}
$$

Some classical kernel functions:

$$
\begin{array}{l|l|l}
\hline \text { Name } & \text { Kernel } & \text { Hyperparameters } \\
\hline \text { Linear } & \mathbf{x}^{T} \mathbf{x}^{\prime} & - \\
\hline \text { Polynomial } & \left(\mathbf{x}^{T} \mathbf{x}^{\prime}+c\right)^{p} & p \in \mathbb{N}, c \in \mathbb{R} \\
\hline \text { Gaussian } & \mathrm{e}^{-\gamma\left\|\mathbf{x}-\mathbf{x}^{\prime}\right\|^{2}} & \gamma \in \mathbb{R}^{+} \\
\hline \text { Exponential } & \mathrm{e}^{-\gamma\left\|\mathbf{x}-\mathbf{x}^{\prime}\right\|} & \gamma \in \mathbb{R}^{+} \\
\hline \text { Sigmoid } & \tanh \left(\mathbf{x}^{T} \mathbf{x}^{\prime}+c\right) & c \in \mathbb{R} \\
\hline
\end{array}
$$

```{figure} ./images/SVM_QSVM_workflows.png
:align: center
```

```{figure} ./images/kernel_circuit.png
:align: center
```
