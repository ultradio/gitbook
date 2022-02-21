---
description: Graph Search
---

# Graph Search

### 그래프 탐색 알고리즘

그래프 탐색 알고리즘에는 깊이우선탐색(DFS)과 너비우선탐색(BFS)이 있다.\
[![](https://lh3.googleusercontent.com/-9f9GjKCUr7k/YRW4yw0H0TI/AAAAAAAAZrw/Nj492B0JTw0UhCc\_XvtkWAocMac3DE7xwCLcBGAsYHQ/w400-h221/image.png)](https://lh3.googleusercontent.com/-9f9GjKCUr7k/YRW4yw0H0TI/AAAAAAAAZrw/Nj492B0JTw0UhCc\_XvtkWAocMac3DE7xwCLcBGAsYHQ/image.png)\


#### DFS(Depth First Search, 깊이우선탐색)

깊이 우선 탐색으로 Stack을 이용한다. 루트 노트에서 출발해 하나의 경로를 끝까지 탐색한 후 다른 경로를 탐색한다. 더 이상 탐색할 노드가 없는지 방문한 노드를 검사하면서 탐색한다.

```python
print("Graph - DFS")
graph = { 
         'A': set(['B', 'C']),
         'B': set(['A', 'D', 'E']),
         'C': set(['A']),
         'D': set(['B', 'F']),
         'E': set(['B']),
         'F': set(['D'])
         }
root = 'A'

def DFS(graph, root):
    visited = []
    stack = [root]
    
    while len(stack) > 0:
        node = stack.pop()
        if node not in visited:
            visited.append(node)
            stack += graph.get(node) - set(visited)
    return visited
    
print(DFS(graph, root))

```

#### BFS(Breadth First Search, 너비우선탐색)

너비우선탐색은 Queue를 이용하여 루트 노드와 인접한 노드를 우선 방문한다.

```python
print("Graph - BFS")
graph = { 
         'A': set(['B', 'C']),
         'B': set(['A', 'D', 'E']),
         'C': set(['A']),
         'D': set(['B', 'F']),
         'E': set(['B']),
         'F': set(['D'])
         }
root = 'A'

def BFS(graph, root):
    from collections import deque
    visited = []
    queue = deque([root])
    
    while len(queue) > 0:
        node = queue.popleft()
        if node not in visited:
            visited.append(node)
            queue += graph.get(node) - set(visited)
    
    return visited

print(BFS(graph, root))
```
