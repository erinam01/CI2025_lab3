# CI2025_lab3

## Implementation
This code implements the SSP (Shortest-Path problem) for all possible source–target node pairs in randomly generated graphs. This notebook aims evaluate different pathfinding algorithms under various conditions. 

The main challenge was implementing an efficient algorithm for both positive-only graphs and graphs containing negative weights.
- For graphs with **negative edges** (for which A* is not applicable) , no efficient optimization of Bellman–Ford alone was available. To address this, `Johnson’s` algorithm (adapted from here: https://www.geeksforgeeks.org/dsa/johnsons-algorithm-for-all-pairs-shortest-paths-implementation/) was used.
- As for graphs with **positive-only edges**, the `A*` algorithm was chosen due to its strong performance.
    - Given that the problem was given as an adjacency matrix, and not with spatial coordinates, the computation for the heuristic `h(s)` is not trivial; Johnson's potentials were adapted to an actual heuristic function, both for simplicity of code text and to avoid extra computations. The results of this attempt were then analyzed.

For larger graphs (size>200), running A* with Johnson’s preprocessing becomes computationally too expensive, so a simpler `Greedy Best-First Search (GBFS)` algorithm was employed instead. GBFS does not guarantee optimal paths, but it provides a practical compromise between accuracy and computational cost.
For very large graphs (size>=500), instead of computing all possible node-pairs (grows quadratically), a random sample of 150 source–target pairs was used.

The optimal shortest path for each source–target pair was obtained using *NetworkX’s* built-in functions (`shortest_path` or `bellman_ford_path`). *NetworkX* was also used to detect negative-weight cycles, ensuring that any graph containing such cycles was skipped before running the experiments.

The project records execution times  and success rates for each algorithm.

**NOTES**: 
- The final output may contain very small or zero `time` values due to decimal rounding and the fast execution o small graphs.
- Johnson potentials are computed once per graph, and reweighted graphs are reused to avoid repeated expensive computations.
- The print `Computing graphs with A* for s<500 and GBFS for s>=500` is wrong, as it should be 200 instead

## Results
 - `A*` is extremely fast, about 1.5 ms per query on average.
 - `GBFS` is slower than `A*` (about 8 ms per query), likely because graphs are larger, and even though `GBFS` is simpler, the larger size dominates
 - the `avg_reweight_pct` indicates the percentage of total execution spent on Johnson’s reweighting for negative graphs. 148% is very high; in large part the time complexity for negative graphs is dominated by the reweighting process
 - `A*` almost always finds the exact shortest path (>99%). GBFS is less accurate, as predicted (interrestingly in my case it performed better for negative-edges graphs; I theorize it's because of Johnson's reweighting process, though it could also be just due to the random creation of the graphs. More test runs on different seeds should be run to check).


 Implemented also using LLM's for better optimization and assisted code review.

