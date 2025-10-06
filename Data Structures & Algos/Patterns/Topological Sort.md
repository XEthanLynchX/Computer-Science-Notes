## **Overview**

Topological Sort is a linear ordering of vertices in a **Directed Acyclic Graph (DAG)** such that for every directed edge u→vu \rightarrow vu→v, vertex uuu comes before vvv in the ordering. It’s commonly used in scenarios like task scheduling, course prerequisite resolution, and build systems.

---

## **What Does it Look Like?**

Given a DAG like this:

```
5 → 0 ← 4
↓         ↑
2         1
↓
3
```

A valid topological sort could be:  
**4, 5, 2, 3, 1, 0**  
(Note: Multiple valid topological orders may exist.)

---

## **How to Recognize This Pattern**

You’re likely dealing with a topological sort problem if:

- The graph is **directed** and **acyclic**.
- The problem involves **dependencies** (e.g., "do X before Y").
- You’re asked to **order tasks** or **resolve prerequisites**.

Common keywords: _dependencies_, _build order_, _task scheduling_, _prerequisites_, _DAG_.

---

## **Example Implementation**

Here’s a Python implementation using **Kahn’s Algorithm** (BFS-based):


```Python

from collections import defaultdict, deque

def topological_sort(num_nodes, edges):
    graph = defaultdict(list)
    in_degree = [0] * num_nodes

    # Build graph and compute in-degrees
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1

    # Queue for nodes with no incoming edges
    queue = deque([i for i in range(num_nodes) if in_degree[i] == 0])
    topo_order = []

    while queue:
        node = queue.popleft()
        topo_order.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    if len(topo_order) == num_nodes:
        return topo_order
    else:
        return []  # Cycle detected, no valid topological sort
```

Example usage:
```python 
edges = [(5, 0), (5, 2), (4, 0), (4, 1), (2, 3), (3, 1)]
print(topological_sort(6, edges))
# Output: [4, 5, 2, 3, 1, 0] or another valid order
```

---

## **Applied Problems**

Here are some classic problems where topological sort is used:

- **Course Schedule (Leetcode 207)**
- **Alien Dictionary (Leetcode 269)**
- **[Build System Dependency Resolution]**
- **[Task Scheduling with Dependencies]**