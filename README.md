README — DAA Assignment 4

Short purpose
-------------
This project implements Tarjan's SCC, condensation (SCC -> DAG), two topological sorts (Kahn and DFS), and DAG shortest/longest path algorithms. The repository includes JUnit tests that check correctness.

Data summary
------------
All datasets are directed and use edge weights.
- small_cyclic.json — n=7
- small_dag.json — n=9
- small_multi_scc.json — n=10
- medium_cyclic.json — n=14
- medium_dag.json — n=16
- medium_dense_scc.json — n=19
- large_cyclic_sparse.json — n=28
- large_dag_dense.json — n=35
- large_multi_scc.json — n=45

How to reproduce (quick)
-----------------------
Requirements: Java (match `pom.xml`) and Maven in PATH.
Run tests:
```powershell
mvn test
```
Run the main pipeline to print metrics:
```powershell
mvn -DskipTests=true compile exec:java -Dexec.mainClass=com.anuar.Main
```
Find per-test timings in `target/surefire-reports/`.

What to measure (brief)
----------------------
For each dataset collect: n, m (edges), number of SCCs, condensation nodes, topo valid (true/false), DAG shortest/longest results (critical path length), and elapsed time. The project has a `Metrics` helper used by algorithms — use its outputs or Maven surefire reports.

Concise analysis
----------------
- Complexity: Tarjan, condensation, Kahn/DFS, and DAG-SP are all O(V + E).
- Bottlenecks: edge count (E) is dominant. Dense graphs (E ≈ V^2) are much slower and use more memory.
- Structure effects:
  - Large SCCs reduce the size of the condensation graph, making subsequent topo and DAG-SP cheaper.
  - When SCCs are size 1 (no cycles), condensation equals original graph and work remains large.
  - Tarjan uses recursion; very deep graphs may need larger JVM stack or iterative rewrite.

Conclusions & recommendations
----------------------------
- Use Tarjan+condensation when the graph may have cycles and you want to run DAG algorithms after collapse.
- Use Kahn for iterative, non-recursive topological sorting; use DFS-based topo when code brevity matters and recursion depth is safe.
- Deduplicate edges when building the condensed DAG to reduce unnecessary work.
