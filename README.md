# SLE-4: Final Viva & Architecture Decision Record (ADR)

**Course:** 02AML204 – Introduction to Artificial Intelligence
**Name:** Balu Tukaram Motar | **PRN:** 25UAM022 | **Division:** A | **Date:** 14 October 2026

## Summary
This repository documents one design decision for a maze-solving system:

> **Use Breadth-First Search (BFS) as the preferred algorithm** when every move costs the same and a shortest path
> is required. Keep Depth-First Search (DFS) as a comparison algorithm.

It also contains the SLE journey summary, the AI contribution note, viva preparation, and small code/tests that
back up the claims in the ADR.

## Quick start
Requires Python 3.8+ (no extra packages).

```bash
git clone https://github.com/<your-username>/<this-repo>.git
cd <this-repo>

python src/compare.py          # steps, nodes expanded and timing for BFS / DFS / A* on every maze
python tests/test_adr_claims.py   # (see note) or: pip install -r requirements-dev.txt && pytest
```
Solve a single maze and draw the path:
```bash
PYTHONPATH=src python -m maze_solver.main mazes/maze4_open_room.txt --algo bfs     # Linux / macOS
$env:PYTHONPATH="src"; python -m maze_solver.main mazes/maze4_open_room.txt --algo dfs   # Windows PowerShell
```
Maze format: `#` wall, `.` open, `S` start, `G` goal.

## What the evidence shows
| Maze | BFS steps | DFS steps |
| --- | --- | --- |
| `maze1.txt` (one route) | 23 | 23 |
| `maze3_bfs_vs_dfs.txt` | 12 | 28 |
| `maze4_open_room.txt` | 10 | 34 |

BFS never returned a longer path than DFS. Timing is machine-dependent, and DFS is sometimes slightly faster, so the
reason to choose BFS is the **shortest-path guarantee**, not speed.

## Repository structure
```
.
├── README.md
├── LICENSE
├── requirements.txt / requirements-dev.txt / pytest.ini
├── docs/
│   ├── ADR-0001-use-bfs-for-maze-solving.md   # Part A – the decision record
│   ├── adr-index.md                            # list of ADRs + template
│   ├── sle-journey.md                          # Part B – SLE-1 → SLE-4 summary
│   ├── ai-contribution-note.md                 # Part C – AI use, stated honestly
│   ├── viva-prep.md                            # Part D – viva notes and Q&A
│   ├── ARCHITECTURE.md + diagrams/             # C4 summary from SLE-3
├── src/
│   ├── compare.py                              # evidence script
│   └── maze_solver/                            # node, maze_input, heuristic, search, output, main
├── mazes/                                      # test mazes
├── tests/test_adr_claims.py                    # tests for the ADR's claims
└── submission/SLE4_25UAM022_BaluMotar.docx     # the submitted Word file
```

## SLE journey
| SLE | Focus |
| --- | --- |
| 1 | Code – maze search with BFS/DFS (https://github.com/BaluMotar/AI-Agent) |
| 2 | Performance – profiling and comparing algorithms |
| 3 | Architecture – full C4 diagrams |
| 4 | Decision + viva – this repository |

## AI contribution
AI tools (ChatGPT, Claude) were used as drafting and learning aids. All content must be checked against the actual
code, SLE-2 report and SLE-3 diagrams. See [docs/ai-contribution-note.md](docs/ai-contribution-note.md).

## Limitations
- The SLE-2 timing values quoted in the ADR come from a recorded run and are specific to that environment.
- Ordinary BFS does not handle weighted paths; use Dijkstra or A* for that.

## License
MIT – see [LICENSE](LICENSE).
