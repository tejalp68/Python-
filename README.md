# GATE DA — Python & DSA Complete Notes

*Covers: Programming in Python, Data Structures, Searching, Sorting, Graph Theory — as per GATE DA official syllabus*

---

## 1. Python Basics & Syntax

- Python is **interpreted**, **dynamically typed**, and **indentation-sensitive** (blocks defined by whitespace, not braces).
- Statements end at newline; no semicolon required (though allowed to separate statements on one line).
- Comments: `#` for single line, `'''...'''` or `"""..."""` for multi-line (technically a string literal, treated as comment when unassigned).
- Variables need no declaration; type is inferred at assignment and can change (`x = 5; x = "hi"` is valid).
- `id()` returns the memory address (identity) of an object; `type()` returns its type.

**GATE trace tip:** Predicting output often hinges on knowing whether an operation creates a *new object* or *mutates in place*.

---

## 2. Data Types & Variables

| Type | Mutable? | Example |
|---|---|---|
| `int` | No | `5` |
| `float` | No | `3.14` |
| `complex` | No | `2+3j` |
| `bool` | No | `True`, `False` (subclass of int: `True == 1`) |
| `str` | No | `"abc"` |
| `list` | **Yes** | `[1,2,3]` |
| `tuple` | No | `(1,2,3)` |
| `set` | **Yes** | `{1,2,3}` |
| `frozenset` | No | `frozenset({1,2})` |
| `dict` | **Yes** | `{"a":1}` |
| `NoneType` | No | `None` |

**Key GATE traps:**
- `True + True` → `2` (bool is int subclass).
- Tuples are immutable, but a tuple *containing* a list can have that list's contents changed: `t = (1, [2,3]); t[1].append(4)` → works, `t` becomes `(1, [2,3,4])`.
- Type coercion in mixed expressions: `int + float → float`; `bool` in arithmetic becomes `0`/`1`.

### Type Conversion
- Implicit: `int → float` in mixed arithmetic.
- Explicit: `int()`, `float()`, `str()`, `list()`, `tuple()`, etc.
- `int("10", 2)` → converts binary string "10" to decimal → `2`.

---

## 3. Operators

### Arithmetic
```
+  -  *  /   → / always returns float
//  → floor division (rounds toward negative infinity, NOT toward zero!)
%   → modulo (sign follows the divisor in Python)
**  → exponentiation
```
**Classic GATE trap:**
```python
-7 // 2   # -4  (floors toward -infinity, not -3)
-7 % 2    # 1   (result has same sign as divisor)
7 % -2    # -1
```

### Comparison
`==`, `!=`, `<`, `>`, `<=`, `>=` — compare **value**.

### Identity vs Equality
- `==` compares value; `is` compares identity (memory location).
- Small int caching: Python caches integers −5 to 256, so `a = 100; b = 100; a is b` → `True`, but `a = 1000; b = 1000; a is b` → `False` (implementation-dependent, but commonly tested).
- String interning follows similar (but not identical) caching rules.

### Logical
`and`, `or`, `not` — short-circuit evaluation. `and`/`or` return an **operand**, not just `True`/`False`:
```python
print(0 or "hello")   # "hello"
print(3 and 5)         # 5
```

### Bitwise
`&`, `|`, `^`, `~`, `<<`, `>>` — operate on integer binary representation. `~x` = `-x - 1`.

### Operator Precedence (high → low, commonly tested)
`**` > unary `+/-` > `*, /, //, %` > `+, -` > bitwise shifts > comparisons > `not` > `and` > `or`

---

## 4. Control Flow

### if / elif / else
Standard branching; no `switch` statement in Python (pre-3.10). Python 3.10+ has `match-case` (structural pattern matching) — know its syntax if using 3.10+.

### Loops
```python
for i in range(start, stop, step):   # stop is exclusive
while condition:
```
- `range(5)` → 0,1,2,3,4
- `range(1, 10, 2)` → 1,3,5,7,9
- `range(10, 0, -2)` → 10,8,6,4,2

### break, continue, else on loops
- `else` block on a `for`/`while` runs **only if the loop completes without a `break`**.
```python
for i in range(5):
    if i == 10:
        break
else:
    print("completed")   # this WILL print, since break never triggered
```
This is a **high-frequency GATE trace question**.

---

## 5. Functions

### Basics
```python
def f(a, b=5, *args, **kwargs):
```
- `b=5` → default argument.
- `*args` → collects extra positional args into a tuple.
- `**kwargs` → collects extra keyword args into a dict.

