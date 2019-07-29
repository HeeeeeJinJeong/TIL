# Graph
- 객체와 객체 사이의 관계를 표현하기 위해서 사용하는 자료구조
- 그래프 G는 두 집합 V, E로 나타낸다.
1. V(G) : 노드(node) 혹은 정점(vertex)의 집합 <br>
2. E(G) : 정점을 잇는 에지(edge)의 집합

## 특징
1. 자기 간선(self-edge)를 가질 수 없다. <br>
(v, v) or <v, v>를 가질 수 없다.
2. 두 정점 사이에 같은 에지를 여러 개 가질 수 없다.

## Adjacent와 incident
- (u, v) ∈ E(G)이면 정점 u와 정점 v는 인접한다(adjacent)
- 에지 (u, v)는 u와 v에 부속된다(incident)

## 경로(path)와 길이(length)
1. 경로(path)
: (v1, v2), (v2, v3), (v3, v4)가 집합 E(G)의 원소일 때
v1에서 v4 까지의 정점 순서(v1→v2→v3→v4)
2. 경로 길이(length)
: 어떤 경로에서 에지의 수
3. 단순 경로(simple path)
: 어떤 경로에서 처음과 마지막을 제외하고
모든 정점이 다를 때

## 사이클(cycle)과 connected
1. 사이클(cycle) : 단순 경로에서 처음과 마지막 정점이 같을 때<br>
v1→v2→v3→v1
2. Connected : 정점 u에서 정점 v까지의 경로가 있다면 두 정점은 연결되었다(connected)
3. 연결된 그래프(Connected graph) : 어떠한 임의의 노드 u와 v에 대해서도 연결되어 있다면 그래프 G가 연결되었다

## 무방향 그래프(undirected graph)
G=(V, E) <br>
V(G)={0, 1, 2, 3} <br>
E(G)={(0, 1), (1, 2), (2, 3), (1, 3)} <br><br>
정점의 수 : n <br>
최대 에지 수 : n(n-1)/2 <br>

## 방향 그래프(directed graph)
G=<V, E> <br>
V(G)={0, 1, 2} <br>
E(G)={<0, 1>, <1, 2>} <br>
정점의 수 : n <br><br>
최대 에지 수 : n(n-1) <br>

## Graph traversal(그래프 순회)
그래프 G에서 한 정점 v가 주어졌을 때,<br>
모든 정점을 중복되지 않게 방문하는 것
1. 너비 우선 탐색(BFS, breadth first search)
2. 깊이 우선 탐색(DFS, depth first search)

## 너비 우선 탐색(BFS)
정점 v에서 시작하여 인접한 정점을 모두 방문 후 방문한 인접 정점에 대해서 모든 인접한 정점을 방문하는 방식

## 깊이 우선 탐색(DFS)
정점 v에서 시작하여 방문하지 않은 정점 중 한 방향으로 쭉 따라간 후,<br>
방문할 정점이 없으면 다시 돌아와 방문하지 않은 다른 정점을 따라 다시 쭉 따라간다.<br>
처음 시작한 정점에 돌아오고 더 이상 방문할 정점이 남아있지 않다면 종료

```python
from queue import Queue

class Stack:
    def __init__(self):
        self.container = list()

    def empty(self):
        if not self.container:
            return True
        return False

    def push(self, data):
        self.container.append(data)
    
    def pop(self):
        return self.container.pop()

    def peek(self):
        return self.container[-1]

class GNode:
    def __init__(self, v):
        self.vertex = v
        self.link = None

class Graph:
    def __init__(self, vnum):
        self.adjacency_list = [None for _ in range(vnum)]
        self.visited = [False for _ in range(len(self.adjacency_list))]

    def __add_node(self, v, uNode):
        cur = self.adjacency_list[v]

        if not cur:
            self.adjacency_list[v] = uNode
            return
        else:
            while cur.link:
                cur = cur.link
            cur.link = uNode

    def insert_edge(self, u, v):
        uNode = GNode(u)
        vNode = GNode(v)
        self.__add_node(v, uNode)
        self.__add_node(u, vNode)

    def bfs(self, s):
        q = Queue()
        visited = [False for _ in range(len(self.adjacency_list))]

        q.put(s)
        visited[s] = True
        # visit
        print(s, end=' ')

        while not q.empty():
            v = q.get()

            uNode = self.adjacency_list[v]
            while uNode:
                if not visited[uNode.vertex]:
                    q.put(uNode.vertex)
                    visited[uNode.vertex] = True
                    # visit
                    print(uNode.vertex, end=' ')
                uNode = uNode.link

    def __dfs_recursion(self, v):
        # base case
        if self.visited[v]:
            return

        self.visited[v] = True
        # visit
        print(v, end=' ')

        uNode = self.adjacency_list[v]
        while uNode:
            self.__dfs_recursion(uNode.vertex)
            uNode = uNode.link

    def dfs(self, s):
        # 방문 리스트 초기화
        for i in range(len(self.adjacency_list)):
            self.visited[i] = False

        self.__dfs_recursion(s)

    def iter_dfs(self, v):
        s = Stack()
        for i in range(len(self.visited)):
            self.visited[i] = False

        s.push(v)
        self.visited[v] = True
        print(v, end=' ')

        while not s.empty():
            is_visited = False
            v = s.peek()
            u = self.adjacency_list[v]
            while u:
                if not self.visited[u.vertex]:
                    self.visited[u.vertex] = True
                    print(u.vertex, end=' ')

                    s.push(u.vertex)
                    is_visited = True
                    break
                u = u.link
            if not is_visited:
                s.pop()

if __name__=="__main__":
    g = Graph(6)
    g.insert_edge(0, 3)
    g.insert_edge(0, 1)
    g.insert_edge(2, 4)
    g.insert_edge(2, 5)
    g.insert_edge(3, 4)

    # g.bfs(3) # 3  0  4  1  2  5
    # print()

    g.dfs(3) # 3 0 1 4 2 5
    print()

    g.iter_dfs(3) # 3 0 1 4 2 5
    print()
```
