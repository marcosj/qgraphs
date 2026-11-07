# qgraphs: VQE solver for the Induced Subgraph Isomorphism Problem

A variational quantum computing framework designed to solve the **induced subgraph isomorphism problem** ($H \subseteq G$) using a hybrid quantum-classical Variational Quantum Eigensolver (VQE) architecture, featuring problem-tailored QUBO-to-Ising mappings and equivariant ansatze. Developed for **QAMP 2025**.

## 🔬 Core Methodology & Pipeline

The pipeline reduces subgraph isomorphism to a maximum clique variation via modular product graphs, maps it to a constrained Hamiltonian, and optimizes it using symmetry-preserving quantum circuits.

```mermaid
graph TD
    Sub["Induced Subgraph Isomorphism Problem H ⊆ G"] --> Red["Reduction to Clique Problem / Modular Product Graph"]
    
    %% Clean parallel split from Reduction
    Red --> Ham["Hamiltonian: QUBO Formulation H_tot = H_A + H_B"]
    Red --> Ansatz["Equivariant Ansatz"]
    
    %% Left Branch: Hamiltonian & Ising
    Ham --> Ising["Ising Formulation: x_i = (1 - Z_i) / 2"]
    
    %% Right Branch: Ansatz Pipeline
    Ansatz --> Dicke["Pseudo-Dicke State Preparation"]
    Dicke --> Layers["L Layers: Phase Separator & Symmetry Preserving Mixer"]
    
    %% Convergence to Estimator / VQE loop
    Ising --> Est["Estimator"]
    Layers --> Est
    
    Est --> Opt["Classical Optimizer"]
    Opt -- "θ" --> Est
    Opt -- "θ̂" --> Sampler["Sampler"]
    
    Sampler --> Check{"E_0 == 0?"}
    Check -- "No" --> Ham
    Check -- "Yes" --> Found([Isomorphism Found])

    style Sub fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Red fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Ham fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Ising fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Ansatz fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Dicke fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Layers fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Est fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Opt fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Sampler fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Check fill:#161b22,stroke:#30363d,color:#c9d1d9
    style Found fill:#161b22,stroke:#30363d,color:#c9d1d9
```

![flowchart](iso.png)

## 🛠️ Key Architectural Components

* **Problem Reduction:** Formulates graph matching via modular product graphs and maps constraints into Quadratic Unconstrained Binary Optimization (QUBO).
* **Ising Transformation:** Converts binary decision variables to Pauli-Z operators via $x_i = \frac{1 - Z_i}{2}$.
* **Equivariant Ansatz:** Implements efficient search-space restriction using Pseudo-Dicke state preparation followed by parameterized layers of phase separators and symmetry-preserving mixers.
* **Hybrid Optimization Loop:** Iteratively minimizes expectation value $E_0$ via classical optimizers until the zero-energy ground state confirms isomorphism.

## 📚 References & Literature

*List of foundational papers and resources utilized in this implementation:*

### Ising & QUBO
- Lucas, A. (2014). *Ising formulations of many NP problems* [DOI](https://arxiv.org/abs/1302.5843)
- Zick, K. (2015). *Experimental quantum annealing: case study involving the graph isomorphism problem* [DOI](https://doi.org/10.1038/srep11168)
- Calude, C. (2017). *QUBO formulations for the graph isomorphism problem and related problems* [DOI](http://dx.doi.org/10.1016/j.tcs.2017.04.016)
- Huang, N. (2020). *A New QUBO Objective Function For Solving the Maximum Common Subgraph Isomorphism Problem Via Quantum Annealing* [DOI](https://doi.org/10.1007/s42979-020-00431-5)

### EQC
- Skolik, A. (2023). *Equivariant quantum circuits for learning on weighted graphs* [DOI](https://doi.org/10.1038/s41534-023-00710-y)
- Mernyei, P. (2022). *Equivariant Quantum Graph Circuits* [DOI](https://doi.org/10.48550/arXiv.2112.05261)
