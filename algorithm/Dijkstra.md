# Dijkstra's Alogirithm  
* Dijkstra's algorithm -> computing shortest paths in a **weighted** graph.  
* It can be seen as a greedy algorithm, or a dynamic programming approach.  

## Introduction  
![Introduction](https://user-images.githubusercontent.com/63276842/179685539-d3ba8dbf-7cce-4cc9-a105-706de1b163b5.png)  
* The distance d(s, v_4) is the length of **the shortest path** from s to v_4.  
* d(s, v_4) = 9.  

## Problem Statement  
### Problem (Single-source shortest path)  
* Input  
  * A directed graph G(V, E) with vertex set V = {v_1, v_2, ... v_n}, where the source node is *s* = *v_1*.  
  * Each edge (v_i, v_j) has a weight *l*(v_i, v_j) > 0 called its **length**.  
* Output  
  * The shortest path from *s* to all other nodes *v_i*. (i = 2, 3, ... , n)  
***
![S](https://user-images.githubusercontent.com/63276842/179688911-47ba58c4-c625-40f3-967c-bece66763286.png)  
* S에 속해있는 *v_i* 의 경우, d_i = d(s, v_i)를 주어진 source로부터 알 수 있다. (**the shortest path** 를 알고 있음)  
* S에 속해있지 않은 *v_j* 의 경우, S와 연결된 *v_i* 의 edge를 통해 알 수 있다.  
  * d`_j = min \(d_i) + *l*(v_i, v_j)  
  * 연결되어 있는 *v_i* 의 edge가 없을 경우, d`_n = INF.  

### Pseudocode  
```python3
 1: procedure DIJKSTRA(G(V, E), l)
 2:    S <- {v1 = S}
 3:    d_1 <- 0
 4:    d`_j <- INF for all j>=2
 5:  
 6:    for each edge (s, v_j) do
 7:        d`_j <- l(s, vj)
 8:  
 9:    while S != V do
10:        Let v_i be the node in V \ S such that d`_i is minimum
11:        d_i <- d`_i
12:        S <- S ∪ {v_i}
13:        for each edge (v_i, v_j) such that v_j ∈/ S do
14:            d`_j = min(d`_j, d_i + l(v_i, v_j))
```

### Efficient Implementation and Analysis  
* While loop는 *n* 번 반복된다. (n = |V|), 따라서 Line 10은 *n* 번 실행된다.  
* d`_j* 를 **heap-based priority queue** 에저장하여, Line 10을 O(log n) 안에 실행한다.  
* Line 14에서 **CHANGEKEY** 를 활용하면 O(log n)이 소요된다.  
* Line 14는 각 edge (v_i, v_j)에 대해 최대 한 번씩만 실행된다.  
  * v_i가 S에 최대 한 번만 삽입될 수 있기 때문이다.  
  * 따라서 Line 14는 m = |E| 번 실행된다.  

* **따라서 Dijkstra 알고리즘은 O((n+m)log n) 내에 실행되도록 구현할 수 있다.**  
  * n : number of vertices  
  * m : number of edges  
