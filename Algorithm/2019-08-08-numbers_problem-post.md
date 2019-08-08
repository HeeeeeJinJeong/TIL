# numbers problem
탈옥수가 검문을 피해 마을과 마을 사이를 도망다니고 있다.<br>
탈옥수는 탈출 당일 인접한 마을에 숨었다.<br>
매일 인접한 마을로 이동해 숨는다.<br>
d일이 지났을 때 각 마을에 숨어있을 확률을 구하시오.

```python
# 마을의 수
n = 5

# 탈출 후 지난 일 수
d = 2

# 처음 탈출한 마을은 0
start = 0

# 마을과 마을 사이에 길이 있는지를 보여주는 행렬 (무방향 그래프)
connected = [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 0],
    [1, 0, 0, 0, 0],
    [0, 1, 0, 0, 0]
]

# 인접한 마을의 수 배열
deg = [3, 2, 1, 1, 1]

# solve(v, days) -> v 마을에 있을 때 days일이 지난 후 t 마을에 탈옥수가 있을 확률
def solve(v, days):
    # base cace
    if d == days:
        return 1.0 if v == t else 0.0

    # 이미 계산된 값이 cache에 있다면 그 값을 반환
    if cache[v][days] != None:
        return cache[v][days]
    
    # 계산된 값이 cache에 없다면 계산해서 반환
    cache[v][days] = 0.0
    for i in range(n):
        if connected[v][i]:
            cache[v][days] += solve(i, days+1) / deg[v]
    return cache[v][days]

prob = []
for t in range(n):
    cache=[[None for _ in range(30)] for _ in range(10)]
    prob.append(solve(0, 0))

print(prob)
# [0.8333333333333333, 0.0, 0.0, 0.0, 0.16666666666666666]
```
