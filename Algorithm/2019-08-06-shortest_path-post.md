# shortest path

### 최단 경로(shortest path)
1. 음수 사이클이 없다.
2. 방향 그래프(directed graph)
3. 가중치 그래프(weighted graph)
4. 경로의 길이 : 에지 가중치의 합

### 최단 경로 알고리즘의 종류
1. 하나의 출발점과 나머지 모든 목적지
- Dijkstra 알고리즘(음수 가중치가 없다)
- Bellman-Ford 알고리즘(일반적인 경우)
2. 모든 (출발점,목적지) 쌍
- Floyd-Warshall 알고리즘

### Dijkstra 알고리즘
1. 탐욕 알고리즘
2. 음수 가중치가 없다.
3. 최단 경로가 발견된 정점의 집합 S
4. 정점 v(v∈ 𝑉 − 𝑆)의 distance[v] : 출발 정점에서 S에 있는 정점만 거쳐 v에 도달하는 경로의 길이

### Relaxation of an edge
출발 정점에서 정점 u까지 정점 v를 거치지 않고 오는 경로의 길이 distance[1]보다<br>
출발 정점에서 정점 v까지 먼저 온 후 정점 u로 가는 경로의 길이 distance[0] + w(v, u)가 더 작으면<br>
distance[1]를 업데이트한다
```python
if distance[u] > distance[v]+w[v, u]:
    distance[u] = distance[v]+w[v, u]
```

### Graph representation
정점 I, j가 인접하면 에지의 가중치, 인접하지 않으면 None
```python
'''
   0   1   2   3
0  N  10   3   N
1  N   N   N   5
2  N   5   N   8
3  N   4  12   N
'''
```

### dijkstra.py
```python

```
