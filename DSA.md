# Data Structures & Algorithms Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is Big O notation?

**Answer:** Big O describes the upper bound of an algorithm's time/space complexity as input grows:
- **O(1)**: constant — hash table lookup
- **O(log n)**: logarithmic — binary search
- **O(n)**: linear — simple loop
- **O(n log n)**: linearithmic — merge sort
- **O(n²)**: quadratic — nested loops
- **O(2ⁿ)**: exponential — recursive Fibonacci

**💡 Tip:** Big O = **"worst case growth rate."** O(1) best, O(2ⁿ) worst. Drop constants: O(2n) = O(n). Drop lower terms: O(n² + n) = O(n²).

---

### Q2. What is the difference between an array and a linked list?

**Answer:**
| Operation | Array | Linked List |
|---|---|---|
| Access | O(1) by index | O(n) traverse |
| Insert end | O(1) amortized | O(1) |
| Insert start | O(n) shift | O(1) |
| Search | O(n) unsorted | O(n) |
| Memory | Contiguous | Scattered (extra pointer storage) |

**💡 Tip:** Array = **"fast access, slow insert/delete."** Linked list = **"slow access, fast insert/delete."** Use array for indexing, linked list for frequent insertions.

---

### Q3. What is a stack?

**Answer:** LIFO (Last In, First Out) data structure:
- **Push**: add to top — O(1)
- **Pop**: remove from top — O(1)
- **Peek**: view top — O(1)

Use cases: undo/redo, function call stack, expression evaluation, bracket matching.

**💡 Tip:** Stack = **"stack of plates."** Add/remove from top only. LIFO. Function calls use a call stack. Browser back button = stack.

---

### Q4. What is a queue?

**Answer:** FIFO (First In, First Out) data structure:
- **Enqueue**: add to rear — O(1)
- **Dequeue**: remove from front — O(1)

Use cases: task scheduling, BFS (Breadth-First Search), message queues, print queue.

**💡 Tip:** Queue = **"line at a store."** First person in line = first served. FIFO. Message queues, task scheduling, BFS traversal.

---

### Q5. What is a hash table?

**Answer:** Key-value store with O(1) average lookup/insert/delete:
- Hash function maps key → index
- Collision handling: chaining (linked list at each index) or open addressing (find next slot)
- Worst case: O(n) if all keys hash to same index

JavaScript objects, Python dicts, Java HashMap, Map in most languages.

**💡 Tip:** Hash table = **"fastest lookup."** Key → hash → index → value. Collisions are rare with good hash function. Average O(1), worst O(n).

---

### Q6. What is a binary search?

**Answer:** Find element in sorted array in O(log n):
```javascript
function binarySearch(arr, target) {
  let left = 0, right = arr.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
}
```

**💡 Tip:** Binary search = **"guess a number game."** Cut search space in half each step. 1M items → 20 steps. Array MUST be sorted.

---

### Q7. What is a binary tree? Binary search tree?

**Answer:**
- **Binary tree**: each node has at most 2 children
- **Binary search tree (BST)**: left child < parent < right child
- BST operations: search O(log n) average, O(n) worst (unbalanced)

Balanced BST: AVL tree, Red-Black tree — guarantee O(log n).

**💡 Tip:** BST = **"sorted tree."** Left < Parent < Right. Like looking up a word in dictionary — go left or right based on comparison. Balanced = O(log n).

---

### Q8. What are common sorting algorithms?

**Answer:**
| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |

**💡 Tip:** Merge sort = **stable, guaranteed O(n log n).** Quick sort = **fastest in practice, O(n²) worst.** Know merge sort (guaranteed) and quick sort (practical). Use built-in `.sort()` in production.

---

### Q9. What is recursion?

**Answer:** Function that calls itself with a smaller problem:
- **Base case**: condition to stop recursion
- **Recursive case**: calls itself with modified input

```javascript
function factorial(n) {
  if (n <= 1) return 1;        // base case
  return n * factorial(n - 1);  // recursive case
}
```

Risk: stack overflow without base case. Every recursion can be converted to iteration.

