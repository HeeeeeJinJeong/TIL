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
class ShortestPath:
    def __init__(self, src, dist, p):
        self.source = src
        self.distance = dist
        self.p = p # pi or predecessor

class Graph:
    # 이론에서는 무한대지만 구현할떄는 모든 가중치보다 충분히 큰 수를 사용
    INF = 99999

    def __init__(self, vnum):
        self.adjacency_matrix=[[None for _ in range(vnum)] for _ in range(vnum)]
        self.vertex_num=vnum

    def insert_edge(self, u, v, w):
        self.adjacency_matrix[u][v] = w

    def find_min(self, distance, S):
        _min=self.INF
        min_v = None

        for v in range(self.vertex_num):
            if v not in S and distance[v] < _min:
                _min = distance[v]
                min_v = v
        return min_v

    def dijkstra(self, src):
        S = set()
        distance = [Graph.INF for _ in range(self.vertex_num)]
        p = [None for _ in range(self.vertex_num)]
        distance[src] = 0

        while len(S) < self.vertex_num:
            v = self.find_min(distance, S)
            S.add(v)

            # adj[v]
            for u in range(self.vertex_num):
                w = self.adjacency_matrix[v][u]

                # w != None --> adj[v]안에 u가 포함된다.
                # u not in S --> u는 아직 S안에 미포함
                # relaxation
                # if distance[u] > distance[v]+w
                # then distance[u] = distance[v]+w
                if w != None and u not in S and distance[u] > distance[v] + w:
                    distance[u] = distance[v] + w
                    p[u] = v

        return ShortestPath(src, distance, p)

if __name__=="__main__":
    g=Graph(4)
    g.insert_edge(0, 1, 10)
    g.insert_edge(0, 2, 3)
    g.insert_edge(1, 3, 5)
    g.insert_edge(2, 1, 5)
    g.insert_edge(2, 3, 8)
    g.insert_edge(3, 1, 4)
    g.insert_edge(3, 2, 12)

    source=0
    sp=g.dijkstra(source)
    for i in range(g.vertex_num):
        print(f"distance[{i}] : {sp.distance[i]}, p[{i}] : {sp.p[i]}")
    '''
    distance[0] : 0, p[0] : None
    distance[1] : 8, p[1] : 2
    distance[2] : 3, p[2] : 0
    distance[3] : 11, p[3] : 2
    '''
```
