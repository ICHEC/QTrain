# 3. Quantum Optimization



## Quadratic Unconstrained Binary Optimization (QUBO)


This section provides an overview of the essential tools required for working with QUBO formulations with Qiskit. We will be exploring small examples of classical optimization problems.

Quantum computing presents promising solutions for various optimization problems, particularly **Quadratic Unconstrained Binary Optimization (QUBO)** problems. Solving a QUBO is mathematically equivalent to determining the ground state of an associated Ising Hamiltonian.

This problem is crucial not only in optimization but also in fields such as quantum chemistry and physics. The transformation involves converting binary variables (values in {0,1}) into spin variables (values in {-1,+1}), which can then be expressed in terms of Pauli Z matrices, ultimately forming an Ising Hamiltonian. More details on this conversion process can be found in [1].

QUBO is a mathematical formulation that can express a variety of combinatorial optimization problems [2]. QUBO solvers can therefore address many problems, once expressed in this form.

``````{admonition} QUBO problem definition:
:class: information

```{card}
Minimize $y = {\bf x}^t {\bf Q} {\bf x}$
```

where $x$ is a vector of binary decision variables.
``````

Note that linear (binary) objective functions are expressed with $Q$ as a diagonal matrix, because $x_j^2 = x_j$.

### Quantum Approximate Optimization Algorithm (QAOA)

- QAOA is a discrete version of **Adiabatic Quantum Computing**
- In a nutshell: **Adiabatic quantum computing** involves starting with a simple Hamiltonian whose ground state can be easily prepared, evolving the system slowly to keep it in the ground state, i.e., **Adiabatic evolution**, and then measuring the result when the Hamiltonian's ground state represents the solution to the considered problem.
- **Adiabatic Quantum Computing** relies on a time-dependent Hamiltonian inducing continuous transformation of the state
- But we can approximate this continuous evolution with **discrete** changes which is called **Trotterization**
- So in QAOA, we will apply a sequence of discrete steps represented by quantum gates as following:

$$
|\beta,\gamma\rangle = e^{i\beta_p H_0}e^{i\gamma_p H_1} ... e^{i\beta_2 H_0} e^{i\gamma_2 H_1} e^{i\beta_1 H_0} e^{i\gamma_1H_1} |\psi_0\rangle
$$

with $H_0$ an initial and easy to prepare Hamiltonian, $H_1$ the target Hamiltonian whose ground state encodes the solution to the problem of interest and $\beta_p$, $\gamma_p$ scalarization values representing now the variables of our problem. We want to find $\beta$ and $\gamma$ that minimize :

$$
\min\limits_{\beta_p,\gamma_p} \langle \beta,\gamma|H_1|\beta,\gamma \rangle
$$



### Quadratic Problem example

In this example, we will solve an instance of a Quadratic Assignment Problem (QAP).
In this problem, there is $n$ facilities and $n$ locations. 
Locations are distant to one another, specified as a matrix $(D_{ij})$.
Also, facilities are connected via a flow matrix $(F_{ij})$.

Decision variable $x_{ij} = 1$ if facility $i$ is assigned to location $j$. 

The objective is to find an assignment of facilities to locations, in order to minimize the weighted flow.

$$
\sum_{i=1}^{n}\sum_{j=1}^{n}\sum_{k=1}^{n}\sum_{l=1}^{n} F_{ij} D_{kl} x_{ik} x_{jl}
$$ 

with: 

$\sum_{i=1}^{n} x_{ij} = 1$ and $j=1,n$, to express that a location hosts one facility

$\sum_{j=1}^{n} x_{ij} = 1$ and $i=1,n$, to express that a facility sits a one location $x_{ij} \in {0,1}$ 

For example, let's consider a Flow matrix: 
$\begin{pmatrix}
0 & 5 & 2\\
5 & 0 & 3\\
2 & 3 & 0
\end{pmatrix}$ 
and a Distance matrix: 
$\begin{pmatrix}
0 & 8 & 15\\
8 & 0 & 13\\
15 & 13 & 0
\end{pmatrix}$ 

Rewriting the assignment variables to use a single digit subscript:

$$(x_{11}, x_{12}, x_{13}, x_{21}, x_{22}, x_{23}, x_{31}, x_{32}, x_{33}) = (x_1, x_2, x_3, x_4, x_5, x_6, x_7,x_8, x_9)$$

The QAP becomes:

