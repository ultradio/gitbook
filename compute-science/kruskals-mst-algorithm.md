# Kruskal's MST Algorithm

신장트리(Spanning Tree)는 모든 vertex가 edge로 연결하면서 edge 사이에 사이클이 없는 그래프이다. vertex수는 edge수 + 1이 된다.\
\
모든 vertex가 다 연결은 되어 있지만, 일부 edge를 사용하지 않아도 되는 점을 활용해 여러 문제를 해결할 수 있다. 예를 들어서 **최소 비용으로 연결된 신장 트리를 찾을 때 사용**할 수 있는데, 이것을 최소 신장 트리(Minimum Spanning Tree)라 한다.

### 크루스칼 알고리즘

대표적은 탐욕적 방법으로 최소 신장 트리 알고리즘이다.

동작 과정은\
먼저 부모테이블을 만들어 자기 자신으로 부모로 초기화하고,\
간선을 비용 오름차순으로 정렬하고 (sorted), \
모든 정점을 방문할 때 까지 (for i in range(V)),\
간선을 순서데로 확인하면서 사이클이 발생하는지 확인하고 (exist\_cycle),\
사이클이 발생하지 않을 경우만 부모테이블을 업데이트하여 트리에 포함시키고 (union)\
이 과정을 반복 수행한다.

![](<../.gitbook/assets/image (24).png>)

간선을 비용 오름차순으로 정렬한다.

| edge | 2-3 | 0-3 | 0-2 | 0-1 | 1-3 |
| ---- | --- | --- | --- | --- | --- |
| cost | 4   | 5   | 6   | 10  | 15  |

모든 정점을 방문할 때 까지 (for i in range(V)),\
간선을 순서데로 확인하면서 사이클이 발생하는지 확인하고 (exist\_cycle),\
사이클이 발생하지 않을 경우만 트리에 포함시키고 (union)\
이 과정을 반복 수행한다.

**step 0**

```python
n: [0,1,2,3]
p: [0,1,2,3]
```

| node  | 0 | 1 | 2 | 3 |
| ----- | - | - | - | - |
| p\[n] | 0 | 1 | 2 | 3 |

**step 1**

```python
union(2,3)
  find(2) -> 2
  find(3) -> 3 -> p[3] = 2
  p: [0,1,2,2]
```

| node  | 0 | 1 | 2 | 3     |
| ----- | - | - | - | ----- |
| p\[n] | 0 | 1 | 2 | **2** |

**step 2**

```python
union(0,3)
  find(0) -> 0
  find(3) -> find(2) -> 2 -> p[2] = 0
  p: [0,1,0,2]
```

| node  | 0 | 1 | 2     | 3 |
| ----- | - | - | ----- | - |
| p\[n] | 0 | 1 | **0** | 2 |

**step 3**

```python
union(0,2)
  find(0) -> 0
  find(2) -> 0 -> pass
```

| node  | 0 | 1 | 2 | 3 |
| ----- | - | - | - | - |
| p\[n] | 0 | 1 | 0 | 2 |

**step 4**

```python
union(0,1)
  find(0) -> 0
  find(1) -> 1 -> p[1] = 0
  p: [0,0,0,2]
```

| node  | 0 | 1     | 2 | 3 |
| ----- | - | ----- | - | - |
| p\[n] | 0 | **0** | 0 | 2 |

### 코드

