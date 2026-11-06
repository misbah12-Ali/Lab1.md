# Lab 4: Hill-Climbing Search with the N-Queens Problem

### Task 1: Create the State and Print It

**Code:**

```python
state = [1, 3, 0, 2]
print("State:", state)
```

**Output:**

```
State: [1, 3, 0, 2]
```

---

### 1. Does the evaluation score increase as the algorithm moves to better states?

Yes. The evaluation score increases when the algorithm moves to a better state because it accepts only improving moves.

### 2. Does the score decrease during a move accepted by this implementation?

No. The score never decreases because the algorithm accepts only moves that improve the current score.

### 3. What happens when the algorithm reaches a state with no better neighbor?

The algorithm stops because no neighboring state has a better evaluation score.

### 4. What does an evaluation score of zero mean?

An evaluation score of zero means there are no conflicts between queens. It represents a valid solution.

---

### Task 2: Experiment with Different Board Sizes

The experiment uses board sizes N = 4, 8, and 10.

| N | Run | Final Conflicts | Iterations | Solved? |
| --- | --- | --- | --- | --- |
| 4 | 1 | 1 | 2 | No |
| 4 | 2 | 0 | 3 | Yes |
| 8 | 1 | 0| 6 | Yes |
| 8 | 2 | 2 | 3 | No |
| 8 | 3 | 1 | 3 | No |
| 10 | 1 | 1 | 3 | No |
| 10 | 2 | 1 | 7| No|
| 10 | 3 | 2 | 4| No |

**Discussion:** The same board size does not always produce the same result because each run starts from a random initial state.

---

### Task 3: Compare Basic and Random-Restart Hill-Climbing

#### 1. Did basic Hill-Climbing find a solution in every run?

No. It may stop at a local maximum without finding a valid solution.

#### 2. Did Random-Restart Hill-Climbing improve the results?

Yes. It provides multiple opportunities to find a valid solution.

#### 3. How many restarts were needed to find a solution?

The number of restarts varies depending on the random initial states.

#### 4. Did any random-restart experiment reach its restart limit without finding a solution?

It is possible because success is not guaranteed within a fixed number of restarts.

#### 5. Why can Random-Restart improve the probability of success without guaranteeing success within a fixed number of attempts?

Each restart provides another opportunity to find a solution, but a fixed number of attempts may still be insufficient.

---

### Lab Questions

#### Question 1: What is the purpose of the evaluation function in Hill-Climbing?

The evaluation function measures the quality of a state. In the N-Queens problem, it counts conflicts between queens and helps the algorithm select better neighboring states.

#### Question 2: Why is an evaluation score of zero the best result in our N-Queens implementation?

An evaluation score of zero means there are no conflicts between queens. Therefore, it represents a valid solution.

#### Question 3: What is the difference between a local maximum and a global maximum?

A local maximum is a state that is better than all its immediate neighbors. A global maximum is the best possible state in the entire search space.

#### Question 4: Why can basic Hill-Climbing stop without finding a solution even when a solution exists?

It can reach a local maximum where no neighboring state improves the current score, even though a valid solution exists elsewhere.

#### Question 5: Does reaching a local maximum always mean that the algorithm has failed? Explain.

No. If the local maximum is also the global maximum and represents a goal state, the algorithm has succeeded. Otherwise, it may stop without finding a solution.

#### Question 6: For N = 8, how many neighbors does each state have in our implementation?

The number of neighbors is calculated as:

$$N \times (N - 1)$$

$$8 \times 7 = 56$$

Therefore, each state has **56 neighbors**.

#### Question 7: What does the evaluation-score graph tell you about the behavior of Hill-Climbing?

The graph shows how the evaluation score changes during the search. The score never decreases because the algorithm accepts only improving moves. It stops when no better neighbor is available.

#### Question 8: Explain probabilistic completeness in your own words.

Probabilistic completeness means that the probability of finding a solution approaches 1 as the number of independent search attempts increases, provided the required assumptions hold.

#### Question 9: Why does Random-Restart Hill-Climbing improve the chance of finding a solution?

It starts the search from different random states. This gives the algorithm more opportunities to escape unsuccessful starting points and find a valid solution.

#### Question 10: Does probabilistic completeness guarantee that a solution will be found within 100 restarts? Why or why not?

No. It does not guarantee success within 100 restarts because every attempt may fail. It only means that the probability of success approaches 1 as independent attempts increase, under the required assumptions.

---

### Extension Task: N = 20

**Objective:** Solve the N-Queens problem for N = 20 using Basic and Random-Restart Hill-Climbing.

| Measurement | Basic Hill-Climbing | Random-Restart Hill-Climbing |
| --- | --- | --- |
| Final conflicts | 2 | 0 |
| Iterations | 9 iteration| Not specified |
| Restarts required | Not applicable | 46 restarts |
| Valid solution found? | No | Yes |

**Comparison:** Basic Hill-Climbing uses one random initial state, while Random-Restart Hill-Climbing tries multiple random initial states. Random restarts can improve the chance of finding a valid solution.

**Discussion:** Increasing the number of queens changes the search space. The random initial state also affects the number of iterations, conflicts, and restarts required.
