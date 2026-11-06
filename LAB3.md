# Lab 3: A* Search for the 8-Puzzle

**Course:** Artificial Intelligence (BSCS 4th Semester)  
**Topic:** Informed Search - A* Search with Heuristics

---

## 1. Problem Representation

The 8-puzzle consists of a 3x3 board containing eight numbered tiles and one blank space. The blank space is represented by 0.

**Goal State:** `(1, 2, 3, 4, 5, 6, 7, 8, 0)`

Each state represents the current arrangement of the tiles. Legal moves shift a tile into the blank space.

### Basic Setup Code

```python
import heapq
import itertools

GOAL_STATE = (1, 2, 3, 4, 5, 6, 7, 8, 0)

def is_goal(state):
    return state == GOAL_STATE

def get_neighbors(state):
    neighbors = []
    blank_index = state.index(0)
    row, col = divmod(blank_index, 3)

    # Possible moves: Up, Down, Left, Right
    moves = []
    if row > 0:
        moves.append((-1, 0, "Up"))
    if row < 2:
        moves.append((1, 0, "Down"))
    if col > 0:
        moves.append((0, -1, "Left"))
    if col < 2:
        moves.append((0, 1, "Right"))

    for dr, dc, action in moves:
        new_row = row + dr
        new_col = col + dc
        new_index = new_row * 3 + new_col
        new_state = list(state)
        new_state[blank_index], new_state[new_index] = new_state[new_index], new_state[blank_index]
        neighbors.append((tuple(new_state), action))

    return neighbors
```

---

## 2. Misplaced Tiles Heuristic

This heuristic counts the number of numbered tiles that are not in their correct goal positions. The blank space (0) is not counted.

```python
def misplaced_tiles(state):
    count = 0
    for i in range(9):
        if state[i] != 0 and state[i] != GOAL_STATE[i]:
            count += 1
    return count
```

---

## 3. Manhattan Distance Heuristic

This heuristic calculates the total horizontal and vertical distance of each numbered tile from its correct goal position. The blank space is ignored.

```python
def manhattan_distance(state):
    distance = 0
    for i, tile in enumerate(state):
        if tile == 0:
            continue
        row, col = divmod(i, 3)
        goal_row, goal_col = divmod(tile - 1, 3)
        distance += abs(row - goal_row) + abs(col - goal_col)
    return distance
```

Both heuristics return 0 for the goal state because no moves are required when the puzzle is solved.

---

## 4. A* Search Algorithm

A* uses the evaluation function:

$$f(n) = g(n) + h(n)$$

- **g(n):** Cost from the starting state to the current state.
- **h(n):** Estimated cost from the current state to the goal.
- **f(n):** Total estimated cost.

A priority queue selects the state with the lowest estimated cost.

```python
def a_star_search(start_state, heuristic_fn):
    counter = itertools.count()
    frontier = []

    heapq.heappush(
        frontier,
        (heuristic_fn(start_state), next(counter),
         0, start_state, [])
    )

    best_g = {start_state: 0}
    nodes_expanded = 0

    while frontier:
        f, _, g, state, path = heapq.heappop(frontier)
        nodes_expanded += 1

        if g > best_g.get(state, float("inf")):
            continue

        if is_goal(state):
            return {
                "path": path,
                "cost": g,
                "nodes_expanded": nodes_expanded
            }

        for neighbor_state, action in get_neighbors(state):
            new_g = g + 1

            if new_g < best_g.get(neighbor_state, float("inf")):
                best_g[neighbor_state] = new_g
                new_f = new_g + heuristic_fn(neighbor_state)

                heapq.heappush(
                    frontier,
                    (new_f, next(counter), new_g,
                     neighbor_state, path + [action])
                )

    return {
        "path": None,
        "cost": None,
        "nodes_expanded": nodes_expanded
    }
```

---

## 5. Testing and Results

For the easy test state:

```python
start_state = (1, 2, 3, 4, 5, 6, 7, 0, 8)

result = a_star_search(start_state, manhattan_distance)

print("Solution Cost:", result["cost"])
print("Solution Path:", result["path"])
print("Nodes Expanded:", result["nodes_expanded"])
```

### Output

```
Solution Cost: 1
Solution Path: ['Right']
Nodes Expanded: 2
```

The A* algorithm using Manhattan Distance produces:

- **Solution Cost:** 1
- **Solution Path:** Right
- **Nodes Expanded:** 2

### Comparison Experiment

The experiment compares both heuristics at scramble depths of 5, 10, 15, and 20 moves, with 5 trials at each depth.

The actual comparison results depend on running the experiment in Google Colab.

---

## 6. Analysis Questions

### Q1. At what scramble depth does the performance gap become visible? Why does it widen?

**Answer:** The gap becomes more noticeable as scramble depth increases. Harder puzzles generally require more search, so a more informative heuristic can reduce the number of states expanded.

### Q2. Manhattan Distance is always greater than or equal to Misplaced Tiles. Is it still admissible?

**Answer:** Yes. Manhattan Distance is admissible because it never overestimates the minimum number of moves needed to reach the goal.

### Q3. Why can Manhattan Distance expand fewer nodes?

**Answer:** Manhattan Distance provides a more detailed estimate of the remaining cost than Misplaced Tiles. Therefore, A* can usually search more efficiently.
