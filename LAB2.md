# Lab 2: Uninformed Search — BFS and DFS on a 2D Maze

**Course:** Artificial Intelligence (BSCS 4th Semester)  
**Topic:** Uninformed Search - Breadth First Search (BFS) & Depth First Search (DFS)

---

## 1. Section 7: Compare BFS and DFS

### Question:
Fill in the comparison table using the outputs of BFS and DFS.

### Answer: Comparison Table

| Algorithm | Path Length | Maximum Frontier |
| :--- | :---: | :---: |
| BFS | 8 | 3 |
| DFS | 8 | 3 |

### Questions & Answers

**Q1. Which algorithm found the shorter path?**

**Answer:** Both BFS and DFS found paths of equal length (8 moves) in this maze.

**Q2. Which algorithm had the larger maximum frontier?**

**Answer:** Both algorithms had the same maximum frontier size of 3.

**Q3. Why did the two algorithms produce different paths on the same maze?**

**Answer:** BFS uses a Queue (FIFO), while DFS uses a Stack (LIFO). Therefore, they explore the maze in different orders and produce different paths.

**Q4. Does DFS always produce the shortest path? Why or why not?**

**Answer:** No, DFS does not always produce the shortest path because it explores one branch deeply before exploring other possible paths.

**Q5. Does BFS always produce the shortest path when every movement costs 1? Why?**

**Answer:** Yes, BFS always finds the shortest path when every movement has an equal cost because it explores all possible paths level by level.

---

## 2. Experiment 1 — Change the Neighbor Order

### Question:
DFS's exact path depends on the order in which neighbors are tried. Change directions and re-run DFS. Did the DFS path change? Did BFS's path depend on this ordering the same way? Why or why not?

### Answer: Complete Code
Use the following code after completing the original BFS and DFS functions.

```python
# Change the neighbor order
directions = [
    (0, 1),   # right
    (1, 0),   # down
    (0, -1),  # left
    (-1, 0)   # up
]

# Run DFS again
dfs_path2, dfs_frontier2 = dfs(maze, start, goal)

print("DFS path (new order):", dfs_path2)
print("DFS path length:", len(dfs_path2) - 1)
print("DFS maximum frontier:", dfs_frontier2)

print()

print_maze(maze, dfs_path2)
```

### Output

```
DFS path (new order):
[(0, 0), (1, 0), (2, 0), (2, 1),
 (2, 2), (3, 2), (3, 3), (3, 4), (4, 4)]

DFS path length: 8
DFS maximum frontier: 3
```

### Answer to the question
Yes, the DFS path changed because DFS uses a Stack (LIFO), which explores the most recently added neighbor first.

BFS also can produce a different exact path when neighbor order changes, if multiple shortest paths are available. However, BFS still guarantees the shortest path when all movements have equal costs.

### Reset directions before Experiment 2:

```python
directions = [
    (-1, 0),  # up
    (1, 0),   # down
    (0, -1),  # left
    (0, 1)    # right
]
```

---

## 3. Experiment 2 — Change Start and Goal

### Question:
Try different start and goal cells in the same maze. Run BFS and DFS and record their results. Pick at least 3 different pairs of open cells.

### Answer: Complete Code

```python
# Reset directions to original order
directions = [
    (-1, 0),  # up
    (1, 0),   # down
    (0, -1),  # left
    (0, 1)    # right
]

# Define three different start and goal pairs
test_pairs = [
    ((0, 0), (2, 2)),
    ((0, 0), (0, 4)),
    ((2, 0), (4, 4))
]

# Run BFS and DFS for each pair
for start, goal in test_pairs:
    bfs_path, bfs_frontier = bfs(maze, start, goal)
    dfs_path, dfs_frontier = dfs(maze, start, goal)

    print("\nStart:", start, "Goal:", goal)

    print("BFS Length:", len(bfs_path) - 1)
    print("BFS Max Frontier:", bfs_frontier)

    print("DFS Length:", len(dfs_path) - 1)
    print("DFS Max Frontier:", dfs_frontier)
```

### Output

```
Start: (0, 0) Goal: (2, 2)
BFS Length: 4
BFS Max Frontier: 3
DFS Length: 10
DFS Max Frontier: 3

Start: (0, 0) Goal: (0, 4)
BFS Length: 4
BFS Max Frontier: 3
DFS Length: 4
DFS Max Frontier: 2

Start: (2, 0) Goal: (4, 4)
BFS Length: 6
BFS Max Frontier: 3
DFS Length: 6
DFS Max Frontier: 4
```

### Answer: Completed Table

| Start | Goal | BFS Length | BFS Max Frontier | DFS Length | DFS Max Frontier |
| :---: | :---: | :---: | :---: | :---: | :---: |
| (0,0) | (2,2) | 4 | 3 | 10 | 3 |
| (0,0) | (0,4) | 4 | 3 | 4 | 2 |
| (2,0) | (4,4) | 6 | 3 | 6 | 4 |

### Questions & Answers

**Q6. Does BFS's path length ever change if you re-run it with the same start/goal? Does DFS's?**

**Answer:** No, both BFS and DFS produce the same path length when run repeatedly with the same start, goal, maze, and neighbor order. Their results are deterministic.

**Q7. As the distance between start and goal grows, what happens to the maximum frontier size for each algorithm?**

**Answer:** The maximum frontier size may increase as the search explores more cells. BFS generally stores more discovered states, while DFS often stores fewer, but the actual size depends on the maze structure and neighbor order.

**Q8. Can you find a start/goal pair where BFS and DFS return the same path? What does that tell you about the maze structure between those two points?**

**Answer:** Yes. For example, start (0,0) and goal (0,4) produce the same path in both algorithms. This indicates that both algorithms follow the same available route between these points.

**Q9. What happens if the goal is unreachable from the start (e.g. sealed off by walls)? Try it — what do `bfs()` and `dfs()` return?**

**Answer:** If the goal is unreachable, both BFS and DFS explore all reachable cells and return `None` as the path.

### Code:

```python
# Test an unreachable goal
start = (0, 0)
goal = (1, 1)  # This cell is a wall

bfs_path, bfs_frontier = bfs(maze, start, goal)
dfs_path, dfs_frontier = dfs(maze, start, goal)

print("BFS Result:", bfs_path)
print("DFS Result:", dfs_path)
```

### Output

```
BFS Result: None
DFS Result: None
```

**Note:** In the supplied maze, all open cells are connected. Here, (1,1) is a wall, so it demonstrates an unreachable goal.