**💡 Tip:** Recursion = **"solve a smaller version of the same problem."** Always need: base case + progress toward it. Stack overflow = forgot base case.

---

### Q10. What is a graph?

**Answer:** Collection of nodes (vertices) connected by edges:
- **Directed**: edges have direction (A → B)
- **Undirected**: bidirectional (A ↔ B)
- **Weighted**: edges have values (distance, cost)
- Represented by: adjacency matrix, adjacency list

Use cases: social networks, maps, dependency graphs, web crawling.

**💡 Tip:** Graph = **"things connected to things."** Nodes + edges. Social networks, Google Maps, dependency trees. Use adjacency list for sparse graphs.

---

### Q11. What is BFS (Breadth-First Search)?

**Answer:** Level-by-level traversal using a **queue**:
1. Start at node, add to queue
2. Dequeue, visit all neighbors, enqueue them
3. Repeat until queue empty

Time: O(V + E). Space: O(V). Finds shortest path in unweighted graphs.

**💡 Tip:** BFS = **"explore near first."** Uses queue. Level by level. Finds shortest path. Like a ripple in water — expand outward.

---

### Q12. What is DFS (Depth-First Search)?

**Answer:** Go as deep as possible before backtracking, using a **stack** (or recursion):
1. Start at node, mark visited
2. Visit unvisited neighbor, recurse
3. Backtrack when no unvisited neighbors

Time: O(V + E). Space: O(V). Uses: cycle detection, topological sort, maze solving.

**💡 Tip:** DFS = **"go deep first, backtrack later."** Uses stack/recursion. Like exploring a maze — go down a path until dead end, then backtrack.

---

### Q13. What is dynamic programming?

**Answer:** Optimization technique that solves problems by breaking into overlapping subproblems and storing results:
- **Top-down (memoization)**: recursive + cache
- **Bottom-up (tabulation)**: iterative, fill table

Key: identify overlapping subproblems and optimal substructure.

**💡 Tip:** DP = **"don't recompute what you already know."** Fibonacci: F(5) needs F(4) and F(3), but F(4) also needs F(3) — cache F(3) instead of computing twice.

---

### Q14. What is a heap?

**Answer:** Complete binary tree where parent is always ≥ (max-heap) or ≤ (min-heap) than children:
- Access min/max: O(1)
- Insert: O(log n)
- Extract min/max: O(log n)

Used for: priority queues, finding K largest/smallest, heap sort.

**💡 Tip:** Min-heap = **"smallest on top."** Max-heap = **"largest on top."** Priority queue = heap. Finding K smallest = min-heap. Finding K largest = max-heap.

---

### Q15. What is the difference between BFS and DFS?

**Answer:**
| Feature | BFS | DFS |
|---|---|---|
| Data structure | Queue | Stack/Recursion |
| Explores | Level by level | Branch by branch |
| Shortest path | Yes (unweighted) | No |
| Memory | More (wide graphs) | Less (deep graphs) |
| Use case | Shortest path, peers | Maze, cycles, topology |

**💡 Tip:** BFS = **"wide first"** (queue, shortest path). DFS = **"deep first"** (stack, backtracking). Use BFS for shortest path, DFS for exploration.

---

### Q16. What is a linked list?

**Answer:** Linear data structure where each node points to the next:
- **Singly linked**: node → next
- **Doubly linked**: node ↔ prev, next
- **Circular**: last node points to first

```javascript
class Node { constructor(val) { this.val = val; this.next = null; } }
```

**💡 Tip:** Linked list = **"chain of boxes."** Each box knows where the next one is. No random access — must traverse. Good for frequent insertions/deletions.

---

### Q17. What is the two-pointer technique?

**Answer:** Use two pointers to solve problems efficiently:
- **Same direction**: slow/fast (detect cycle, find middle)
- **Opposite direction**: left/right (palindrome, two-sum in sorted array)
- **Sliding window**: subset of array problems

```javascript
// Reverse array: left starts at 0, right at end, swap and move inward
```

