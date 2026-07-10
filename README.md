# Synchronizing Nonlinear Dynamical Networks Using Purely Graph-Theoretic Tools

This project presents an algorithm for synchronizing a network of identical nonlinear dynamical systems over a digraph graph. Instead of solving Lyapunov inequalities or LMIs, the method assigns coupling strengths combinatorially by analyzing directed cycles and directed trees.

The notebook, `pure-graph-sync.ipynb`, generates a random directed graph, applies the coupling-strength assignment algorithm, and numerically simulates the resulting coupled network. The results show that neighboring system trajectories converge toward each other over time, indicating synchronization.

## Overview

This algorithm is from the paper "A (purely) graph-theoretic approach to synchronization of nonlinear dynamical networks". The main idea is that if the graph contains a directed spanning tree and the system dynamics satisfy the required QUAD condition, then synchronization can be achieved without solving matrix inequalities.

## Key Idea

The algorithm works by:

- Building a directed graph with a root strongly connected component.
- Assigning arc weights using graph-theoretic rules.
- Constructing the weighted graph Laplacian.
- Simulating the coupled nonlinear system.
- Verifying synchronization through pairwise distance decay.

## When the Method Works

The method applies when two conditions hold:

1. **QUAD condition on the system dynamics.**  
   The uncoupled dynamics must satisfy a quadratic inequality of the form

   \[
   (x-y)^T(f(x,t)-f(y,t)) \le (x-y)^T(aP-c)(x-y)
   \]

   for some diagonal positive-definite matrix \(P\), scalar \(a\), and constant \(c > 0\).

2. **Directed spanning tree in the network.**  
   The graph must contain a directed spanning tree, meaning there is a root vertex or root strongly connected component from which every other vertex is reachable by a directed path.

These conditions are sufficient for the coupling-strength assignment method used in the notebook.

## What the Notebook Does

The notebook performs the following steps:

1. **Generates a random directed graph** with one root strongly connected component.
2. **Assigns coupling strengths** to the graph using the proposed graph-theoretic rule.
3. **Defines the Lorenz oscillator** as the system dynamics.
4. **Simulates the full coupled system** using numerical ODE integration.
5. **Plots the network topology** as the first figure.
6. **Plots pairwise distances over time** as the second figure.
7. **Exports results** to CSV and Excel files for further analysis.

## Files Generated

The notebook produces these outputs:

- `network.svg` — visualization of the directed graph.
- `pairwise_distances.png` — plot showing synchronization behavior.
- `adjacency_matrix.csv` — weighted adjacency matrix used in the simulation.
- `pairwise_distances.xlsx` — time-series of pairwise distances.

## Requirements

Install the following Python packages:

```text
numpy
scipy
pandas
matplotlib
networkx
openpyxl
```

## How to Run

Open `pure-graph-sync.ipynb` in Google Colab or Jupyter and run the cells from top to bottom.

- The graph is generated randomly.
- The simulation runs numerically.
- The plots appear inline.
- The final cell exports the data files.

## Using a Different Dynamical System

To adapt the notebook for another nonlinear system:

1. Replace the Lorenz oscillator with your own vector field.
2. Derive the corresponding QUAD parameters \(P\) and \(a\).
3. Update the simulation setup with those values.
4. Keep the graph-theoretic coupling assignment unchanged.

This makes the approach broadly reusable for other nonlinear systems that satisfy the same structural condition.

## Results

The main empirical result is that the pairwise distances between neighboring system trajectories converge toward zero over time. This indicates that the network achieves synchronization using topology-based coupling assignment rather than inequality solving.

## Why This Project Matters

This project combines nonlinear dynamics, directed graph theory, and synchronization analysis in a way that is both mathematically interesting and computationally practical. It shows that network structure alone can be used to design coupling strengths that synchronize complex systems.

## Repository Structure

```text
pure-graph-sync/
├── pure-graph-sync.ipynb
├── README.md
└── output/
    ├── network.svg
    ├── pairwise_distances.png
    ├── adjacency_matrix.csv
    └── pairwise_distances.xlsx
```

## Suggested Resume Description

You can describe this project on your resume as:

**Developed a graph-theoretic synchronization algorithm for nonlinear dynamical networks on directed graphs and demonstrated convergence using simulated coupled Lorenz oscillators.**

If you want, I can next turn this into:
- a more formal academic README,
- a shorter recruiter-friendly README,
- or a polished resume bullet list.
