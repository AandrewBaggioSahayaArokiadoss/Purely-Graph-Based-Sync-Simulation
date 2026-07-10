# Synchronizing Nonlinear Dynamical Networks using Purely Graph Theoretic Tools

This project demonstrates a coupling-strength assignment algorithm for
synchronizing a network of identical dynamical systems connected over a
**directed** graph. Unlike conventional approaches, the algorithm does not
require solving any matrix (Lyapunov/LMI) inequalities. Instead, it works
combinatorially — by locating directed cycles and constructing a spanning
tree of the network — giving it a computational complexity of **O(n·(m−n))**,
where `n` is the number of vertices and `m` is the number of arcs (edges).

The notebook (`pure-graph-sync.ipynb`) builds a random
directed graph, applies the algorithm, and shows numerically that the
distance between every pair of neighboring nodes' state trajectories
converges to zero — i.e., the network synchronizes.

## Conditions for the Algorithm to Apply

The method is guaranteed to work whenever two conditions hold:

1. **QUAD property** — each node's uncoupled dynamics `f(x, t)` must satisfy
   a quadratic inequality of the form

   `(x − y)ᵀ(f(x,t) − f(y,t)) ≤ (x − y)ᵀ (aP-c) (x − y)`

   for some diagonal positive-definite matrix `P` and scalar `a`. This is a
   mild, commonly-satisfied condition — the Lorenz system, Chua's circuit,
   and many other chaotic oscillators satisfy it.

2. **Directed spanning tree** — the network topology must contain at least
   one directed spanning tree, i.e., a "root" node (or root strongly
   connected component) from which every other node is reachable by a
   directed path. This is the weakest possible connectivity requirement for
   synchronization — much weaker than requiring the graph to be strongly
   connected or undirected.

If a dynamical system other than the Lorenz oscillator is used, its own
QUAD parameters `(P, a)` must be derived and supplied in place of the ones
computed here.

## What the Notebook Does

1. **`generate_one_root_scc(n)`** — builds a random digraph on `n` nodes
   that contains a directed cycle (the root SCC) which reaches every other
   node, guaranteeing a directed spanning tree exists.
2. **`sync_coupling_assign(G, weight)`** — assigns the coupling strength
   (derived from the Lorenz QUAD parameters) to every edge in the graph.
3. **`lorenz_oscillator`** — the uncoupled node dynamics (vectorized across
   all `N` nodes at once).
4. **`coupled_dynamics` / `simulate_coupled_systems`** — assembles the
   graph Laplacian from the weighted adjacency matrix and integrates the
   full coupled network ODE with `scipy.integrate.solve_ivp`.
5. **Figure 1 — Network Plot**: visualizes the random digraph.
6. **Figure 2 — Pairwise Distances**: plots ‖xᵢ(t) − xᵢ₊₁(t)‖ over time for
   all neighboring node pairs. Convergence of every curve to zero is the
   empirical signature that the network has synchronized.
7. Exports the adjacency matrix (`adjacency_matrix.csv`) and the pairwise
   distance time series (`pairwise_distances.xlsx`) for further analysis.

## Requirements

```
numpy
scipy
pandas
matplotlib
networkx
openpyxl
```

## How to Run

Open `random_digraph_synchronization.ipynb` in Google Colab (or Jupyter)
and run the cells top to bottom. Plots render inline; no files are written
until the final "Export Data" cell.

## Using a Different Dynamical System

To synchronize a network of systems other than the Lorenz oscillator:

1. Replace `lorenz_oscillator` with your system's vector field, vectorized
   over all `N` nodes (shape `(state_dim, N)`).
2. Derive the QUAD parameters `P` (diagonal, positive-definite) and `a` for
   your system.
3. Replace the `sigma, rho, beta` / `a` computation in the "Run the
   Simulation" cell with your derived values, and pass your matrix `P` into
   `simulate_coupled_systems`.

No other part of the algorithm changes — the spanning-tree-based coupling
assignment (`generate_one_root_scc` + `sync_coupling_assign`) is agnostic
to the specific node dynamics, as long as the QUAD property holds.

## Output

- `network.svg`-style plot (shown inline) — the random digraph topology.
- Pairwise-distance plot (shown inline) — convergence to zero demonstrates
  synchronization achieved purely from topology (spanning tree) + QUAD
  parameters, with no matrix inequalities solved.
- `adjacency_matrix.csv` — the weighted adjacency matrix used in the run.
- `pairwise_distances.xlsx` — pairwise distance values at every simulated
  time step.