**💡 Tip:** Two pointers = **"two fingers scanning."** Same direction: cycle detection. Opposite: palindrome check. Sliding window: subarray problems. Often turns O(n²) → O(n).

---

### Q18. What is the sliding window technique?

**Answer:** Fixed or variable-size window sliding over an array/string:
- Fixed: "find max sum of subarray of size K"
- Variable: "longest substring with no repeating characters"

Maintain window with two pointers. Add/remove elements as window slides.

**💡 Tip:** Sliding window = **"move a frame across data."** Expand right, shrink left when condition broken. O(n) instead of O(n²). Key: "contiguous subarray/string" = sliding window hint.

---

### Q19. What is time complexity vs space complexity?

**Answer:**
- **Time complexity**: how runtime grows with input size (how fast?)
- **Space complexity**: how memory grows with input size (how much memory?)

Always consider both. Sometimes trading space for time is worth it (memoization, caching).

**💡 Tip:** Time = **"how many operations?"** Space = **"how much memory?"** Trade space for time: caching, memoization, pre-computation.

---

### Q20. What is a greedy algorithm?

**Answer:** Make the locally optimal choice at each step, hoping for global optimum:
- Doesn't always work (not all problems have greedy solution)
- Works for: activity selection, Huffman coding, Dijkstra's, coin change (specific cases)
- When greedy works: problem has "greedy choice property" + "optimal substructure"

**💡 Tip:** Greedy = **"pick the best option right now."** No backtracking. Works for some problems (coin change with standard denominations), fails for others (general coin change).

---

## Intermediate Questions (Q21–Q40)

### Q21. What is the difference between merge sort and quick sort?

**Answer:**
- **Merge sort**: divide → sort halves → merge. O(n log n) guaranteed. O(n) space. Stable.
- **Quick sort**: pick pivot → partition → recurse. O(n log n) average, O(n²) worst. O(log n) space. In-place.

Quick sort is faster in practice due to cache locality. Merge sort is stable and guaranteed.

**💡 Tip:** Merge sort = **"guaranteed fast, needs extra memory."** Quick sort = **"usually faster, rare worst case, in-place."** Most languages use a hybrid (Timsort, introsort).

---

### Q22. What is a trie (prefix tree)?

**Answer:** Tree where each node represents a character:
- Path from root to node = a word/prefix
- Search: O(m) where m = word length (independent of total words)
- Use cases: autocomplete, spell check, IP routing, word search

**💡 Tip:** Trie = **"autocomplete tree."** Each path = a word. Find all words starting with "app" → traverse to "app" → collect children. O(m) search regardless of dictionary size.

---

### Q23. What is Dijkstra's algorithm?

**Answer:** Find shortest path from source to all nodes in weighted graph:
1. Start at source, distance = 0, all others = infinity
2. Visit unvisited node with smallest distance
3. Update neighbors' distances if shorter path found
4. Repeat until all visited

Time: O((V + E) log V) with min-heap. Doesn't work with negative weights.

**💡 Tip:** Dijkstra = **"always visit the closest unvisited node."** Min-heap for efficiency. No negative weights (use Bellman-Ford for those). Google Maps uses variants of this.

---

### Q24. What is topological sort?

