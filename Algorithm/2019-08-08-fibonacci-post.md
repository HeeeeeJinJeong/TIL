# fibonacci
0과 1부터 시작하여 다음 수가 앞의 두 수를 더한 값이 되는 수열(0 1 1 2 3 5 8 13 21 34...)

- 점화식 : fibonacci(n)=fibonacci(n-2)+fibonacci(n-1)
- 기저 조건 : if n==1, return 0 and if n==2 return 1

```python
def fibonacci(n):
    if n == 1:
        return 0
    elif n == 2:
        return 1
        
    return fibonacci(n-2) + fibonacci(n-1)
    

# test code
if __name__=='__main__':
    for n in range(1, 11):
        print(fibonacci(n), end=' ')
```
### Memoization
함수가 입력이 같다면 출력이 같을때만 사용할 수 있다.
```python
cache = [None for _ in range(300)]
cache[1] = 0
cache[2] = 1

def fibonacci(n):
    if cache[n] != None:
        return cache[n]
    return fibonacci(n-2) + fibonacci(n-1)
    
if __name__=='__main__':
    for n in range(1, 11):
        print(fibonacci(n), end=' ')
```
