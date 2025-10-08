## All pairs shortest paths
- Suppose that we wanted to find the Shortest Path between **all** pairs of start and end nodes.
	- Dijkstra does this from one node. 
	- We could run dijkstra for each $V$
	- But this would be an awful solution, with poor performance.
- May be better to run a specific algorithm

## Floyd-warshall algorithm
Dynamic programming algorithm used to achieve all pairs shortest paths, in a weighted graph, works for both directed and undirected graphs.

Algorithm incrementally improves solution. Want to find shortest path, possibly passing through intermediate nodes.

we define $d(i,j,k)$ = Shortest distance from node $i$ to node $j$ using only nodes $\{1,2,...k\}$ as intermediate points 

start by assuming not allowed to use any intermediate nodes.

$d(i, j, 0)$ =
	$w(i, j)$ if there is a direct edge from $i$ to $j$
	$0$ if $i=j$ 
	$\infty$ no known path yet.
	Where k = 0

$d(i, j, k)$ $= min( \ d(i, \ j, \ k-1),\ \ d(i,\ k, \ k-1)+d(k, \ j, k-1) \ )$
Either best path from $i$ to $j$ doesn't use node $k$, and is direct, or it does use it!

![[Pasted image 20250516210118.png]]
![[Pasted image 20250516210509.png]]
![[Pasted image 20250516210552.png]]
![[Pasted image 20250516210804.png]]
![[Pasted image 20250516210815.png]]
![[Pasted image 20250516210827.png]]
![[Pasted image 20250516210849.png]]
Consider new intermediary each time, testing all possible intermediary nodes for shortest paths, until $k$ is exhausted.

![[Pasted image 20250516210441.png]]
FOR EXAM, ALSO NEED TO INITIALISE MATRIX ACCORDING TO BASE CASE FORMULA, 0, DISTANCE, INFINITY.

As such, 3 nested loops of ranges $|V|$, hence $O(|V|^3)$ 


### FW on digraphs/directed graphs
The initial matrix need not be symmetric, but use exactly the same formula.
![[Pasted image 20250516211318.png]]
No longer symmetric as edges are not bidirectional, so for any 2 vertices $i$ and $j$, with a directed edge, $dist[i][j]$ $\neq$ $dist[j][i]$

![[Pasted image 20250516211448.png]]
K can also be equal to i or j, fyi future felix.
![[Pasted image 20250516211719.png]]
REMEBER, CHECKS ALL INTERMEDIATES
![[Pasted image 20250516211906.png]]
WORK THROUGH SOME OF THESE