**Answer:** Linear ordering of directed acyclic graph (DAG) nodes where for every edge A→B, A comes before B:
- Use DFS + stack (post-order)
- Or BFS with in-degree tracking (Kahn's algorithm)
- Use cases: build systems, course prerequisites, dependency resolution

**💡 Tip:** Topological sort = **"order by dependencies."** Must take CS101 before CS201. Only works on DAGs (no cycles). Build systems use this.

---

### Q25. What is the Knuth-Morris-Pratt (KMP) algorithm?

**Answer:** Pattern matching in O(n + m) time:
- Pre-process pattern to build "failure function" (longest prefix that's also suffix)
- Skip characters we know won't match
- Never goes backward in the text

Better than naive O(n × m) matching.

**💡 Tip:** KMP = **"smart string search."** Remember where you've been — don't recheck characters. Build prefix table from pattern, then scan text once.

---

### Q26. What is a balanced binary tree?

**Answer:** Tree where height difference between left and right subtrees ≤ 1 for every node:
- **AVL tree**: strict balance (height diff ≤ 1). Rotations on insert/delete.
- **Red-Black tree**: relaxed balance. Used in Java TreeMap, C++ std::map.
- Both guarantee O(log n) operations.

**💡 Tip:** Balanced = **O(log n) guaranteed.** Unbalanced BST can degrade to O(n) (like a linked list). Self-balancing trees (AVL, Red-Black) prevent this.

---

### Q27. What is the divide and conquer strategy?

**Answer:** Break problem into smaller subproblems, solve independently, combine:
- **Divide**: split into smaller parts
- **Conquer**: solve each part recursively
- **Combine**: merge solutions

Examples: merge sort, quick sort, binary search, closest pair of points.

**💡 Tip:** Divide and conquer = **"split, solve, merge."** Like managing a company: CEO → VPs → Managers → ICs. Each level solves a smaller problem.

---

### Q28. What is memoization?

**Answer:** Cache results of expensive function calls:
```javascript
function fib(n, memo = {}) {
  if (n in memo) return memo[n];
  if (n <= 1) return n;
  memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
  return memo[n];
}
```

Turns O(2ⁿ) recursive Fibonacci into O(n) with memoization.

**💡 Tip:** Memoization = **"remember and reuse."** Top-down DP. Before computing, check cache. After computing, store in cache. Same as caching but for function results.

---

### Q29. What is backtracking?

**Answer:** Systematic trial-and-error: try option → if leads to dead end → undo and try another:
- Use: N-Queens, Sudoku solver, maze solving, combinations/permutations
- Pruning: skip paths that can't lead to solution (optimization)

Like DFS with "undo" when path fails.

**💡 Tip:** Backtracking = **"try, fail, undo, try something else."** Like solving Sudoku — fill a cell, if it breaks things, erase and try another number. Prune early to save time.

---

### Q30. What is a priority queue?

**Answer:** Queue where elements are dequeued by priority (not insertion order):
- Min-priority queue: smallest element first
- Max-priority queue: largest element first
- Implemented with heap: O(log n) insert, O(1) peek, O(log n) extract

Used for: Dijkstra's, task scheduling, Huffman coding.

**💡 Tip:** Priority queue = **"VIP line"** — highest priority goes first. Heap-based. Min-heap = min priority. Max-heap = max priority. Use when order matters.

---

### Q31. What is the Floyd-Warshall algorithm?

**Answer:** All-pairs shortest path in O(V³):
- Dynamic programming approach
- Works with negative weights (no negative cycles)
- Creates distance matrix for all node pairs

Use for: finding shortest path between ANY two nodes. Space: O(V²).

**💡 Tip:** Floyd-Warshall = **"shortest path between EVERY pair."** O(V³) but simple. Three nested loops. Use when you need all pairs, not just one source.

---

### Q32. What is amortized analysis?

**Answer:** Average cost of operations over a sequence:
- Dynamic array: most insertions O(1), occasional resize O(n) → amortized O(1)
- Not worst case, not average case — spread cost over many operations

**💡 Tip:** Amortized = **"occasional expensive operation, but rare."** Like buying coffee: most days $0 (brought from home), one day $5. Amortized = $5/30 = $0.17/day.

---

### Q33. What is the difference between stable and unstable sort?

**Answer:**
- **Stable**: preserves relative order of equal elements. [3a, 2, 3b] → [2, 3a, 3b]
- **Unstable**: equal elements may reorder. [3a, 2, 3b] → [2, 3b, 3a]

Stable: merge sort, insertion sort, bubble sort. Unstable: quick sort, heap sort, selection sort.

**💡 Tip:** Stability matters when: sorting by multiple keys (sort by name, then by age — stable keeps name order within same age).

---

### Q34. What is a graph cycle?

**Answer:** Path that starts and ends at the same node:
- Detect with: DFS (back edge detection), Union-Find
- Directed graph: topological sort fails if cycle exists
- Undirected graph: if node visited twice (not parent) → cycle

**💡 Tip:** Cycle = **"you end up where you started."** DFS with recursion stack detects it. No cycle = DAG (directed acyclic graph). Important for dependency resolution.

---

### Q35. What is Union-Find (Disjoint Set)?

**Answer:** Data structure for tracking connected components:
- **Find**: which component does element belong to?
- **Union**: merge two components
- Optimization: path compression + union by rank → nearly O(1)

Use for: Kruskal's MST, connected components, detecting cycles.

**💡 Tip:** Union-Find = **"merge groups, find group membership."** Path compression makes it fast. Use for: "are these two nodes connected?" and "merge these two groups."

---

### Q36. What is a minimum spanning tree (MST)?

**Answer:** Tree connecting all nodes with minimum total edge weight:
- **Kruskal's**: sort edges, add smallest that doesn't create cycle. O(E log E).
- **Prim's**: grow tree from one node, always add cheapest edge. O(E log V).

Use for: network design, clustering, circuit design.

**💡 Tip:** MST = **"connect everything with minimum cost."** Like wiring a house — connect all rooms with least wire. Kruskal: sort + add edges. Prim: grow from one node.

---

### Q37. What is the "longest common subsequence" problem?

**Answer:** Find longest subsequence common to two strings:
- DP solution: O(m × n) time and space
- If chars match: 1 + LCS(i-1, j-1)
- If not: max(LCS(i-1, j), LCS(i, j-1))

"ABCD" and "ACBD" → LCS = "ABD" (length 3).

**💡 Tip:** LCS = **"what's common between these strings?"** Classic DP. Build a table, fill it diagonally. Used in: diff tools, DNA comparison, version control.

---

### Q38. What is the Knapsack problem?

**Answer:** Given items with weight and value, maximize value without exceeding capacity:
- **0/1 Knapsack**: each item used once (DP: O(n × W))
- **Fractional Knapsack**: can take fractions (greedy: sort by value/weight ratio)

**💡 Tip:** 0/1 = **DP** (take or don't take each item). Fractional = **greedy** (take best value/weight first). Capacity W = constraint. Build table DP[i][w].

---

### Q39. What is a segment tree?

**Answer:** Data structure for range queries with updates:
- Build: O(n)
- Range query (sum, min, max): O(log n)
- Point update: O(log n)
- Use: range sum, range min, interval problems

**💡 Tip:** Segment tree = **"fast range queries with updates."** Like a restaurant menu organized by sections. Find min/sum in any range in O(log n).

---

### Q40. What is a LRU cache?

**Answer:** Least Recently Used cache that evicts the least recently accessed item when full:
- Get: O(1) — move to front (most recently used)
- Put: O(1) — add to front, evict from back if over capacity
- Implementation: HashMap + Doubly Linked List

**💡 Tip:** LRU = **"use it or lose it."** HashMap for O(1) lookup. Doubly linked list for O(1) reorder. Front = most recent. Back = least recent (evicted first).

---

## Advanced Questions (Q41–Q50)

### Q41. What is the Bellman-Ford algorithm?

**Answer:** Single-source shortest path that handles negative weights:
- Relax all edges V-1 times
- Detect negative cycles (one more relaxation check)
- Time: O(V × E) — slower than Dijkstra but handles negatives

Use when graph has negative weights. Detect negative cycles.

**💡 Tip:** Bellman-Ford = **"Dijkstra that handles negatives."** Slower O(VE) but detects negative cycles. Use only when negative weights exist.

---

### Q42. What is the A* search algorithm?

**Answer:** Best-first search with heuristic for optimal pathfinding:
- f(n) = g(n) + h(n)
- g(n): cost from start to n
- h(n): estimated cost from n to goal (heuristic)
- Must use admissible heuristic (never overestimates)

Used in: game AI, GPS navigation, robot pathfinding.

**💡 Tip:** A* = **"Dijkstra with a brain."** Heuristic guides search toward goal. Faster than Dijkstra if heuristic is good. GPS uses A* for route planning.

---

### Q43. What is a B-tree?

**Answer:** Self-balancing tree optimized for disk/database access:
- Each node can have multiple children (not just 2)
- Keeps data sorted, searches O(log n)
- Minimizes disk reads (wide, shallow tree)

Databases use B-trees for indexes.

**💡 Tip:** B-tree = **"fat tree for disk access."** Wide and shallow → fewer disk reads. Database indexes are B-trees. Every database interview should mention this.

---

### Q44. What is the traveling salesman problem (TSP)?

**Answer:** Find shortest route visiting every city exactly once:
- NP-hard: no known polynomial-time solution
- Exact: O(n!) brute force, O(n² × 2ⁿ) with DP (Held-Karp)
- Approximation: nearest neighbor, Christofides algorithm (1.5x optimal)

**💡 Tip:** TSP = **"visit everywhere, return home, minimize distance."** NP-hard = no fast exact solution. Use heuristics/approximations for large inputs.

---

### Q45. What is the Boyer-Moore majority vote algorithm?

**Answer:** Find element appearing more than n/2 times in O(n) time, O(1) space:
```javascript
let candidate, count = 0;
for (const num of nums) {
  if (count === 0) candidate = num;
  count += (num === candidate) ? 1 : -1;
}
```

Pair different elements to cancel. Majority always survives.

**💡 Tip:** Boyer-Moore = **"majority always wins in cancellations."** Pair different elements → cancel. Majority element survives. O(n) time, O(1) space. Brilliant.

---

### Q46. What is the fast and slow pointer technique?

**Answer:** Two pointers moving at different speeds:
- **Cycle detection**: fast moves 2 steps, slow 1 step → if they meet, cycle exists
- **Find middle**: fast reaches end, slow is at middle
- **Find cycle start**: after detection, reset one pointer, move both 1 step

**💡 Tip:** Fast/slow = **"tortoise and hare."** If hare meets tortoise → cycle. Used in: linked list cycle, find duplicate number, palindrome check.

---

### Q47. What is a suffix array?

**Answer:** Sorted array of all suffixes of a string:
- Enables efficient pattern matching, longest repeated substring
- Build: O(n log n) or O(n)
- Search: O(m log n) with binary search

Used in: DNA sequencing, text compression, search engines.

**💡 Tip:** Suffix array = **"all endings of a string, sorted."** Find patterns, repeated substrings. More space-efficient than suffix tree.

---

### Q48. What is the Kadane's algorithm?

**Answer:** Find maximum subarray sum in O(n):
```javascript
function maxSubArray(nums) {
  let maxSum = nums[0], currentSum = nums[0];
  for (let i = 1; i < nums.length; i++) {
    currentSum = Math.max(nums[i], currentSum + nums[i]);
    maxSum = Math.max(maxSum, currentSum);
  }
  return maxSum;
}
```

**💡 Tip:** Kadane's = **"start fresh when running sum goes negative."** At each position: best ending here = max(current element, running sum + current). O(n) time.

---

### Q49. What is the Rabin-Karp algorithm?

**Answer:** Pattern matching using rolling hash:
- Compute hash of pattern and first window
- Slide window: update hash in O(1) (remove leftmost, add rightmost)
- When hash matches → verify (avoid false positives)
- Average O(n + m), worst O(n × m)

**💡 Tip:** Rabin-Karp = **"rolling hash for string search."** Slide window → update hash in O(1). Good for multiple pattern search. Plagiarism detection uses this.

---

### Q50. How to approach a DSA interview problem?

**Answer:**
1. **Understand**: clarify the problem, ask about edge cases
2. **Examples**: work through examples manually
3. **Brute force**: start with naive solution, explain complexity
4. **Optimize**: look for patterns (sorted? → binary search; subarray → sliding window)
5. **Code**: write clean, readable code
6. **Test**: verify with examples and edge cases
7. **Complexity**: state time and space complexity

**💡 Tip:** DSA interview = **"understand → brute force → optimize → code → test."** Always start with brute force, then optimize. Communicate your thinking. No silence.

---