$$
\text{min} x_0 = 80 x_1 x_5 + 150 x_1 x_6 + 32 x_1 x_8 + 60 x_1 x_9 + 80 x_2 x_4 + 130 x_2 x_6 + 60 x_2 x_7 + 52 x_2 x_9 +
150 x_3 x_4 + 130 x_3 x_5 + 60 x_3 x_7 + 52 x_3 x_8 + 48 x_4 x_8 + 90 x_4 x_9 + 78 x_5 x_9 + 78 x_6 x_8
$$

subject to the assignment constraints:

$$
x_1 + x_2 + x_3 = 1, x_4 + x_5 + x_6 = 1, x_7 + x_8 + x_9 = 1\\
x_1 + x_4 + x_7 = 1, x_2 + x_5 + x_8 = 1, x_3 + x_6 + x_9 = 1
$$

Because QUBO stands for unconstrained, we should remove the assignment constraints. 

We can do so by converting the constraints into penalty terms, added to the objective function.

The penalty terms are the constraint equations, squared with a penalty factor $P$, sufficiently high to prevent constraints from being violated:

\begin{align*}
\text{min}~x_0 
&=80 x_1 x_5 + 150 x_1 x_6 + 32 x_1 x_8 + 60 x_1 x_9 + 80 x_2 x_4 + 130 x_2 x_6 + 60 x_2 x_7 + 52 x_2 x_9 \\
&+ 150 x_3 x_4 + 130 x_3 x_5 + 60 x_3 x_7 + 52 x_3 x_8 + 48 x_4 x_8 + 90 x_4 x_9 + 78 x_5 x_9 + 78 x_6 x_8 \\
&+ P (x_1 + x_2 + x_3 -1)^2 + P (x_4 + x_5 + x_6 -1)^2 + P (x_7 + x_8 + x_9 -1)^2 \\
&+ P (x_1 + x_4 + x_7 -1)^2 + P (x_2 + x_5 + x_8 -1)^2 + P (x_3 + x_6 + x_9 -1)^2
\end{align*}

But, Qiskit Algorithms library allows us to define the QUBO directly with the constraints.


### Linear Binary Optimization Problem example

In this example, we will solve an instance of a Linear Binary Optimization problem: the **Knapsack** problem.

As a reminder, this problem deals with a list of objects $i = 1, ..., n$, each with a weight $W_i$ and a value $C_i$.
The objective is to select a subset of objects, such that the total weight is less or equal to a limit W, yet maximizing the total value.
Let the binary variables $x_i$ indicate if object $i$ is selected ($x_i = 1$).

We wish to minimize: 

$$-\sum_{i=1}^{n} C_i x_i$$ 

with the constraint: 

$$\sum_{i=1}^{n} W_i x_i \leq W, \quad x_i \in {0,1} $$


For example, let's consider the following instance: Minimize: $-5 x_1 - 3 x_2 - 4 x_3$
Under the constraint: $3 x_0 + x_2 + x_3 \leq 3$

This problem does not fit the QUBO formulation.

To express it in a QUBO form, we: 
- rewrite the inequality constraint as an equality
- insert this equality constraint as a penalty term (as above)

To change the inequality, we add a slack binary variable(s). In this case, because we need to reach 3, we need to add 2 binary slack variables:

$3 x_1 + x_2 + x_3 \leq 3 \Leftrightarrow  3 x_1 + x_2 + x_3 + y_1 + 2 y_2 = 3$

To remove the constraint, we add the constraint expression to the objective function, with a penalty factor $P$ that avoids violating the constraint:

Minimize: $-5 x_1 - 3 x_2 - 4 x_3 + P (3 x_1 + x_2 + x_3 + y_1 + 2 y_2 - 3)^2$

The original objective function range is in $[-12, 0]$, so any violation of the constraint must result in a value greater than the maximum value, let's choose $P = 13$.


### **Graph Cut Definition**

A **graph cut** is a fundamental concept in graph theory that refers to a **partition of the vertices** of a graph into two disjoint subsets, separating the graph into two parts. The set of edges that have one endpoint in each subset is called the **cut set**, and the goal of different graph cut problems is often to minimize or maximize the number (or total weight) of these edges.

#### **Formal Definition**
Given an **undirected graph** $ G = (V, E) $, where $ V $ is the set of vertices and $ E $ is the set of edges, a **cut** is defined by a partition of $ V $ into two disjoint subsets $ S $ and $ \bar{S} $ such that:
- $ S \cup \bar{S} = V $
- $ S \cap \bar{S} = \emptyset $

The **cut-set** consists of all edges that have **one endpoint in $ S $ and the other in $ \bar{S} $**:
$$
\text{Cut}(S, \bar{S}) = \{ (u, v) \in E \mid u \in S, v \in \bar{S} \}
$$

### **Max-Cut Problem Formulation**

