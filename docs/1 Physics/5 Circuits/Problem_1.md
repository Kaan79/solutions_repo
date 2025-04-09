# Problem 1

# Equivalent Resistance Using Graph Theory

## Introduction

Understanding how to calculate the equivalent resistance of electrical circuits is essential in electrical engineering. Traditional methods rely heavily on applying series and parallel rules iteratively, which can become impractical for complex networks. Instead, we can apply **graph theory**, which provides a systematic and algorithmic approach to analyze circuits using mathematical graph structures.

In this notebook, we will:
- Model circuits using graphs
- Implement algorithms to simplify these graphs
- Visualize circuits and transformations
- Derive equivalent resistances step-by-step using Python

---

## Table of Contents

1. [Mathematical Background](#math)
2. [Graph Representation of Circuits](#graph-representation)
3. [Simplification Algorithm](#algorithm)
4. [Implementation in Python](#implementation)
5. [Examples](#examples)
6. [Efficiency and Limitations](#analysis)
7. [Conclusion](#conclusion)

---

## 1. Mathematical Background 

The behavior of electrical circuits can be rigorously analyzed by modeling them as **weighted undirected graphs**. In this framework, each **node** (vertex) corresponds to a junction or terminal, and each **edge** represents a resistor with a specific resistance value.

Let us define the circuit as a graph:

- $( G = (V, E) ) is an undirected graph.
- $( V )$ is the set of vertices representing junctions or terminals.
- $( E )$ is the set of edges, where each edge $( e_{ij} \in E )$ connects vertices $( i )$ and $( j )$ and is associated with a resistance value $( R_{ij} )$.

### 1.1 Ohm's Law and Linear Resistors

The fundamental law governing resistor behavior is **Ohm’s Law**, which relates voltage $( V )$, current $( I )$, and resistance $(R)$:

$$
V = IR
$$

This linear relationship implies that resistors can be combined algebraically when they are in series or in parallel. This is the basis for the simplification rules used in graph-based analysis.

---

### 1.2 Series Rule

When two or more resistors are connected **in series**, the same current flows through each resistor, and the total voltage is the sum of the voltages across each resistor. Hence, the equivalent resistance is simply the sum of the individual resistances:

If $( R_1, R_2, \ldots, R_n )$ are in series:

$$
R_{eq} = R_1 + R_2 + \cdots + R_n
$$

** Example:**

Three resistors in series:  
$( R_1 = 2\,\Omega )$, $( R_2 = 3\,\Omega )$, $( R_3 = 5\,\Omega )$

Then:

$$
R_{eq} = 2 + 3 + 5 = 10\,\Omega
$$

---

### 1.3 Parallel Rule

When two or more resistors are connected **in parallel**, the voltage across each resistor is the same, but the total current is the sum of the currents through each branch.

For two resistors:

$$
\frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2}
$$

Solving for $$( R_{eq} )$$:

$$
R_{eq} = \frac{R_1 R_2}{R_1 + R_2}
$$

For $( n )$ resistors in parallel:

$$
\frac{1}{R_{eq}} = \sum_{i=1}^{n} \frac{1}{R_i}
$$

**Example:**

Two resistors in parallel:  
$( R_1 = 4\,\Omega )$, $( R_2 = 6\,\Omega )$

Then:

$$
\frac{1}{R_{eq}} = \frac{1}{4} + \frac{1}{6} = \frac{5}{12} \Rightarrow R_{eq} = \frac{12}{5} = 2.4\,\Omega
$$

---

### 1.4 Mixed Configurations

Most practical circuits are **combinations of series and parallel** resistors. These mixed configurations must be reduced step-by-step by identifying local patterns.

**Example:**

A combination:

- $( R_1 = 3\,\Omega )$ and $( R_2 = 6\,\Omega )$ in parallel:

$$
R_p = \frac{3 \cdot 6}{3 + 6} = 2\,\Omega
$$

- Then, $( R_p )$ in series with $( R_3 = 5\,\Omega )$:

$$
R_{eq} = 2 + 5 = 7\,\Omega
$$

---

### 1.5 Graph-Theoretic Implication

In a graph-based model:

- **Series combinations** can be detected by finding a node of degree 2 that is not a source or sink and replacing the adjacent edges with a single edge.
- **Parallel combinations** appear as **multi-edges** (multiple edges between the same two nodes) or **cycles** with identical endpoints.

Graph reduction techniques involve **edge merging**, **node contraction**, and simplification rules derived from these principles.


## 2. Graph Representation of Circuits

To analyze electrical circuits using graph theory, we must first translate the physical components of a circuit into a **graph structure**. This allows us to apply mathematical and algorithmic techniques for simplification and analysis.

We use the Python library `networkx` to model circuits as graphs, and `matplotlib` to visualize them.

###  Required Libraries

```python
import networkx as nx
import matplotlib.pyplot as plt
```

- `networkx`: Used to create and manipulate the graph (nodes = junctions, edges = resistors).
- `matplotlib.pyplot`: Used to draw and display the circuit graph.

---

### Building the Graph

Each **node** in the graph represents a **junction** or connection point (e.g., wires meeting at a point).

Each **edge** represents a **resistor**, with an attribute `resistance` that stores its value in ohms (Ω).

```python
G = nx.Graph()
G.add_edge("A", "B", resistance=4)
G.add_edge("B", "C", resistance=2)
G.add_edge("A", "C", resistance=4)
```

- This creates three connections:
  - Between A and B with 4Ω
  - Between B and C with 2Ω
  - Between A and C with 4Ω

These represent a **triangle** configuration — a common form where multiple current paths exist (useful for demonstrating parallel and nested structures).

---

###  Visualizing the Circuit Graph

Once the graph is built, we can visualize it using `matplotlib`.

```python
pos = nx.spring_layout(G)  # Automatically position nodes
nx.draw(G, pos, with_labels=True, node_size=2000)  # Draw nodes
labels = nx.get_edge_attributes(G, 'resistance')   # Get resistance values
nx.draw_networkx_edge_labels(G, pos, edge_labels=labels)  # Label edges
plt.title("Circuit Graph")
plt.show()
```

#### Explanation:
- `spring_layout`: Computes visually appealing positions for nodes.
- `with_labels=True`: Labels each node with its name ("A", "B", etc.).
- `edge_labels=labels`: Displays resistance values directly on the edges.
- `plt.title`: Adds a title to the graph.

---

### Interpretation of the Graph

The resulting graph will be a triangle connecting nodes A, B, and C, with labeled resistances:

- A to B: 4Ω  
- B to C: 2Ω  
- A to C: 4Ω  

This configuration represents a **simple closed-loop circuit**. Depending on which two points are chosen as terminals (input/output), this can represent:
- A parallel configuration (A-B and A-C paths)
- A nested series-parallel network (A → B → C vs. A → C)

---

###  Why Graphs?

Modeling circuits as graphs provides several advantages:
- Easier identification of **series and parallel** components.
- Enables **automated simplification** using algorithms.
- Ready to integrate with **simulation tools** and **matrix-based methods** (like Kirchhoff analysis).

In the next sections, we will explore how to **simplify** this graph by identifying and reducing resistors step-by-step.



## 3. Simplification Algorithm <

### Goal:
Reduce the graph to a single equivalent edge between source and sink.

### Key Steps:
1. Detect and merge series resistors
2. Detect and merge parallel resistors
3. Repeat until only two nodes remain (start and end)

### Pseudocode:
```text
while graph has more than 2 nodes:
    for node in graph:
        if node has degree 2:
            if both neighbors are not connected:
                merge as series
        if two edges share same endpoints:
            merge as parallel
```

---

## 4. Implementation in Python

We'll implement helper functions to:
- Detect series and parallel structures
- Merge edges
- Track steps

```python
def equivalent_resistance(graph, source, sink):
    # Simplification loop
    while True:
        changed = False
        # Handle series
        for node in list(graph.nodes):
            if graph.degree[node] == 2 and node not in [source, sink]:
                neighbors = list(graph.neighbors(node))
                if not graph.has_edge(neighbors[0], neighbors[1]):
                    R1 = graph[node][neighbors[0]]['resistance']
                    R2 = graph[node][neighbors[1]]['resistance']
                    graph.add_edge(neighbors[0], neighbors[1], resistance=R1 + R2)
                    graph.remove_node(node)
                    changed = True
                    break
        # Handle parallel
        for u, v in list(graph.edges()):
            parallels = [(u_, v_) for u_, v_ in graph.edges() if set([u_, v_]) == set([u, v])]
            if len(parallels) > 1:
                total = sum([1 / graph[a][b]['resistance'] for a, b in parallels])
                Req = 1 / total
                for a, b in parallels:
                    graph.remove_edge(a, b)
                graph.add_edge(u, v, resistance=Req)
                changed = True
                break
        if not changed:
            break
    return graph[source][sink]['resistance']
```

---

## 5. Examples 

### Example 1: Simple Series

A --(2Ω)-- B --(3Ω)-- C

Result:
```python
Expected: 5Ω
```

### Example 2: Simple Parallel

A --(2Ω)-- B
A --(3Ω)-- B

Result:
```python
Expected: 1.2Ω
```

### Example 3: Nested Configuration

```
        A
       / \
     2Ω  4Ω
     /     \
    B       C
     \     /
      2Ω  1Ω
         \
           D
```

Code and visualization will follow for each.

---

## 6. Efficiency and Limitations

While graph theory provides a powerful and general framework for analyzing circuits, its implementation has both computational and structural trade-offs that must be considered. This section reviews the **time complexity**, **algorithmic bottlenecks**, and **practical limitations**—along with suggestions for future improvements.

---

### Time Complexity

The core idea of our algorithm is to iteratively search for and reduce **series** and **parallel** resistor patterns within the graph. At each iteration, the graph is simplified step-by-step.

- **Series detection**: Requires checking if a node has exactly two neighbors (degree = 2). This can be done in \( O(n) \), where \( n \) is the number of nodes.
- **Parallel detection**: Requires checking for multi-edges or duplicate edge endpoints, which also takes \( O(m) \), where \( m \) is the number of edges.

Since simplification is done **iteratively**, the **worst-case complexity** could approach:

$$
O(k \cdot (n + m))
$$

Where:
- $( k )$ = number of simplification passes (in deeply nested circuits, this could be large),
- $( n )$ = number of nodes,
- $( m )$ = number of edges.

---

### 🔁 Example of Iterative Passes

Consider a nested configuration like this:

- A triangle $( A \leftrightarrow B \leftrightarrow C \leftrightarrow A )$
- Each edge is made up of **two resistors in series**
- All triangle edges are connected in **parallel**

To simplify:

1. **First Pass**: Each series pair must be merged → results in 3 single resistors
2. **Second Pass**: Triangle must be reduced using parallel formula → results in one equivalent resistor

Thus, even for small graphs, **multiple iterations** are often required.

---

### Limitations

Despite the flexibility of the method, there are several limitations to be aware of:

####  1. Non-linear Components
- Devices like **diodes**, **transistors**, or **capacitors** have **non-linear or time-dependent** behavior.
- These components cannot be modeled as static resistors and require **differential equations** or **piecewise models**, which are beyond the scope of our graph simplification approach.

####  2. No Automatic Terminal Identification
- The algorithm assumes that the **source and sink nodes are known in advance** (e.g., “A” and “D”).
- In general circuit analysis, **determining valid input/output nodes** from arbitrary topologies requires an additional **pre-processing step**.

####  3. Cycles and Bridges
- Graphs with complex cycles or bridge connections (edges whose removal increases the number of connected components) may require symbolic tracking.
- Sometimes **series and parallel rules are insufficient** for full simplification, especially in **non-planar** graphs.

---

###  Potential Improvements

Several algorithmic and symbolic enhancements can extend this method:

####  1. Graph Contraction Heuristics
Use more advanced **graph theory operations** like:
- **Edge contraction** (for series nodes)
- **Bridge detection** and **cycle basis analysis** (for parallel and redundant paths)

These can accelerate simplification in sparse or dense networks.

#### 2. Symbolic Computation
- Use symbolic libraries (e.g., SymPy) to track resistance values algebraically:
  - Supports **variable resistors** like $( R_1 = x )$, $( R_2 = 2x )$
  - Allows solving for **equivalent resistance as a function**

#### 3. Kirchhoff-Based Matrix Analysis
- Construct **incidence**, **adjacency**, or **Laplacian matrices** from the graph
- Use them to solve for voltages and currents using **Kirchhoff’s Laws**
- Especially useful when simplification rules are not enough (e.g., for bridge networks)

---

### Summary

| Aspect | Description |
|--------|-------------|
| **Best Case** | Simple series/parallel → $( O(n) )$ |
| **Worst Case** | Nested & cyclic circuits → multiple passes $( O(k \cdot (n + m)) )$ |
| **Weaknesses** | Non-linear components, undefined terminals |
| **Extensions** | Symbolic algebra, matrix methods, edge contraction |

This efficiency and limitation analysis helps define the **scope** of graph-based resistance computation and guides us toward **next-generation circuit analysis tools**.


## 7. Conclusion

Graph theory provides a structured, scalable, and algorithmically elegant framework for analyzing and simplifying electrical circuits. By abstracting physical components like resistors into a mathematical graph—where **nodes represent junctions** and **edges represent resistors with weights**—we are able to systematically reduce even highly complex configurations.

This graph-based methodology offers several key advantages over traditional techniques:

-  It enables **automated simplification** of circuits using computational algorithms.
-  It naturally handles **nested combinations** and ambiguous topologies without requiring visual intuition.
-  It leverages well-established graph operations, such as edge contraction, cycle detection, and multi-edge reduction, to mirror physical simplifications like series and parallel reduction.

---

###  Bridging Disciplines

This approach beautifully bridges the disciplines of:

- **Electrical Engineering**, which provides the physical and theoretical background (Ohm's Law, Kirchhoff's Laws),
- **Computer Science**, which contributes data structures (graphs) and algorithmic strategies (search, recursion, reduction),
- **Mathematics**, which supplies tools like matrix representations, graph theory, and symbolic algebra.

Such interdisciplinary fusion not only improves the efficiency of circuit analysis, but also opens the door for innovations in:

- 📐 **Circuit design automation**
-  *Simulation software**
-  **Education and visualization tools**
-  **Optimization of electrical networks**

---

###  Future Outlook

As circuits become more complex, incorporating **nonlinear elements**, **time-dependent behaviors**, and **programmable logic**, the need for **advanced symbolic methods**, **graph-based simulations**, and **AI-driven optimizers** will become even more pronounced.

Graph theory sets the foundation for these developments, offering a clear, formal language to describe and manipulate electrical systems.

---

###  Final Thoughts

Studying equivalent resistance through graph theory is more than just an academic exercise—it is a practical, future-ready approach that enhances both understanding and capability.

> In a world where systems are becoming increasingly complex, the ability to model them simply and solve them algorithmically is not just useful—it's essential.

---