{% tabs %}
{% tab title="Python" %}
```python
def kruskalMST(n, some_list):
    parent = []
    for i in range(n):
        parent.append(i)
    
    def find(parent, i):
        if parent[i] == i:
            return i
        else:
            parent[i] = find(parent, parent[i])
        return parent[i]
    
    def exsit_cycle(parent, n1, n2):
        n1 = find(parent, n1)
        n2 = find(parent, n2)
        
        if n1 == n2:
            return True
        else:
            return False
    
    def union(parent, n1, n2):
        n1 = find(parent, n1)
        n2 = find(parent, n2)
        if n1 <= n2:
            parent[n2] = n1
        else:
            parent[n1] = n2
        return parent
    
    some_list = sorted(some_list, key=lambda x: x[2])
    min_cost = 0
    for [n1, n2, w] in some_list:
        if not exsit_cycle(parent, n1, n2):
            min_cost += w
            parent = union(parent, n1, n2)
            print("{} -- {} == {}".format(n1, n2, w))
            print("parent: {}".format(parent))
            
    return min_cost

n = 4
some_list = [[0,1,10], [0,2,6], [0,3,5], [1,3,15], [2,3,4]]
min_cost = kruskalMST(n, some_list)
print("Minimum Spanning Tree: {}".format(min_cost))
```
{% endtab %}

{% tab title="Java" %}
```java
// https://velog.io/@ming/MST최소-신장트리-알고리즘
// https://github.com/chaminhye/Algorithm/tree/master/src/concept/graph

/*
10
1 2 5
2 3 1
3 6 9
1 4 3
1 3 8
1 5 10
4 5 4
5 6 12
4 6 2
3 4 4
*/


import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.StringTokenizer;

// https://velog.io/@ming/MST최소-신장트리-알고리즘
// https://github.com/chaminhye/Algorithm/tree/master/src/concept/graph

public class Kruskal {

	static class Edge implements Comparable<Edge> {
		int v1;
		int v2;
		int w;

		Edge(int v1, int v2, int w) {
			this.v1 = v1;
			this.v2 = v2;
			this.w = w;
		}

		@Override
		public int compareTo(Edge edge) {
			return edge.w >= this.w ? -1 : 1;
		}
	}

	public static ArrayList<Edge> graph = new ArrayList<Edge>();
	public static int[] parent;

	public static int find(int n) {
		if (parent[n] == n) {
			return n;
		} else {
			return parent[n] = find(parent[n]);
		}
	}

	public static boolean exist_cycle(int n1, int n2) {
		n1 = find(n1);
		n2 = find(n2);

		if (n1 == n2) {
			return true;
		} else {
			return false;
		}
	}

	public static void union(int n1, int n2) {
		n1 = find(n1);
		n2 = find(n2);

		if (n2 > n1) {
			parent[n2] = n1;
		} else {
			parent[n1] = n2;
		}
	}

	public static void main(String args[]) throws IOException {
		System.setIn(new FileInputStream("D:\\CS\\wks\\pro\\src\\basic\\kruskal.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int N = Integer.parseInt(br.readLine());

		for (int i = 0; i < N; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int v1 = Integer.parseInt(st.nextToken());
			int v2 = Integer.parseInt(st.nextToken());
			int w = Integer.parseInt(st.nextToken());
			graph.add(new Edge(v1, v2, w));
		}

		parent = new int[N];
		for (int i = 0; i < parent.length; i++) {
			parent[i] = i;
		}

		Collections.sort(graph);

		int min_cost = 0;
		for (int i = 0; i < graph.size(); i++) {
			Edge e = graph.get(i);
			if (!exist_cycle(e.v1, e.v2)) {
				min_cost += e.w;
				union(e.v1, e.v2);
				System.out.println(String.format("%s -- %s = %s", e.v1, e.v2, e.w));
				System.out.println(Arrays.toString(parent));
			}
		}

		System.out.println(min_cost);
	}
}
```
{% endtab %}
{% endtabs %}

### 참고자료

[https://velog.io/@ming/MST최소-신장트리-알고리즘](https://velog.io/@ming/MST%EC%B5%9C%EC%86%8C-%EC%8B%A0%EC%9E%A5%ED%8A%B8%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)\
[https://www.geeksforgeeks.org/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/](https://www.geeksforgeeks.org/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)\
[https://github.com/chaminhye/Algorithm/blob/master/src/concept/graph/Kruskal.java](https://github.com/chaminhye/Algorithm/blob/master/src/concept/graph/Kruskal.java) \
[https://deep-learning-study.tistory.com/592](https://deep-learning-study.tistory.com/592)