### The Mutable Default Argument Trap (very common GATE question)
```python
def f(x, lst=[]):
    lst.append(x)
    return lst

print(f(1))   # [1]
print(f(2))   # [1, 2]  <-- NOT [2]! Default list persists across calls
```
Default arguments are evaluated **once**, at function definition time, not each call.

### Scope — LEGB Rule
Local → Enclosing → Global → Built-in (search order for name resolution).
- `global` keyword: modify a global variable inside a function.
- `nonlocal` keyword: modify a variable in an enclosing (non-global) scope, used in nested functions/closures.

```python
def outer():
    x = 10
    def inner():
        nonlocal x
        x += 1
    inner()
    print(x)   # 11
```

### Recursion
- Trace using a **call stack** / recursion tree.
- Base case + recursive case.
- Watch for stack overflow with no base case (`RecursionError`).
- Common GATE questions: trace factorial, Fibonacci, or tricky mutual recursion; compute number of function calls.

### Lambda functions
`lambda args: expression` — anonymous, single-expression functions. Often combined with `map()`, `filter()`, `sorted(key=...)`.

---

## 6. Core Data Structures

### Lists
- Ordered, mutable, allow duplicates, heterogeneous elements.
- Key methods: `append(x)` (O(1) amortized), `insert(i,x)` (O(n)), `pop()` (O(1) from end, O(n) from front/middle), `remove(x)` (removes first match, O(n)), `extend()`, `sort()` (in-place, Timsort, O(n log n)), `sorted()` (returns new list), `reverse()`.
- List comprehension: `[x**2 for x in range(5) if x % 2 == 0]`

**Shallow vs Deep Copy:**
```python
a = [1, [2, 3]]
b = a          # same object (alias)
c = a.copy()   # shallow copy: new outer list, but inner list still shared
import copy
d = copy.deepcopy(a)  # fully independent copy
```

### Tuples
- Ordered, **immutable**, allow duplicates.
- Faster than lists, used as dict keys (since hashable) when elements are all hashable.
- Tuple packing/unpacking: `a, b = 1, 2`; `a, *rest = [1,2,3,4]` → `rest = [2,3,4]`.

### Sets
- Unordered, mutable, **no duplicates**, elements must be hashable.
- Operations: `union (|)`, `intersection (&)`, `difference (-)`, `symmetric_difference (^)`.
- O(1) average membership testing (`in`), unlike O(n) for lists.

### Dictionaries
- Key-value pairs, keys must be hashable & unique, values can be anything.
- Since Python 3.7+, dicts preserve **insertion order**.
- Average O(1) lookup/insert/delete (hash table underneath).
- Methods: `get()`, `keys()`, `values()`, `items()`, `pop()`, `setdefault()`.
- Dict comprehension: `{k: v**2 for k, v in d.items()}`

---

## 7. Strings

- Immutable sequence of Unicode characters.
- Every "modification" method (`.upper()`, `.replace()`, etc.) returns a **new** string.
- Concatenation in a loop (`s += x`) is O(n²) total due to immutability — a common efficiency-trap question.
- Slicing works exactly like lists: `s[::-1]` reverses, `s[1:5]`, etc.
- `.split()`, `.join()`, `.strip()`, `.find()` vs `.index()` (find returns -1 if not found; index raises exception).
- String formatting: f-strings `f"{x}"`, `.format()`, `%` operator.

---

## 8. Mutability & Object References (High-Yield Trace Topic)

- Python passes **object references by value** ("pass by object reference" / "call by sharing").
- If you pass a mutable object and mutate it inside a function, the change is visible outside.
- If you **rebind** the parameter to a new object inside the function, the outer variable is unaffected.

```python
def modify(lst):
    lst.append(4)      # mutates shared object -> visible outside

def rebind(lst):
    lst = [9, 9, 9]     # rebinds local name -> NOT visible outside

x = [1, 2, 3]
modify(x)   # x becomes [1, 2, 3, 4]
rebind(x)   # x remains [1, 2, 3, 4] (unchanged)
```

---

## 9. Object-Oriented Programming

- `class`, `__init__` (constructor), `self` (refers to instance).
- Inheritance: `class Child(Parent):` ; `super().__init__()` calls parent constructor.
- Method Resolution Order (MRO) for multiple inheritance — uses C3 linearization; check with `ClassName.__mro__`.
- **Dunder (magic) methods** — let objects integrate with built-in syntax:

