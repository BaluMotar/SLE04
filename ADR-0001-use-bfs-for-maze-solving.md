# ADR-0001: Use Breadth-First Search (BFS) as the Preferred Algorithm for Maze Solving

- **Status:** Accepted (for the project design)
- **Date:** 14 October 2026
- **Author:** Balu Tukaram Motar (PRN 25UAM022, Division A)

## 1. Context
The Maze Solving System searches for a route from a start position to a goal position. The project compares
Breadth-First Search (BFS) and Depth-First Search (DFS). Both explore reachable positions but in different orders.
When every move has the same cost, finding a shortest path is a useful goal. The project also records performance
measurements and uses a modular design that separates input, search, visited-state tracking and output.

## 2. Decision
BFS is the preferred algorithm when the maze uses equal-cost moves and the shortest path is required.
DFS is retained as a comparison algorithm so its traversal behaviour and performance can be evaluated.

## 3. Alternatives Considered
| Option | Summary |
| --- | --- |
| DFS | Explores deeply along one route before backtracking. Can find a path but does not guarantee the shortest. |
| BFS | Explores level by level. Guarantees a shortest path when every move has equal cost. |
| A* | Can guide exploration with a heuristic, but needs extra design and validation of the heuristic and a priority-based frontier. |

## 4. Rationale / Justification
- BFS matches the requirement of finding a shortest path in an unweighted maze.
- BFS explores all positions at one distance before moving farther away, which supports its shortest-path guarantee.
- The recorded SLE-2 benchmark reported about 0.002005 ms/search for BFS and 0.002842 ms/search for DFS, with 11 nodes
  reported for each algorithm. These figures are specific to the recorded test environment, must be checked against the
  final SLE-2 report or rerun before the viva, and are not universal performance values.
- In that run BFS had the lower average time, but one small test cannot prove BFS is always faster than DFS.
- The SLE-3 C4 design separates the Input Module, Search Engine, Visited Memory and Output Module, which makes the
  algorithm easier to understand and test.
- Inside the search design the frontier, visited set, goal test and path reconstruction are kept distinct.
- This modular structure makes it easy to compare BFS and DFS while reusing common maze input and output handling.

## 5. Consequences
**Positive**
- Finds a shortest path for equal-cost moves.
- Makes the reason for algorithm selection clear and testable.
- Supports a fair BFS-versus-DFS comparison within the same architecture.

**Negative / trade-offs**
- BFS can use more memory because it stores many frontier positions.
- Performance depends on maze size, layout, implementation and test environment.
- For weighted paths, ordinary BFS is not enough to find the minimum-cost route.

## 6. Supporting evidence in this repository
Run `python src/compare.py` and `pytest` (or the test file directly). Path lengths below are deterministic:

| Maze | BFS steps | DFS steps | Meaning |
| --- | --- | --- | --- |
| `maze1.txt` (single corridor route) | 23 | 23 | Only one route, so both agree |
| `maze3_bfs_vs_dfs.txt` | 12 | 28 | DFS took a much longer route |
| `maze4_open_room.txt` | 10 | 34 | DFS wandered through the open room |

Timing in this repo's code is machine-dependent, and on some mazes DFS ran slightly faster than BFS. That matches the
ADR's own warning that one small timing test does not prove BFS is always faster. The reason to prefer BFS is the
**shortest-path guarantee**, not speed.
