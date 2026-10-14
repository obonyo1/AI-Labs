## AI-Labs

Lab exercises from the AI class.

### Lab: Maze Search Algorithms

Notebook: `maze_search_algos.ipynb`

#### Lab Instructions

We have already discussed and demonstrated BFS, DFS, and UCS. In this lab, you will:

- Refactor UCS to use a while loop.
- Build a test function that can run BFS, DFS, DLS, IDDFS, and UCS (including your modified UCS) on the different mazes provided below.

##### Provided Mazes

```python
# Maze 1: Open Maze
maze1 = [
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 1, 1, 1, 1, 0, 1, 1, 1, 0],
    [0, 0, 0, 0, 1, 0, 0, 0, 1, 0],
    [1, 1, 1, 0, 1, 1, 1, 0, 1, 0],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0],
    [0, 1, 1, 1, 1, 0, 1, 1, 1, 0],
    [0, 1, 0, 0, 0, 0, 0, 0, 1, 0],
    [0, 1, 0, 1, 1, 1, 1, 0, 1, 0],
    [0, 0, 0, 1, 0, 0, 0, 0, 0, 0],
    [1, 1, 0, 0, 0, 1, 1, 1, 1, 0],
]

# Maze 2: Dense Maze
maze2 = [
    [0, 1, 0, 1, 0, 1, 0, 1, 0, 0],
    [0, 1, 0, 1, 0, 1, 0, 1, 1, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 1, 0],
    [1, 1, 1, 1, 1, 1, 1, 0, 1, 0],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0],
    [0, 1, 1, 1, 1, 0, 1, 1, 1, 0],
    [0, 1, 0, 0, 0, 0, 0, 0, 1, 0],
    [0, 1, 0, 1, 1, 1, 1, 0, 1, 0],
    [0, 0, 0, 1, 0, 0, 0, 0, 0, 0],
    [1, 1, 0, 0, 0, 1, 1, 1, 1, 0],
]

# 0 = Free space
# 1 = Wall
# Start = (0, 0)
# Goal = (9, 9)
```

##### Task Part 1: Modify UCS

Refactor the UCS implementation below so that it uses a while loop as the main control structure. Do not use any imports.

```python
def ucs(maze, start, goal):
    queue = [(0, start, [start])]
    for i in range(100):
        if len(queue) == 0:
            return None
        # Find the cheapest node
        cheapest = 0
        for i in range(len(queue)):
            if queue[i][0] < queue[cheapest][0]:
                cheapest = i
        # Remove the cheapest node
        cost, node, path = queue.pop(cheapest)
        # Check if we reached the goal
        if node == goal:
            return path
        # Add neighbors to the queue
        for neighbor in get_neighbors(maze, node):
            queue.append((cost + 1, neighbor, path + [neighbor]))
    return None

# Example Usage
start = (0, 0)
goal = (3, 3)

path = ucs(maze1, start, goal)
print(f"Path: {path}")
# Output: Path: [(0, 0), (1, 0), (2, 0), (2, 1), (2, 2), (2, 3), (3, 3)]
```

##### Task Part 2: Write a Test Function

Write a function that:

a. Accepts a maze, a start, a goal, and an algorithm.
b. Runs the algorithm and prints the path length (and path if found).

##### Deliverables

Your complete Google Colaboratory notebook with:

- Modified UCS code using a while loop.
- Your test function with outputs showing BFS, DFS, UCS, DLS, IDDFS, and your new UCS on both provided mazes.

---

### What I Did in the Notebook

#### Part 1: UCS refactor

- Kept the original `ucs` (a `for` loop capped at 100 iterations) so it can be compared with the new version.
- Wrote `ucs_while`, which uses `while queue:` as the main control structure and has no imports.
- Added a `best_cost` dictionary that records the cheapest known cost to each cell. It only queues a neighbour if a cheaper route to it has been found, which makes the search terminate now that the 100-iteration cap is gone.
- Checked `ucs_while` on the brief's example (start `(0, 0)`, goal `(3, 3)`).

#### Part 2: Test function

- Wrote `test_algorithm(name, algorithm, maze, start, goal)`, which runs an algorithm and prints the path length (number of moves) and the path, or "No path found".
- Put BFS, DFS, UCS (original), UCS (while loop), DLS and IDDFS in one dictionary. DLS and IDDFS are wrapped in a `lambda` with a limit of 20, so every algorithm can be called the same way.
- Ran all six algorithms on both mazes from `(0, 0)` to `(9, 9)`.

#### Results

| Algorithm | Maze 1 | Maze 2 |
|---|---|---|
| BFS | 18 | 18 |
| DFS | 18 | 18 |
| UCS (original) | No path found | No path found |
| UCS (while loop) | 18 | 18 |
| DLS (limit 20) | 20 | 20 |
| IDDFS (max depth 20) | 18 | 18 |

- **UCS (original)** finds no path because its 100-iteration cap runs out before it reaches the goal. `ucs_while` has no cap and finds the shortest path (18 moves).
- **DLS** returns a path of length 20 because it has no visited set, so it steps back over cells it has already seen. The path is valid but not the shortest.
- **DFS** found a path of length 18 on both mazes, but DFS does not guarantee the shortest path.