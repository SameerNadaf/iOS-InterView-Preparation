# Data Structures and Algorithms

### 1. How do we measure algorithms ?

<details>
  <summary>Solution</summary>

We measure algorithms primarily by **how their resource usage grows** with input size. The most common metrics:

**1. Time complexity (asymptotic)**

* Describes how the number of basic operations grows as input size `n` grows.
* Captured with Big-O notation (upper bound). Also used:

  * **Big-O** `O(f(n))` — worst-case upper bound.
  * **Big-Ω** `Ω(f(n))` — best-case lower bound.
  * **Big-Θ** `Θ(f(n))` — tight bound (both upper & lower).

**2. Space complexity**

* Amount of extra memory the algorithm uses (auxiliary space), e.g. `O(1)` (constant), `O(n)` (linear).

**3. Average / Worst / Best / Amortized**

* *Worst-case* (e.g., Quicksort worst `O(n^2)`), *average-case* (expected), *best-case* (ideal input), *amortized* (cost averaged across many operations, e.g., dynamic array append is amortized `O(1)`).

**4. Practical considerations beyond asymptotics**

* Constant factors, cache locality, memory allocation, and I/O can drastically affect real-world speed even for same Big-O.
* Benchmarking/profiling (Instruments, time trials) is used to measure real performance.

**Example:**

* Array indexing → `O(1)` time.
* Linear search in array → `O(n)`.
* Merge sort → `O(n log n)` time, `O(n)` extra space.

In interviews you’ll cite asymptotic complexity, explain assumptions (random input, hash distribution), and note space trade-offs.
</details>

### 2. Why should developers care about data structures and algorithms ?

<details>
  <summary>Solution</summary>

**Reasons developers should care:**

1. **Correct tool for the problem**

   * Different data structures match different access/update patterns. Example: mapping names→phone → use `Dictionary` (hash map); ordered access and indexed retrieval → use `Array`.

2. **Performance & scalability**

   * Choosing wrong DS/algorithm leads to slow, memory-hungry apps. Small differences (`O(n)` vs `O(log n)`) become critical at scale.

3. **Better design and trade-offs**

   * Understand memory vs speed vs complexity trade-offs. Example: `Tree` for ordered data vs `HashMap` for O(1) lookups.

4. **Maintainability & correctness**

   * Data-structure-informed code is easier to reason about, test and debug.

5. **Problem solving & interviews**

   * Many system design and coding interviews test DS\&A knowledge.

6. **Real-world examples**

   * Caching layers (LRU cache: linked list + hash map), routing (graphs), search (binary search, indices), concurrency-safe structures.

**Practical tip:** Learn a small set of powerful data structures (arrays, hash sets/dicts, trees, heaps, graphs) and core algorithms (search/sort, traversal, Dijkstra/BFS/DFS) — you’ll be able to pick appropriate solutions quickly.

</details>

### 3. Which sorting algorithm uses a pivot and partitioning ?

<details>
  <summary>Solution</summary>

**Quicksort.**

**How it works (high level):**

* Choose a **pivot** element.
* **Partition** the array into elements `< pivot` and `> pivot` (often in-place).
* Recursively sort the partitions.
* Combine (concatenation is trivial because partition step arranges elements).

**Partition schemes**

* **Lomuto** partition (simple, uses single index).
* **Hoare** partition (often faster; uses two indices moving inward).

**Complexities**

* Average time: `O(n log n)`
* Worst time: `O(n^2)` (e.g., if pivot choice is poor on already sorted input and naive pivot selection)
* Space: `O(log n)` average recursion stack (in-place sort).

**Notes**

* Pivot selection strategy matters (first/last/median-of-three/random). Randomized pivot gives expected `O(n log n)` and dramatically reduces worst-case likelihood.
* Quicksort is typically faster in practice than mergesort for in-memory sorting because of locality and low constant factors, though merge sort is stable and has guaranteed `O(n log n)` worst-case.

</details>

### 4. What are the properties of a doubly linked node ?

<details>
  <summary>Solution</summary>

A **doubly linked node** typically contains:

1. **Value** (payload/data) — the stored item (generic `T`).
2. **Next pointer** — a reference to the next node in the list (or `nil` at tail).
3. **Previous pointer** — a reference to the previous node in the list (or `nil` at head).

**Operations & complexity**

* Insert/delete at a known node: `O(1)` (adjust pointers).
* Traversal to find a value: `O(n)`.
* Doubly linked lists let you traverse forward/backward and remove nodes without scanning from the head (when you already have the node reference).

**Simple Swift-like node example**

```swift
class DNode<T> {
  var value: T
  var next: DNode?
  weak var prev: DNode? // weak to avoid retain cycles when using classes
  init(value: T) { self.value = value }
}
```

