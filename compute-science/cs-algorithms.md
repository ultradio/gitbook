# Algorithms

### 분할 정복법(Divide & Conquer)

풀려는 문제가 큰 문제일 경우 그 문제를 작은 문제들로 나누고(Divide), 그 문제를 푼 뒤(Conquer) 다시 합쳐나가는 방식(Combine)이다.\
\
대표적인 알고리즘으로 **이진탐색과 합병 정렬** 있다.

```python
print("# n번째 피보나치 수를 리턴")
# 0 1 1 2 3 5 8 13 21 ...
def _fib(n):   
    if n ==1 or n == 2: # base case
        return 1
    else: # recursive case
        return _fib(n-1) + _fib(n-2)

# 테스트: fib(1)부터 fib(10)까지 출력
for i in range(1, 11):
    print(_fib(i))

```

![](https://lh3.googleusercontent.com/-5FM2gP\_2KA8/YRMiek2D16I/AAAAAAAAZo8/mL990xkpqZsErAlz\_43Uqg-yxixmSkSEgCLcBGAsYHQ/w506-h210/1628643960413769-0.png)

### 동적 계획법

분할 정복법과 비슷하고 차이점은 부분으로 나눈 문제가 서로 중첩될 경우 이전에 계산해둔 것을 재활용함으로써 더 복합한 문제를 빠르게 계산할 수 있다.

#### Memoization

분활 정복법과 비슷하며, 차이점은 부분 문제를 메모하면서 재귀적으로 푼다.  (Top-down Approach)

#### Tabulation

테이블 방식으로 정리하면서 문제를 푼다. 중복 문제를 먼저 푼다. (Bottom-up Approach)\
\
분할 정복법과 비슷하며, 차이점은 테이블 방식으로 정리하면서 문제를 반복문으로 푼다. Memoization과 차이점은 재귀 대신 반복문을 사용하는 것이다.\
대표적인 알고리즘으로 **다익스트라의 최단거리**가 있다.

```python
print("# 피보나치 수열 - Memoization")
# 재귀방식으로 푼다.
# 부분 문제를 메모하면서 푼다 (Top-down Approach)
def fib_memo(n, cache):
    if n == 1 or n == 2:
        cache[n] = 1
        return 1
    else:
        if not cache.get(n, None):
            cache[n] = fib_memo(n-2, cache) + fib_memo(n-1, cache)
        return cache.get(n)

cache = {} # !!
for i in range(1, 11):
    print(fib_memo(i, cache))
```

```python
print("# 피보나치 수열 - Tabulation")
# 테이블 방식으로 정리하면서 문제를 푼다.
# 중복 문제를 먼저 푼다. (Bottom-up Approach)
def fib_tab(n):
    cache = {1:1, 2:1} # !!
    for i in range(1, n):
        if not cache.get(i, None):
            cache[i] = cache.get(i-2) + cache.get(i-1)
    return cache.get(n-2) + cache.get(n-1)

print(fib_tab(10))
```

### 탐욕적 방법 (Greedy Algorithm)

그때 그때 최적의 선택을 하는 방식이다. 간단하고 빠르지만 최적의 답이 보장되지는 않는다.\
\
대표적인 알고리즘으로 그래프의 모든 노드를 최 비용으로 연결하는 최소 신장 트리를 만드는 **크루스칼 알고리즘**(Kruskal algorithm)과 **프림 알고리즘**(Prim algorithm)이 있고 두 노드 사이의 최단거리를 찾는 다익스트라 알고리즘이 있다.\
크루스칼은 최초 모든 노드가 각각 하나의 트리인 상태에서 가중치가 가장 작은 간선을 고른 후 해당하는 두 노드를 연결했을 때 사이클이 발생하지 않으면 두 노드를 합쳐나가면서 최소신장트리를 만드는 방식이다. 프림은 아무 노드나 시작노드로 설정하고  현재 노드에서 연결할 수 있는 간선 중 가장 가중치가 작은 간선을 차례로 붙여나가는 방식이다.\
다익스트라는 시작 노드를 기준으로 연결된 노드를 추가해가면서 최단 거리를 찾는 알고리즘으로 가중치가 0이상인 그래프에서만 활용 가능하다.\


### 완전탐색

모든 경우의 수를 다 시도해보는 방법이다. 모든 경우의 수를 다 시도하기 때문에 느릴 수 있으며 가지치기로 계산 성능을 올릴 수 있다.\
최소비용을 찾는 문제로 예를 들어보면, 지금까지 계산된 최소비용이 100이라면 100보다 큰 비용은 탐색하지 하지 않아 계산을 줄이는 식이다.\
대표적인 알고리즘으로 **DFS, BFS** 가 있다.

```python
print("# 피보나치 수열 - 반복문")
def fib_iter(n):
    series = [0,1,1]
    for i in range(3, n):
        next_element = series[i-2] + series[i-1]
        series.append(next_element)
    return series[n-2] + series[n-1]

print(fib_iter(10))
```

### 알고리즘 성능 빅오(O) 표기법

알고리즘 성능은 \
시간복잡도(Time Complexity)인 알고리즘 수행시간\
공간복잡도(Space Complexity)인 알고리즘 메모리 사용량 있다.\
\
빅오의 순서\
O(1) < O(logn) < O(n) < O(nlogn) < O(n2)