| Method | Triggered by |
|---|---|
| `__init__` | Object creation |
| `__str__` / `__repr__` | `print(obj)` / `repr(obj)` |
| `__len__` | `len(obj)` |
| `__getitem__` | `obj[i]`, slicing |
| `__setitem__` | `obj[i] = val` |
| `__eq__` | `obj1 == obj2` |
| `__add__` | `obj1 + obj2` |
| `__iter__` / `__next__` | iteration, `for` loops |

- Class variables (shared across instances) vs instance variables (per-object) — a common trace trap:
```python
class C:
    count = 0          # class variable
    def __init__(self):
        C.count += 1   # shared across all instances
```

---

## 10. Exception Handling

```python
try:
    ...
except ValueError as e:
    ...
except (TypeError, KeyError):
    ...
else:
    ...        # runs only if no exception occurred
finally:
    ...        # always runs
```
- Common built-in exceptions: `ZeroDivisionError`, `IndexError`, `KeyError`, `TypeError`, `ValueError`, `AttributeError`, `NameError`, `StopIteration`.
- `finally` executes even if there's a `return` in `try`/`except`.

---

## 11. Iterators & Generators

- **Iterable**: has `__iter__()` (e.g., list, tuple, dict, string).
- **Iterator**: has both `__iter__()` and `__next__()`; raises `StopIteration` when exhausted.
- `iter(obj)` gets an iterator; `next(it)` advances it.
- **Generator functions** use `yield` instead of `return`; they produce values lazily (one at a time), preserving state between calls.
```python
def gen():
    yield 1
    yield 2
    yield 3
g = gen()
next(g)  # 1
next(g)  # 2
```
- **Generator expression**: `(x**2 for x in range(5))` — like list comprehension but lazy, memory-efficient.

---

## 12. Searching Algorithms

### Linear Search
- Check each element sequentially.
- Time complexity: O(n) worst/average case.
- Works on unsorted data.

### Binary Search
- Requires **sorted** array.
- Repeatedly divide search interval in half.
- Time complexity: O(log n).
```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```
**GATE favorite:** trace mid-value computation and number of iterations for a given array/target.

---

## 13. Sorting Algorithms

| Algorithm | Best | Average | Worst | Space | Stable? |
|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |

### Bubble Sort
Repeatedly swap adjacent elements if out of order; largest "bubbles" to the end each pass.

### Selection Sort
Find the minimum of the unsorted portion, swap it into place; repeat.

### Insertion Sort
Build sorted portion one element at a time, inserting each new element into its correct position.

### Merge Sort (Divide & Conquer)
Recursively split array in half, sort each half, then merge two sorted halves. Guaranteed O(n log n), extra O(n) space.

### Quick Sort (Divide & Conquer)
Choose a pivot, partition array into elements less-than and greater-than pivot, recursively sort each partition. Average O(n log n); worst case O(n²) if pivot choices are poor (e.g., already-sorted array with first-element pivot).

**GATE favorite:** trace array state after each pass/partition; compute number of comparisons/swaps.

---

## 14. Stacks & Queues

### Stack (LIFO)
- Operations: `push` (add to top), `pop` (remove from top), `peek/top`. All O(1).
- Python implementation: use a `list` with `append()`/`pop()` (both operate on the end — O(1)).
- Applications: expression evaluation (infix→postfix), balanced parentheses, function call stack, undo operations, DFS.

### Queue (FIFO)
- Operations: `enqueue` (add to rear), `dequeue` (remove from front). O(1) each with the right structure.
- **Avoid using `list.pop(0)`** for dequeue — it's O(n)! Use `collections.deque` instead (O(1) both ends).
```python
from collections import deque
q = deque()
q.append(1)      # enqueue
q.popleft()       # dequeue
```
- Variants: Circular Queue, Priority Queue (often via `heapq`), Deque (double-ended queue).
- Applications: BFS, task scheduling, buffering.

---

## 15. Linked Lists

- Nodes containing `data` + pointer(`next`) to next node; no contiguous memory required (unlike arrays).
- **Singly Linked List**: one-directional traversal.
- **Doubly Linked List**: nodes have `prev` and `next` — bidirectional traversal.
- **Circular Linked List**: last node points back to the first.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

| Operation | Array | Linked List |
|---|---|---|
| Access by index | O(1) | O(n) |
| Insert/Delete at known position | O(n) (shifting) | O(1) |
| Insert/Delete at beginning | O(n) | O(1) |
| Search | O(n) | O(n) |

