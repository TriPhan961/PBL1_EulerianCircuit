This project focuses on the research and implementation of algorithms to find Eulerian circuits and paths in graphs using the C programming language. 
It bridges theoretical graph theory with practical application, featuring a real-time graphical simulation to help users intuitively understand algorithmic behavior.

* Key Features:
- Diverse Graph Models: Full support for both undirected and directed graphs.
- Flexible Input Methods: Allows for direct data entry via the console or reading from structured text files (.txt).
- Optimized Algorithms: Features two classic approaches:
  + Fleury's Algorithm: A "bridge-avoiding" strategy with a complexity of O(E×(V+E)).
  + Hierholzer's Algorithm: A high-performance strategy using a Stack and sub-circuit merging with a complexity of O(V×E).
- Visual Simulation: Integrated graphical interface to draw graphs, highlight current paths, and update execution status in real-time.

* Tech Stack:
- Language: C.
- Graphics Library: winbgim.h (C++ Graphics Library for Windows).
- Data Structures: Adjacency Matrix, Stack.
- Code Presentation: The source code is optimized for readability using Syntax Highlighting principles found in modern IDEs like VS Code

* Project Structure:
- main.cpp: Core execution flow and menu navigation.
- algorithm.cpp: Implementation of Fleury and Hierholzer algorithms.
- graphic.cpp: Handles graph rendering, vertex coordinate calculation (Circular Layout), and animation logic.
- ui.cpp: Manages console-based user interaction.

* Installation and Usage:
- System Requirements: A C++ compiler supporting the winbgim.h library (e.g., Dev-C++ or Code::Blocks with configured graphics).
- Execution:
  + Clone or download the source code.
  + Open the project file in your IDE.
  + Press F11 to compile and run.
- Data Input: Standard .txt file format:
  + Line 1: Number of vertices n and number of edges m.
  + Next m lines: Pairs of vertices (u,v) representing an edge.

* Algorithm Analysis:
The project includes a benchmarking feature to compare the execution times of both algorithms.
Experimental results demonstrate that Hierholzer’s algorithm is significantly faster on dense graphs due to its efficient stack-based sub-circuit merging mechanism.
