
![[Pasted image 20250517203119.png]]
Left, also given by applying prim's

![[Pasted image 20250517203146.png]]
Right
![[Pasted image 20250517203157.png]]
A->C

![[Pasted image 20250517203335.png]]
A->C->D->B(from A)->E(from B)
![[Pasted image 20250517203413.png]]
![[Pasted image 20250517203524.png]]
(D, B), (B,C), (B, A), (C, E), (E, F)

2+2+3+5+5 = 17


## Floyd-Warshall
Have K, I, J, variables, each represent vertex. K as outerloop for 0 < n, i and j used interchangeably, and a 2d array $dist[i][j]$ which represents distances between i and j respectively.

Base case, dist(i, j, 0) = plain distance, set to 0 for any i = j
Other case, dist(i, j, k-1) = min(dist(i,j,0), dist(i, k, k-1), dist(k, j, k-1))
Or $\infty$ 
![[Pasted image 20250517205213.png]]

![[Pasted image 20250517210051.png]]
Can now go from v2, through v1, to v3


