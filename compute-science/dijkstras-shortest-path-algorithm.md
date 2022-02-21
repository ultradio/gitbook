---
description: Dijkstra’s shortest path algorithm
---

# Dijkstra’s shortest path algorithm

**출발 노드에서 목적지 노드로 가는 최단 경로를 구하는 알고리즘**으로 BFS 알고리즘과 유사하다. 다익스트라 알고리즘은 음의 간선이 없을 때 정상 동작한다.

동작 과정은 먼저 최단 거리 테이블(distances)을 초기화하고 출발노드를 설정(queue)한다. 미방문 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다. 시작 노드에서 해당 노드를 거쳐 다른 노드로 가는 비용을 구하고, 최단 거리 테이블을 갱신하는 과정을 반복한다.

![](<../.gitbook/assets/image (25).png>)

최단 거리 테이블을 초기화하고 출발노드를 설정한다.

```python
A B C D E F  A
0 ∞ ∞ ∞ ∞ ∞  0            
```

&#x20;미방문 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.

```python
A->A=0
A B C D E F  C D B
0 8 1 2 ∞ ∞  1 2 8   --> A->B=∞ > A->B=8, A->C=∞ > A->C=1, A->D=∞ > A->D=2
```

시작 노드에서 해당 노드를 거쳐 다른 노드로 가는 비용을 구하고, 최단 거리 테이블을 갱신하는 과정을 반복한다.

```python
A->C=1
A B C D E F  D B B
0 6 1 2 ∞ ∞  2 6 8   --> A->B=8 > A->C->B=1+5=6, A->D=2 < A->C->D=1+2=3
C->D=2
A B C D E F  E B F B
0 6 1 2 5 7  5 6 7 8 --> A->E=∞ > A->D->E=2+3=5, A->F=∞ > A->D->F=2+5=7
D->E=5
A B C D E F  B F F B
0 6 1 2 5 6  6 6 7 8 --> A->F=6 < A->E->F=5+6=11
E->B=6
A B C D E F  F F B
0 6 1 2 5 6  6 7 8   --> A노트로 가는 edge가 없음
B->F=6
A B C D E F  F B
0 6 1 2 5 6  7 8     --> 스킵
F->F=7
A B C D E F  B
0 6 1 2 5 6  8       --> 스킵
F->B=7
A B C D E F   
0 6 1 2 5 6          --> 스킵
```

### &#x20;코드&#x20;

{% tabs %}
{% tab title="Python" %}
```python
"""
출발노드에서 목적지 노드로 가는 최단 경로를 구한다.
"""
import heapq

def dijkstra(graph, start_node, end_node):
    dists = {} # {'A': [0, 'A'], 'B': [inf, 'A']}
    for node in graph:
        dists[node] = [float('inf'), start_node]
        
    queue = []
    dists[start_node] = [0, start_node]
    heapq.heappush(queue, [dists[start_node][0], start_node])

    while len(queue) > 0:
        curr_dist, curr_node  = heapq.heappop(queue)
        if dists[curr_node][0] < curr_dist:
            continue
        
        for adj_node, w in graph[curr_node].items():
            dist = curr_dist + w
            if dist < dists[adj_node][0]:
                dists[adj_node] = [dist, curr_node]
                heapq.heappush(queue, [dist, adj_node])
    
    return dists

graph = {    
         'A': {'B': 8, 'C': 1, 'D': 2},
         'B': {},
         'C': {'B': 5, 'D': 2},
         'D': {'E': 3, 'F': 5},
         'E': {'F': 1},
         'F': {'A': 5}
         }

dists = dijkstra(graph, 'A', 'F')
print(dists)

def shortest_path(dists, start_node, end_node):
    path = end_node
    path_output = end_node

    while path != start_node:
        path = dists[path][1]
        path_output += '->' + path
    
    return path_output  
    
path_output = shortest_path(dists, 'A', 'F')
print(path_output)
```
{% endtab %}

{% tab title="Java" %}
```java
//https://wikidocs.net/103987

/*
7 9
1 2 3
2 6 2
1 3 2
3 5 6
2 5 3
4 5 1
2 4 1
5 7 2
6 7 4
1 7
*/
/*
7->5->4->2->1
*/

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Dijkstra {

	static class Node implements Comparable<Node> {
		int v;
		int w;

		public Node(int v, int w) {
			this.v = v;
			this.w = w;
		}

		@Override
		public int compareTo(Node o) {
			return this.w - o.w;
		}

		@Override
		public String toString() {
			return "Node [v=" + v + ", w=" + w + "]";
		}
	}

	static class Distance {
		int d;
		int v;

		Distance(int d, int v) {
			this.d = d;
			this.v = v;
		}

		@Override
		public String toString() {
			return "Distance [d=" + d + ", v=" + v + "]";
		}
	}

	public static void main(String[] args) throws IOException {
		System.setIn(new FileInputStream("D:\\CS\\wks\\pro\\src\\basic\\dijkstra.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int N = Integer.parseInt(st.nextToken());
		int M = Integer.parseInt(st.nextToken());

		List<Node>[] graph = new ArrayList[N + 1];
		List<Distance> dist = new ArrayList<Distance>();

		for (int i = 1; i <= N; i++) {
			graph[i] = new ArrayList<Node>();
		}
		for (int i = 0; i <= N; i++) {
			if (i == 0) {
				dist.add(null);
			} else {
				dist.add(new Distance(Integer.MAX_VALUE, i));
			}
		}

		for (int i = 0; i < M; i++) {
			st = new StringTokenizer(br.readLine());
			int v1 = Integer.parseInt(st.nextToken());
			int v2 = Integer.parseInt(st.nextToken());

			int w = Integer.parseInt(st.nextToken());
			graph[v1].add(new Node(v2, w));
			graph[v2].add(new Node(v1, w));
		}

		st = new StringTokenizer(br.readLine());
		int start = Integer.parseInt(st.nextToken());
		int dest = Integer.parseInt(st.nextToken());
		dist.set(start, new Distance(0, start));

		PriorityQueue<Node> pq = new PriorityQueue<Node>();
		pq.offer(new Node(start, 0));

		System.out.println(Arrays.toString(graph));
		System.out.println(dist.toString());

		while (!pq.isEmpty()) {
			Node no = pq.poll();
			for (int i = 0; i < graph[no.v].size(); i++) {
				Node tmp = graph[no.v].get(i);
				if (dist.get(tmp.v).d > no.w + tmp.w) {
					dist.get(tmp.v).d = no.w + tmp.w;
					dist.get(tmp.v).v = no.v;
					pq.add(new Node(tmp.v, dist.get(tmp.v).d));
				}
			}
		}

		int path = dest;
		String path_output = String.valueOf(dest);
		while (path != start) {
			path = dist.get(path).v;
			path_output += "->" + path;
		}
		System.out.println(path_output);
	}
}
```
{% endtab %}
{% endtabs %}

### 참고자료

[https://www.fun-coding.org/Chapter20-shortest-live.html](https://www.fun-coding.org/Chapter20-shortest-live.html)\
[https://brownbears.tistory.com/554https://hsp1116.tistory.com/42](https://brownbears.tistory.com/554https://hsp1116.tistory.com/42)\
[https://thecleverprogrammer.com/2021/04/18/dijkstras-algorithm-using-python/](https://thecleverprogrammer.com/2021/04/18/dijkstras-algorithm-using-python/)