The **Max-Cut problem** is a fundamental problem in graph theory and combinatorial optimization. Given an **undirected graph** $ G = (V, E) $, where $ V $ is the set of vertices and $ E $ is the set of edges, the goal is to find a partition of the vertex set into two subsets $ S $ and $ \bar{S} $ that **maximizes the number (or total weight) of edges** that cross between the two subsets.

#### **Mathematical Formulation**
Let:
- $ x_i $ be a binary decision variable for each vertex $ i $, where:
  $$
  x_i =
  \begin{cases}
  1, & \text{if node } i \text{ is in subset } S \\
  0, & \text{if node } i \text{ is in subset } \bar{S}
  \end{cases}
  $$
- Each edge $ (i, j) \in E $ contributes to the cut **if and only if** its endpoints are in different subsets, meaning $ x_i \neq x_j $.

The **Max-Cut objective function** is given by:
$$
\max \sum_{(i, j) \in E} w_{ij} (x_i \oplus x_j)
$$
where:
- $ w_{ij} $ is the weight of edge $ (i, j) $ (assumed 1 in the unweighted case).
- $ \oplus $ represents the **XOR** operation, which equals 1 if $ x_i \neq x_j $ (i.e., the edge is cut).

### **Reformulation in Terms of the Ising Model**
To solve the Max-Cut problem using **quantum optimization**, we express it in terms of spin variables:
- Define $ s_i $ as a spin variable taking values in $ \{-1, +1\} $, where:
  $$
  s_i = 2x_i - 1
  $$
- The Max-Cut objective function can then be rewritten as:
  $$
  \max \frac{1}{2} \sum_{(i, j) \in E} w_{ij} (1 - s_i s_j)
  $$
  Since minimizing $ s_i s_j $ encourages opposite spins (partitioning nodes into different sets), this formulation maps directly to the **Ising model**.


### **Quadratic Unconstrained Binary Optimization (QUBO) Formulation**
Since QUBO problems use binary variables $ x_i \in \{0,1\} $, we express Max-Cut as:
$$
\max \sum_{(i, j) \in E} w_{ij} (x_i + x_j - 2x_i x_j)
$$
which is a **quadratic** function of binary variables.

This formulation enables solving Max-Cut using classical and quantum optimization techniques, such as **QAOA (Quantum Approximate Optimization Algorithm)** and **Simulated Annealing**.






















## Optimizing portfolios with QAOA

Adapted from: 

https://qiskit-community.github.io/qiskit-finance/tutorials/01_portfolio_optimization.html

### Introduction
Here we given an example of how the QAOA algorithm can be used to solve financial portfolio optimization problems.


#### The problem
We are given a set of $n$ assets, labeled as $\{1, 2, ..., n\}$, each with an expected return $\mu_i$.
In this simplified model, we have a binary choice: either choose an asset of do not.
We wish to find the maximum return:

$$ \max_{x_i} \left( \sum_i \mu_i x_i \right) = \min_{x_i} \left( - \sum_i \mu_i x_i \right)$$

where $x_i \in \{0, 1\}$ indicates a decision:
- $x_i=1$ we pick asset $i$
- $x_i=0$ don't pick asset $i$

However we also want to take risk into account, which can be done via the covariances between the assets $\sigma_{ij}$. Thus we want to minimize the following cost function:

$$C(\{x_i\}) = - \sum_i \mu_i x_i  + q\sum_{ij} \sigma_{ij} x_i x_j$$

where $q$ is the risk factor. 

To understand this cost function note that:
- For $q=0$ we just pick the best assests based on expected return
- For $q>0$ we take volatility into account
- For $q >>0$ we pick the assets with low variance and negative covariance

If $p_i^{(t)}$ is the price at time $t$ for asset $i$ then the return on investiment (ROI) at time $t$ is 

$$
\left(p_i^{(t+1)} - p_i^{(t)} \right) \over p_i^{(t)}
$$

To compute $\mu_i$ we calculate the mean ROI over the whole period and to compute $\sigma_{ij}$ we calculate the covariance between difference the ROIs of different assets over the whole period.