**GATE favorite:** trace pointer manipulations for insertion/deletion/reversal; detect cycles (Floyd's Cycle Detection / "tortoise and hare").

---

## 16. Trees

### Binary Tree
Each node has at most 2 children (`left`, `right`).

### Binary Search Tree (BST)
- Left subtree values < node < right subtree values.
- Search/Insert/Delete: O(log n) average (balanced), O(n) worst case (skewed/degenerate tree, essentially a linked list).

### Tree Traversals (high-frequency GATE topic)
- **Inorder** (Left, Root, Right) → gives sorted order for a BST.
- **Preorder** (Root, Left, Right) → used to copy/serialize a tree.
- **Postorder** (Left, Right, Root) → used to delete a tree / evaluate expression trees.
- **Level-order** (BFS using a queue) → level by level.

```python
def inorder(root):
    if root:
        inorder(root.left)
        print(root.data)
        inorder(root.right)
```

**GATE favorite:** given a traversal sequence (or two of them), reconstruct the tree or determine another traversal order; compute height/depth; check BST validity.

---

## 17. Hash Tables

- Store key-value pairs using a **hash function** to compute an index (bucket) for each key.
- Average O(1) insert/search/delete; worst case O(n) with poor hash function / many collisions.
- **Collision resolution:**
  - *Chaining*: each bucket holds a linked list of entries.
  - *Open addressing*: probe for next free slot (linear probing, quadratic probing, double hashing).
- **Load factor** = number of entries / number of buckets; high load factor → more collisions → resizing ("rehashing") needed.
- Python's `dict` and `set` are hash-table implementations under the hood.

---

## 18. Graph Theory

### Representations
- **Adjacency Matrix**: 2D array, `matrix[i][j] = 1` if edge exists. O(V²) space, O(1) edge lookup.
- **Adjacency List**: list/dict of neighbors per vertex. O(V+E) space, better for sparse graphs.

### Terminology
- Directed vs Undirected; Weighted vs Unweighted.
- Degree of a vertex (in-degree/out-degree for directed graphs).
- Path, Cycle, Connected Graph, Connected Components.
- Tree = connected, acyclic graph with V-1 edges.

### Graph Traversal
**BFS (Breadth-First Search)**
- Uses a **queue**; explores level by level.
- Finds shortest path in **unweighted** graphs.
- Time: O(V + E).
```python
from collections import deque
def bfs(graph, start):
    visited, queue, order = {start}, deque([start]), []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return order
```

**DFS (Depth-First Search)**
- Uses a **stack** (explicit or via recursion); explores as deep as possible before backtracking.
- Time: O(V + E).
- Used for cycle detection, topological sort, connected components.

**GATE favorite:** trace BFS/DFS visit order given an adjacency list and a starting vertex (note: order depends on how neighbors are listed/sorted).

### Shortest Path — Dijkstra's Algorithm
- Finds shortest paths from a source to all vertices in a **weighted graph with non-negative edge weights**.
- Greedy approach: repeatedly pick the unvisited vertex with the smallest known distance, relax its edges.
- Typically implemented with a **min-heap (priority queue)**.
- Time complexity: O((V + E) log V) with a binary heap.
- **Does not work correctly with negative edge weights** (use Bellman-Ford instead — good to know even if not in syllabus, for conceptual contrast).

```python
import heapq
def dijkstra(graph, start):
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    pq = [(0, start)]
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]:
            continue
        for neighbor, weight in graph[node]:
            nd = d + weight
            if nd < dist[neighbor]:
                dist[neighbor] = nd
                heapq.heappush(pq, (nd, neighbor))
    return dist
```

**GATE favorite:** hand-trace Dijkstra step by step on a small weighted graph, tracking the distance table update at each iteration.

---

## Quick Revision Checklist

- [ ] Indexing & slicing (positive/negative, step)
- [ ] Operator precedence, floor division/modulo with negatives
- [ ] Mutable default argument trap
- [ ] LEGB scope, global/nonlocal
- [ ] Shallow vs deep copy
- [ ] Pass-by-object-reference (mutate vs rebind)
- [ ] `is` vs `==`, small-int caching
- [ ] Dunder methods (`__getitem__`, `__len__`, `__iter__`)
- [ ] for-else loop semantics
- [ ] Generators vs list comprehensions
- [ ] Stack/Queue via list vs deque (Big-O pitfalls)
- [ ] Linked list pointer tracing, cycle detection
- [ ] BST properties & all 4 traversal types
- [ ] Hash table collision handling
- [ ] BFS vs DFS (queue vs stack), when each finds shortest path
- [ ] Sorting algorithm complexities + stability + trace passes
- [ ] Binary search trace + Dijkstra trace

---

**Study strategy:** After each topic above, immediately solve GATE DA PYQs (2021–2026) on that exact topic — code-tracing skill is built through repetition, not just reading theory.
