# [PROJECT-BASED LEARNING 1] Eulerian Circuit

## Introduction
This is Project-based Learning 1 - Computational Programming Project.

The program applies graph theory to find Eulerian circuits in both undirected and directed graphs using Fleury's algorithm and Hierholzer's algorithm.

Additionally, it visualizes the Eulerian circuit on the graph using a graphics library.

## Project Structure
```text
PBL1_EulerianCircuit/
|
├── algorithm.cpp   # Core logic for Eulerian circuit algorithms (Fleury & Hierholzer)
├── algorithm.h      # Function prototypes and declarations for algorithm.cpp
|
├── graphic.cpp      # Graph rendering and step-by-step Eulerian circuit visualization
├── graphic.h        # Function prototypes and declarations for graphic.cpp
|
├── ui.cpp           # User interface (UI) design and layout logic
├── ui.h             # Function prototypes and declarations for ui.cpp
|
|── main.cpp         # Main application entry point
|
└── README.md            
```

## Algorithms Overview

### 1. Fleury's Algorithm

Fleury's algorithm finds an Eulerian circuit by greedily traversing edges and removing them in real time, following the core rule: **"Never cross a bridge unless there is no other choice."**

* **Step 1: Graph Validation & Connectivity Check**
  Verifies overall graph connectivity using a non-recursive Depth-First Search (DFS) with a static array stack. Checks vertex degrees to ensure an Eulerian circuit exists ($odd = 0$ for undirected graphs; $out\_deg(v) = in\_deg(v)$ for all $v$ in directed graphs).
* **Step 2: Initialization & Data Cloning**
  Sets $u = startNode$ and initializes the path array. Clones the original adjacency matrix to a temporary matrix (`temp_adj`) via `memcpy` to protect the initial graph data for visualization.
* **Step 3: Edge Selection & Bridge Check**
  Iterates through adjacent vertices $v$ of $u$. For each candidate edge $(u, v)$, it checks if the edge is a bridge via `isBridge()`:
  * For undirected graphs, it compares reachability before and after removing the edge (`countReachable()`).
  * For directed graphs, it checks if a return path from $v$ back to $u$ exists (`isPathExist()`).
  * If $(u, v)$ is not a bridge, it immediately selects $v$ and breaks the loop. If all remaining edges are bridges, it falls back to selecting the first available edge.
* **Step 4: Edge Deletion & Path Update**
  Appends $v$ to the result path, decrements the edge weight in `temp_adj` (both directions if undirected), and updates $u = v$.
* **Step 5: Completion**
  Repeats Steps 3–4 until no remaining edges exist. Returns the completed Eulerian circuit sequence.

---

### 2. Hierholzer's Algorithm

Hierholzer's algorithm finds an Eulerian circuit in linear time $\mathcal{O}(E)$ by decomposing the graph into sub-cycles and stitching them together using a stack.

* **Step 1: Graph Validation**
  Validates graph connectivity via non-recursive DFS and confirms degree balance requirements for Eulerian existence.
* **Step 2: Stack & Memory Setup**
  Clones the adjacency matrix using `memcpy`. Initializes a static array stack (`stackH`) with $startNode$ pushed onto it, and creates an empty result array (`resH`).
* **Step 3: Sub-cycle Traversal & Backtracking**
  Inspects the top vertex $curr\_v$ of the stack:
  * **Case 1 (Unvisited edges available):** Picks an adjacent vertex $v$, pushes $v$ onto the stack, removes edge $(curr\_v, v)$ from `temp_adj`, and continues traversal to expand the sub-cycle.
  * **Case 2 (Stuck / No available edges):** When $curr\_v$ has no remaining outgoing edges, a sub-cycle is closed. The algorithm pops $curr\_v$ from the stack and appends it to `resH` (backtracking phase).
* **Step 4: Path Reversal & Completion**
  Repeats Step 3 until the stack is empty. Because vertices are collected during backtracking, the sequence in `resH` is reversed. The algorithm reverses `resH` to produce the final, correct chronological Eulerian circuit.
---

### 3. Algorithm Comparison

| Criteria | Fleury's Algorithm | Hierholzer's Algorithm |
| :--- | :--- | :--- |
| **Principle** | Gradually deletes edges, prioritizing avoiding bridges to preserve graph connectivity. | Decomposes the graph into simple cycles, then joins them at common vertices using a Stack. |
| **Time Complexity** | $\mathcal{O}(E \times (V + E))$ | $\mathcal{O}(V \times E)$ |
| **Memory** | Moderate (primarily stores the graph structure). | Slightly higher due to requiring a Stack structure to trace the path. |
| **Pros** | Intuitive and easy to understand from a mathematical logic perspective. | Very fast execution speed; efficient for large-scale graphs. |
| **Cons** | Impractical for large graphs; complex bridge-checking function. | Requires stack management techniques. |




---

## How to Run the Program

Follow these steps to build and run the application in Dev-C++:

1. **IDE & Graphics Library Setup**: 
   * **Dev-C++** IDE installed on your system.
   * **WinBGIm** (Windows BGI extension for C/C++) graphics library installed and correctly linked to Dev-C++ to support the graph visualization features.
2. **Compiler**: **GCC / G++** compiler.

3. **Open the Project / Files**:
   * Launch **Dev-C++**.
   * Open the project file (`.dev`), or open all source files (`algorithm.cpp`, `graphic.cpp`, `ui.cpp`, `main.cpp`) and header files (`.h`) together.
4. **Build and Run**:
   * Open `main.cpp`.
   * Press **F11** (or go to **Execute** ➔ **Compile & Run**).
   * The program will compile the source code, open the interactive graphical UI, and allow you to visualize Fleury's and Hierholzer's algorithms step-by-step.

## Project Members
* **Students**: Ho Tan Phuoc - Phan Huu Tri
* **Instructor**: Dr. Nguyen Van Hieu
* **Course**: Project-based Learning 1 – Computational Programming Project
* **University**: University of Science and Technology - The University of Da Nang