For more on the portfolio model, see [here](https://en.wikipedia.org/wiki/Modern_portfolio_theory).



### Create the Data
We will use randomly generated data using `RandomDataProvider`.
The number of assets `num_assets` will equal the number of qubits required in the quantum computation.

### Define Terms
If $p_i^{(t)}$ is the price at time $t$ for asset $i$ then define $d_i^{(t)}$ as the return at time $t$

$$ d_i^{(t)} = \frac{p_i^{(t+1)} - p_i^{(t)}}{p_i^{(t)}} $$

To compute $\mu_i$ we calculate the mean return over the whole period:

$$ \mu_i = \frac{1}{T} \sum_t d_i^{(t)} $$

and similarly for the covariance

$$ \sigma_{ij} = \frac{1}{T-2} \sum_t (d_i^{(t)} - \mu_i ) (d_j^{(t)} - \mu_j ) $$

where $T$ is the total number of timesteps.

### Classial Solution
Here we minimize the cost function by seacrhing through all the combinations of $\{x_i\}$. This scales as $2^{n}$.

### Quantum Solution

To map the problem to quantum computing, we change the variables to $x_i=(1-z_i)/2$, the problem transorm to -

```{admonition} Some algebra
:class: tip

$$
\begin{split}
C(\{z_i\}) &= \frac{1}{2} \sum_i (\mu_i z_i-\mu_i) + \frac{q}{4}\sum_{ij} (\sigma_{ij} - \sigma_{ij} z_j  - \sigma_{ij} z_i + \sigma_{ij} z_i z_j) \\
&= \frac{1}{4} \sum_i (q\sigma_{ii}-2\mu_i) + \frac{q}{4}\sum_{ij} \sigma_{ij} \\
&+ \frac{1}{4} \sum_i \left(2 \mu_i  - q\sum_{j} \sigma_{ij} - q\sum_{j} \sigma_{ji} \right) z_i + \frac{q}{4}\sum_{i \ne j} \sigma_{ij} z_i z_j  \\
&= \text{const} + \frac{1}{4}\left(\sum_i  c_i z_i + q\sum_{i \ne j} \sigma_{ij} z_i z_j \right) \\
\end{split}
$$
```

```{card}
Now we have the Quantum Hamiltonian:

$$
H = \sum_i  c_i Z_i + q\sum_{i \ne j} \sigma_{ij} Z_i Z_j
$$

where $c_i=2\mu - q\sum_j(\sigma_{ij} + \sigma_{ji})$.
```

Given an ansatz $|\psi(\theta)\rangle$ we minimize $\langle \psi(\theta)|H|\psi(\theta)\rangle$ to find our solution.

### With constraint

Suppose we have a budget $B \in N$ we must spend, i.e.

$$\sum_i x_i = B$$

We can force our quantum algorithm to find such solution by introducing a penalty term in the cost function

$$
C_{\text{penalty}} = \lambda \left(\sum_i x_i - B\right)^2
$$

Thus our Hamiltonian is now

```{card}
$$H = \sum_i  \left(c_i - \lambda \tilde n \right) Z_i + \sum_{i \ne j} \left(q\sigma_{ij}  + \frac{\lambda}{4} \right) Z_i Z_j  $$
```

where $\tilde n = \frac{n - 2B}{2}.$

#### Quantum Solution
We can force our quantum algorithm to find such solution by introducing a penalty term in the cost function

$$
\begin{split}
C_{\text{penalty}} &= \lambda \left(\sum_i x_i - B\right)^2 = \lambda \left( \tilde n - \frac{1}{2}\sum_i z_i\right)^2 \\
&= \lambda \tilde n ^2 + \frac{\lambda}{4}\sum_{ij} z_iz_j - \lambda \tilde n \sum_i z_i  \\
\end{split}
$$

where $\tilde n = \frac{n - 2B}{2}.$

Thus our Hamiltonian is now

$$H = \sum_i  \left(c_i - \lambda \tilde n \right) Z_i + \sum_{i \ne j} \left(q\sigma_{ij}  + \frac{\lambda}{4} \right) Z_i Z_j  $$



```{admonition} Tutorial notebook
:class: tip

The tutorial notebooks are in folder `exercise3` of the [QTrain](https://github.com/ICHEC/QTrain/tree/main) repository.

- Notebook link 1: https://github.com/ICHEC/QTrain/blob/main/exercise3/SCynergy-QAOA.ipynb
- Notebook Link 2: https://github.com/ICHEC/QTrain/blob/main/exercise3/finance_qubo.ipynb
```

## Acknowledgements

We extend our gratitude to the Irish Centre for High-End Computing (ICHEC) and University of Galway for providing computing and for all-encompassing invaluable support. This project was funded by the **EuroHPC JU** under **grant agreement No 951732** and Ireland.

``````{card}
```{image} ../logos/logos.png
:width: 100%
```
``````


### References

[1] [A. Lucas, *Ising formulations of many NP problems*, Front. Phys., 12 (2014).](https://arxiv.org/abs/1302.5843)

[2] [F. Glover, et al., *Quantum bridge analytics I: a tutorial on formulating and using QUBO models*, Annals of Operations Research 314.1 (2022): 141-183.]
