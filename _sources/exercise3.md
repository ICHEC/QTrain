# 3. Quantum Optimization


## Portfolio Optimization

Adapted from: 

https://qiskit-community.github.io/qiskit-finance/tutorials/01_portfolio_optimization.html

We are given a set of $n$ assets $\{1, 2, ..., n\}$, each with an expected return $\mu_i$.
In this simplified model, we have a binary choice: either choose an asset of do not.
We wish to find the maximum return:
$$ \max_{x_i} \left( \sum_i \mu_i x_i \right) = \min_{x_i} \left( - \sum_i \mu_i x_i \right)$$
where $x_i \in \{0, 1\}$ indicates a decision:
- $x_i=1$ pick asset $i$
- $x_i=0$ don't pick asset $i$

However we also want to take risk into account, which can be done via the covariances between the assets $\sigma_{ij}$.
Thus we want to minimize the following cost function:
$$C(\{x_i\}) = - \sum_i \mu_i x_i  + q\sum_{ij} \sigma_{ij} x_i x_j$$
where $q$ is the risk factor. 

To understand this cost function note that:
- For $q=0$ we just pick the best assests based on expected return
- For $q>0$ we take volatility into account
- For $q >>0$ we pick the assets with low variance and negative covariance

For more on the portfolio model see [here](https://en.wikipedia.org/wiki/Modern_portfolio_theory).


## Create the Data
We will use randomly generated data using `RandomDataProvider`.
The number of assets `num_assets` will equal the number of qubits required in the quantum computation.

## Define Terms
If $p_i^{(t)}$ is the price at time $t$ for asset $i$ then define $d_i^{(t)}$ as the return at time $t$
$$ d_i^{(t)} = \frac{p_i^{(t+1)} - p_i^{(t)}}{p_i^{(t)}} $$

To compute $\mu_i$ we calculate the mean return over the whole period:
$$ \mu_i = \frac{1}{T} \sum_t d_i^{(t)} $$
and similarly for the covariance
$$ \sigma_{ij} = \frac{1}{T-2} \sum_t (d_i^{(t)} - \mu_i ) (d_j^{(t)} - \mu_j ) $$
where $T$ is the total number of timesteps.

## Classial Solution
Here we minimize the cost function by seacrhing through all the combinations of $\{x_i\}$. This scales as $2^{n}$.

## Quantum Solution

To Solve on a QC, we change to $x_i=(1-z_i)/2$
$$
\begin{split}
C(\{z_i\}) &= \frac{1}{2} \sum_i (\mu_i z_i-\mu_i) + \frac{q}{4}\sum_{ij} (\sigma_{ij} - \sigma_{ij} z_j  - \sigma_{ij} z_i + \sigma_{ij} z_i z_j) \\
&= \frac{1}{4} \sum_i (q\sigma_{ii}-2\mu_i) + \frac{q}{4}\sum_{ij} \sigma_{ij} + \frac{1}{4} \sum_i \left(2 \mu_i  - q\sum_{j} \sigma_{ij} - q\sum_{j} \sigma_{ji} \right) z_i + \frac{q}{4}\sum_{i \ne j} \sigma_{ij} z_i z_j  \\
&= \text{const} + \frac{1}{4}\left(\sum_i  c_i z_i + q\sum_{i \ne j} \sigma_{ij} z_i z_j \right) \\
\end{split}
$$
Now we have the Quantum Hamiltonian:
$$H = \sum_i  c_i Z_i + q\sum_{i \ne j} \sigma_{ij} Z_i Z_j $$

Given an ansatz $|\psi(\theta)\rangle$ we minimize $\langle \psi(\theta)|H|\psi(\theta)\rangle$ to find our solution.

## With constraint

Suppose we have a budget $B \in N$ we must spend, i.e.
$$\sum_i x_i = B$$

## Quantum Solution
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