**Trade-offs**

* Pros: O(1) deletion/insertion when node reference is known, bidirectional traversal.
* Cons: extra memory for prev pointer, pointer-chasing hurts cache locality (slower than arrays for sequential access).

</details>

### 5. Name three built-in data structures in Swift ?

<details>
  <summary>Solution</summary>

Three common built-in Swift collections:

1. **Array** (`Array<T>`)

   * Ordered, indexable, contiguous storage.
   * `index` access `O(1)`, append amortized `O(1)`, insert/delete at arbitrary index `O(n)`.

2. **Set** (`Set<T>`)

   * Unordered collection of unique elements. Backed by hashing.
   * Average `contains`, `insert`, `remove`: `O(1)` (depends on good `Hashable`).

3. **Dictionary** (`Dictionary<Key, Value>`)

   * Key-value mapping with unique keys (hash-based).
   * Average `get/set/remove` `O(1)`; keys must conform to `Hashable`.

**Notes**

* Swift also provides `ArraySlice`, `ContiguousArray` variations and sequence/collection protocols (`Sequence`, `Collection`, `BidirectionalCollection`) to generalize behavior.
* Linked lists, trees, heaps are not built-in but can be implemented or obtained via libraries.

</details>

### 6. Name the types of depth-first traversals and explain how each works ?

<details>
  <summary>Solution</summary>

For a binary tree, the main **depth-first traversals**:

1. **Pre-order (Root, Left, Right)**

   * Visit the current (root) node first, then recursively traverse left subtree, then right subtree.
   * Use-case: copy tree, prefix expression (Polish notation).
   * Pseudocode:

     ```text
     preOrder(node):
       if node == nil: return
       visit(node)
       preOrder(node.left)
       preOrder(node.right)
     ```

2. **In-order (Left, Root, Right)**

   * Traverse left subtree, visit root, traverse right subtree.
   * For binary search trees (BST), in-order yields **sorted order**.
   * Use-case: get sorted elements from BST, infix expression.
   * Pseudocode:

     ```text
     inOrder(node):
       if node == nil: return
       inOrder(node.left)
       visit(node)
       inOrder(node.right)
     ```

3. **Post-order (Left, Right, Root)**

   * Traverse left subtree, then right subtree, then visit root.
   * Use-case: delete/free nodes, evaluate postfix expressions (post-order produces postfix), compute sizes/aggregates from children.
   * Pseudocode:

     ```text
     postOrder(node):
       if node == nil: return
       postOrder(node.left)
       postOrder(node.right)
       visit(node)
     ```

**Complexity**

* Time: `O(n)` for all (visit each node once).
* Space: recursion uses `O(h)` stack where `h` is tree height (worst `O(n)` for skewed tree, `O(log n)` for balanced tree). Iterative versions use an explicit stack.

**Implementation note**

* Depth-first can be implemented recursively (simple) or iteratively with a stack (explicit control).

</details>

### 7. What is the runtime of `contains` on an array ?

<details>
  <summary>Solution</summary>

**`O(n)` (linear time).**

Reason:

* To check whether an element exists in an `Array`, you typically scan elements until you find a match (or reach end). In the worst case you examine all `n` elements.

**Notes**

* Best case `O(1)` (found at first element). Average `O(n/2)` → still `O(n)`.
* If you need frequent membership checks, consider `Set` (average `O(1)`).
* If array is **sorted**, you can do binary search `O(log n)` (e.g., `array.binarySearch()`).

</details>

### 8. What's the runtime of `contains` on a set ?

<details>
  <summary>Solution</summary>

**Average-case: `O(1)`** (constant time).
**Worst-case: `O(n)`** (pathological hash collisions or degenerate hash distribution).

Reason:

* `Set` is hash-based. A lookup computes the item’s hash and inspects a bucket — expected constant-time if the hash function distributes well.
* Worst-case occurs when many values collide into the same bucket or hash function is poor; then lookup can degrade to linear scanning of bucket entries.

**Practical note**

* Swift’s `Set` requires elements conform to `Hashable`. Use good `hash(into:)` implementations for custom types to avoid collisions.

</details>

### 9. What's the requirements of a recursive function ?

<details>
  <summary>Solution</summary>

Two essential requirements:

1. **Base case(s)** — a condition under which the function returns a result directly without making further recursive calls. Prevents infinite recursion.
   Example: `if n == 0 { return 1 }` for factorial.

2. **Recursive case(s)** — the function calls itself with a smaller/simpler input, moving the computation toward the base case.

**Other important points**

