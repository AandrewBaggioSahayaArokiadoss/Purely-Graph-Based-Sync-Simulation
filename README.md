# Graph-Theoretic Synchronization — Interactive Notebook

An interactive implementation of the constructive coupling-weight allocation method in [Arokiadoss & Arunkumar, *Physica A* (2026)](https://doi.org/10.1016/j.physa.2026.131508). It uses directed graph structure—**not an LMI solver**—to calculate positive coupling weights and simulate identical nonlinear oscillators.

![Example: generated network and synchronization error](assets/preview.png)

## Quick start

1. Open **`graph_theoretic_sync_interactive.ipynb`** in Jupyter or Google Colab.
2. Run its cells from top to bottom. If packages are missing, uncomment the installation line in the setup cell.
3. In the dashboard, choose an oscillator and graph, then click **Validate & simulate**.
4. Optionally click **Export CSV + PNG**. Files are saved to `output/`.

Requires Python 3.9+ and `numpy`, `scipy`, `networkx`, `matplotlib`, `ipywidgets`, and `IPython`. The notebook is **self-contained**—no external Python module is needed.

## Inputs

| Choice | How it works |
|---|---|
| **Dynamics** | Lorenz, Chua, piecewise-linear Rössler-like, or **User-defined** |
| **User-defined dynamics** | Edit `custom_dynamics(t, X)` and `CUSTOM_INITIAL_STATE` in the clearly marked cell. Choose **User-defined** in the dashboard and enter **the diagonal of `P`** (e.g., `1,0,1`) and **`a > 0`**. |
| **Random graph** | Generates a random directed graph known to contain an out-arborescence. |
| **Paste edges** | Enter `tail,head` on each line, e.g., `0,1` followed by `1,2`. |
| **Upload CSV** | Upload a two-column `tail,head` edge list (optional header). An example is supplied as `example_edges.csv`. |

For pasted/uploaded graphs, set the **number of vertices N**; labels must be integers `0` through `N−1`. An absent edge does not imply an absent vertex. Self-loops are not supported.

**Graph safety check:** A directed graph has a spanning **out-arborescence** if and only if its condensation digraph has exactly **one source strongly connected component**. The notebook checks this first. If the condition fails, it prints a warning and **stops before allocating weights**.

## Mathematical assumption

For identical node dynamics `f`, the method assumes there exist `a>0`, `c>0`, and a diagonal **0–1** matrix `P` such that

\[
(x-y)^\top\bigl[f(x)-f(y)-aP(x-y)\bigr]\le -c\|x-y\|^2.
\]

This **QUAD condition must hold globally** (or on the domain justified by the theorem). The notebook supplies literature-based `P` and `a` for the three presets; for a custom system **you must justify `P`, `a`, and the existence of `c>0`**. It does **not** establish that condition numerically. The included custom example is a contracting linear system with `P=diag(1,1)`, `a=1`, `c=2`.

The algorithm assigns positive weights to directed edges using SCC decomposition, root-to-vertex shortest paths, positive cycle-space circulations, and incoming-edge rules. It then checks the assigned weights and internal imbalance inequalities.

## Outputs

The notebook shows the directed graph and synchronization-error curves (distances along a spanning out-arborescence, including their mean and maximum). The **Export** button creates:

- `directed_edges.csv` — `tail,head,weight`
- `weighted_adjacency.csv` — receiving-vertex rows, sending-vertex columns
- `tree_edge_distances.csv` — distances along tree edges over time
- `synchronization.png` — network and error plots

### Scope and limitations

- The interactive runner supports the three built-in **three-dimensional** oscillators and vectorized custom models of **any state dimension**.
- Numerical decay over a finite horizon illustrates synchronization; it is not a substitute for verifying the paper's theoretical assumptions.
- In particular, large positive weights can lead to stiff numerical integration.
- This is a research demonstration, not a certified black-box synchronizer for arbitrary dynamics.

## Reference

A. B. S. Arokiadoss and G. Arunkumar, *A (purely) graph-theoretic approach to synchronization of nonlinear dynamical networks*, *Physica A* **692** (2026), 131508. https://doi.org/10.1016/j.physa.2026.131508
