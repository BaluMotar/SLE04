# Viva Preparation – SLE-4 (ADR + whole journey)

## Your own notes (from the submitted document)
- BFS uses a queue and explores level by level; DFS uses a stack or recursion and goes deep before backtracking.
- BFS finds a shortest path only when each move has equal cost.
- A visited set stops the same position being explored repeatedly and avoids loops.
- The C4 design separates input, search, memory/visited tracking and output.
- Before the viva, verify the exact profiling numbers in the SLE-2 report and be ready to explain the maze/test setup.

## 60-second answer: "Explain your project"
"I built a maze solver in Python. In SLE-1 I coded BFS and DFS. In SLE-2 I profiled them. In SLE-3 I drew the C4
architecture: Input, Search Engine, Visited Memory and Output. In SLE-4 I wrote an Architecture Decision Record:
I chose BFS because in an unweighted maze it guarantees the shortest path. I kept DFS to compare, and I know BFS
uses more memory and that my timing results only apply to my test setup."

## What is an ADR?
A short document that records **one important design decision**: the context, what was decided, what else was
considered, why, and the consequences. It lets someone later understand *why* the system is the way it is.

## Likely questions and answers
1. **What did you decide and why?** Use BFS when moves cost the same and the shortest path is needed, because BFS
   explores by distance level, so the first time it reaches the goal is via a shortest path.
2. **Why not DFS?** DFS finds a path but may be long. In `maze4_open_room.txt` BFS gives 10 steps, DFS gives 34.
3. **Why not A\*?** A* can be faster but needs a good heuristic (admissible) and a priority queue – more design and
   validation. For a small unweighted maze BFS is simpler and still optimal. (A* is a reasonable future improvement.)
4. **Is BFS always faster than DFS?** No. My benchmark was one small test in one environment. BFS's advantage is the
   shortest-path guarantee, not speed.
5. **What are the negatives of BFS?** More memory (stores a whole level of the frontier); not enough for weighted paths.
6. **When would BFS fail to give the best path?** When moves have different costs (e.g., mud = 5, road = 1). Then use
   Dijkstra or A*.
7. **Data structure for BFS / DFS?** BFS = queue (FIFO); DFS = stack (LIFO) or recursion.
8. **Why a visited set?** To avoid revisiting cells and looping forever, and to save time.
9. **Where does the visited set live in your C4 design?** In the Visited Memory container, used by the Search Engine.
10. **Explain your C4 levels.** Context (user + system), Container (Input, Search Engine, Visited Memory, Output),
    Component (frontier, visited set, goal test, path reconstruction), Code (classes/functions).
11. **Time and space complexity?** On a grid with V cells: time O(V), space O(V) for both. BFS's frontier can hold a
    whole level, so it often uses more memory than DFS's stack.
12. **Your SLE-2 numbers were 0.002005 ms (BFS) and 0.002842 ms (DFS) – can you trust them?** They are valid only for
    that test environment and maze. I can rerun `python src/compare.py` to show current numbers.
13. **What did you use AI for?** Drafting/organising the ADR, explaining concepts, practice questions. I checked it
    against my own code and can explain each decision.
14. **What would you change next?** Add A* with Manhattan distance, test larger mazes, and test weighted grids.

## Demo checklist for the viva
- [ ] `python src/compare.py` runs and you can read the table.
- [ ] You can open `mazes/maze4_open_room.txt` and say why DFS wanders.
- [ ] You can point to each C4 box and say its job.
- [ ] You can state the ADR in one sentence: "BFS, because shortest path in an unweighted maze."
