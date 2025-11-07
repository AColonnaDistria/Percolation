# Percolation $Z^2$ Simulator

This project (under the supervision of Fu-Hsuan Ho for 3rd year of math bachelor) simulates the concept of **Percolation** on a 2D square lattice ($\mathbb{Z}^2$) and visualizes the resulting connected clusters using the **Pygame** library. The simulation identifies the **largest connected component** (the spanning cluster) and highlights it in a distinct color.

---

## ⚙️ How it Works

The core of the simulation is based on a grid where connections (or 'bonds') between adjacent sites are randomly present or absent based on a given probability $p$.

1.  **Initialization (`initialize`):** A $N \times N$ grid (`network`) is created. Each cell $[i][j]$ stores information about its four possible connections (bonds) to its neighbors: `[right, down, left, up]`, plus a marker for connected component identification (`[..., mark]`). Initially, all bonds are 'open' (represented by `0` in the provided code, though the `delete` function seems to *set* to `1` which is confusing, see **Note** below).
2.  **Percolation (`percolate`):** This function randomly 'deletes' (or **closes**) bonds between neighboring sites with a given probability $p$. This process forms the random network.
3.  **Cluster Identification (BFS):**
    * `bfs_subiterative` performs a **Breadth-First Search (BFS)** starting from an unvisited site to find all sites belonging to the same connected component. It marks these sites with a unique ID (`mark`) and counts the size of the cluster.
    * `bfs_iterative` iterates over the entire grid, running `bfs_subiterative` on every unvisited site to find **all** connected components. It tracks and returns the ID (`maxid`) of the largest component found.
4.  **Visualization (`show`):** The `pygame` window draws the grid. Each connected component is assigned a random color, *except* for the **largest component (`maxid`), which is colored bright red** (255, 0, 0).

> **Note on `delete`:** The `delete` function seems to set the bond status to `1`, which is then checked in the `neighbours` function: `if (state[x][y][direction] == 1):`. This implies that in this code, a value of **`1` means the bond is present/open**, and a value of **`0` means the bond is absent/closed**. The name `delete` is slightly misleading based on this logic, as it *creates* the connection. Assuming `1` = connected/open bond.

---

## Prerequisites

To run this simulation, you need **Python** and the **Pygame** library.

```bash
pip install pygame
```

```bash
python percolationWindow.py
```

You can easily adjust the simulation parameters at the end of the script:
Variable	Description	Default Value
N	The size of the square lattice (N×N).	100
probability	The bond occupation probability p. This is the chance that a bond is deleted/closed in the percolate function.	0.5
display	The size of the display window (set to 700x700).	700

Key Concept: Critical Probability (pc​)

For an infinite 2D square lattice, the critical probability pc​ for bond percolation is 0.5.

    If the bond occupancy probability is p<0.5, only small clusters form.

    If the bond occupancy probability is p>0.5, a large, system-spanning cluster (the percolating cluster) is likely to form.

In this code, the probability variable represents the chance that a bond is closed. Therefore:

    If you set probability to a low value (e.g., 0.2), the network will be highly connected and a large red cluster will dominate.

    If you set probability to a high value (e.g., 0.8), the network will be fragmented and only small, colorful clusters will appear.

Try setting the probability close to the critical value 0.5 to observe the transition.