* **Progress toward base case:** each recursion should make measurable progress (e.g., `n-1`) so base case is reachable.
* **Stack usage:** each call uses stack frames; deep recursion can cause stack overflow.
* **Tail recursion:** if recursion is tail-call (last operation is recursive call), some languages optimize it. Swift does *not* reliably perform tail-call optimization, so prefer iterative or explicit stack for deep recursion.
* **Correctness & termination:** reason about invariants and ensure termination for all valid inputs.

**Example (factorial):**

```swift
func factorial(_ n: Int) -> Int {
  if n == 0 { return 1 }       // base
  return n * factorial(n - 1)  // recursive
}
```

</details>

### 10. Name a few sorting algorithms and their runtimes ?

<details>
  <summary>Solution</summary>

Here’s a concise list with time/space complexity and notes (n = number of elements):

* **Bubble Sort**

  * Best: `Ω(n)` (if optimized with early exit when already sorted)
  * Average/Worst: `O(n^2)`
  * Space: `O(1)`
  * Stable: yes
  * Use: educational, rarely used in practice.

* **Insertion Sort**

  * Best: `Ω(n)` (already sorted)
  * Average/Worst: `O(n^2)`
  * Space: `O(1)`
  * Stable: yes
  * Use: small arrays or nearly-sorted data; often used as base case for hybrid sorts.

* **Selection Sort**

  * Best/Average/Worst: `O(n^2)`
  * Space: `O(1)`
  * Stable: no (unless implemented carefully)
  * Use: simple but inefficient.

* **Merge Sort**

  * Best/Average/Worst: `O(n log n)`
  * Space: `O(n)` (extra array)
  * Stable: yes
  * Use: guaranteed `O(n log n)`, good for linked lists and external sorting.

* **Quicksort**

  * Average: `O(n log n)`
  * Worst: `O(n^2)` (bad pivot)
  * Space: `O(log n)` recursion average (in-place partitioning)
  * Stable: no (unless modified)
  * Use: very fast in practice for in-memory sorts; choose pivot carefully (random/median-of-three).

* **Heapsort**

  * Best/Average/Worst: `O(n log n)`
  * Space: `O(1)` extra (in-place)
  * Stable: no
  * Use: good worst-case guarantee, does not require extra memory like mergesort.

* **Counting Sort** (non-comparison)

  * Time: `O(n + k)` where `k` is range of input values
  * Space: `O(k)`
  * Stable: yes
  * Use: integer keys in small range.

* **Radix Sort** (non-comparison)

  * Time: `O(n * k)` (k = number of digits/keys)
  * Space: `O(n + k)`
  * Use: sort integers/strings efficiently when k is small.

**Stability & memory trade-offs**

* Stable sorts maintain relative order of equal elements (useful when sorting by multiple keys).
* Non-comparison sorts (counting, radix) can beat `O(n log n)` under restricted input models.

**Practical note**

* Real-world standard library sorts (like Swift’s `sort()` / `sorted()`) use optimized hybrid algorithms that choose efficient strategies, handle small arrays, and ensure good real-world performance.

</details>

### 11. What is divide and conquer and give an algorithm example ?

<details>
  <summary>Solution</summary>

**Divide and Conquer (D\&C)** is a general algorithmic paradigm with three steps:

1. **Divide** the problem into smaller subproblems (usually of the same type).
2. **Conquer**: solve the subproblems (often recursively).
3. **Combine**: merge the subproblem solutions to form a solution for the original problem.

**Examples**

* **Merge Sort** — divide array in half, recursively sort each half, then **merge** sorted halves.

  * Complexity: `O(n log n)` time, `O(n)` space for merging.

* **Quicksort** — partition around a pivot (divide), recursively sort partitions (conquer), combine (trivial because array is partitioned in place).

  * Average: `O(n log n)`.

* **Binary Search** — on a sorted array: divide range in half, decide which half contains the target, recurse on that half.

  * Complexity: `O(log n)`.
  * Example pseudocode:

    ```text
    binarySearch(arr, target):
      low = 0, high = arr.count - 1
      while low <= high:
        mid = (low + high) / 2
        if arr[mid] == target: return mid
        else if arr[mid] < target: low = mid + 1
        else: high = mid - 1
      return not found
    ```

**Why D\&C is powerful**

* Recursion naturally captures the divide/solve/combine steps.
* Often yields logarithmic or `n log n` behavior because you reduce problem size multiplicatively each step.

**Master Theorem** (brief)

* A tool to analyze recurrence relations common in D\&C (e.g., `T(n) = a T(n/b) + f(n)`) to derive asymptotic complexity.

**Practical tip**

* For small subproblems, switch to simpler algorithms (e.g., insertion sort) to reduce overhead — common in hybrid sorts.

</details>
