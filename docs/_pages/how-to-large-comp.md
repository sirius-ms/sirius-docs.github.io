---
permalink: /faq/how-to-large-comp
title: "How to deal with high mass compounds?"
---

## Configuring SIRIUS for Large Datasets with High-Mass Compounds {#large-compounds}

When analyzing large datasets with SIRIUS, particularly those containing high-mass compounds, a few compounds can consume most of the computing time. 
In some cases, these may not finish in a reasonable timeframe, potentially blocking your entire analysis.

The most straightforward solution is to exclude high-mass compounds from the analysis 
by setting a mass threshold.
But since many high-mass compounds could actually be processed efficiently, it would be unfortunate to leave them out. **A better approach is to set a per-compound timeout (`--compound-timeout`) to prevent a few challenging cases from halting your analysis.**

## Causes of Prolonged Computation Times {#causes}

There are two primary reasons why molecular formula identification for specific compounds may take an unusually long time:

**1. Extremely high number of candidates for the given precursor mass**

This is the most common reason for extended computation times.

Possible Solutions:

- **Restricting candidates to databases.** If your interest lies solely in molecular structure database hits, consider restricting candidates to those present in the database. This approach usually resolves running time issues. However, be aware that this may exclude the correct novel molecular formula for some compounds. The confidence score should help identify such cases. **This method is not recommended if you are interested in [CANOPUS]({{ "/methods-background/#CANOPUS" | relative_url }}) results.**
- **Set a per-compound timeout** (**not** per-tree) to prevent a few problematic cases from delaying the entire analysis. Use the `--compound-timeout` option in the CLI or check the [advanced settings in the GUI]({{"/gui/#SIRIUS-advanced" | relative_url}}).

**2. Optimizing the fragmentation tree for a candidate can be time-consuming, especially as the tree size increases. In rare cases, this process may take an unreasonably long time.**

Possible solutions:
- **Use a commercial ILP solver.** While multithreading with the integrated CLP solver is effective, using commercial solvers like Gurobi or CPLEX can significantly speed up processing, particularly for *hard cases*.
- **Use heuristic algorithms.** For compounds above a certain mass, enable `heuristic-algorithm-only` mode (available since SIRIUS version 4.7.0). While the resulting tree may not be optimal for some instances, it is usually sufficient for CSI:FingerID, and a heuristic tree is better than no tree.
- **Set timeouts.** Set a timeout per-compound (`--compound-timeout`) or per-tree (`--tree-timeout`) to prevent specific cases from stalling the analysis. You can find the timeout parameters in the [advanced settings in the GUI]({{"/gui/#SIRIUS-advanced" | relative_url}}).

**Note:** Including many "rare" elements in the analysis can also dramatically increase the number of formula candidates, leading to extended computation times even for compounds with lower masses.