---
description: Prim's algorithm
---

# Prim's algorithm



{% tabs %}
{% tab title="Python" %}

{% endtab %}

{% tab title="Java" %}
```java

// https://sohee-dev.tistory.com/69
// https://victorydntmd.tistory.com/102

/*
5
0 5 10 8 7 
5 0 5 3 6 
10 5 0 1 3 
8 3 1 0 1 
7 6 3 1 0
*/
/*
10
*/

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class PrimMST {
	static class Node implements Comparable<Node> {
		int v;
		int w;

		Node(int v, int w) {
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

	public static void main(String[] args) throws Exception {
		System.setIn(new FileInputStream("D:\\CS\\wks\\pro\\src\\basic\\prim.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int N = Integer.parseInt(br.readLine());

		int[][] graph = new int[N][N];
		boolean[] visited = new boolean[N];
		int[] dist = new int[N];
		PriorityQueue<Node> pq = new PriorityQueue<Node>();

		StringTokenizer st;
		for (int i = 0; i < N; i++) {
			st = new StringTokenizer(br.readLine());
			for (int j = 0; j < N; j++) {
				graph[i][j] = Integer.parseInt(st.nextToken());
			}
			dist[i] = Integer.MAX_VALUE;
			visited[i] = false;
		}

		int min_dist = 0;
		int s = 0;
		dist[s] = 0;
		pq.offer(new Node(s, 0));
		while (!pq.isEmpty()) {
			Node curr = pq.poll();

			if (visited[curr.v]) {
				continue;
			}

			visited[curr.v] = true;
			min_dist += curr.w;
			for (int i = 0; i < N; i++) { // !!
				if (!visited[i] && graph[curr.v][i] != 0) {
					if (dist[i] > graph[curr.v][i]) {
						dist[i] = graph[curr.v][i];
						pq.offer(new Node(i, graph[curr.v][i]));
					}
				}
			}
		}
		System.out.println(min_dist);
	}
}
```
{% endtab %}

{% tab title="Java" %}
```java
/*
3
0 2 3
2 0 1
3 1 0
*/

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
	static int N;
	static List<Edge>[] graph;
	static List<Edge> mst;
	static boolean[] visited;

	static class Edge implements Comparable<Edge> {
		int u, v;
        long w;

		public Edge(int u, int v, long w) {
			this.u = u;
			this.v = v;
			this.w = w;
		}

		@Override
		public int compareTo(Edge o) {
			return (int)(this.w - o.w);
		}

		@Override
		public String toString() {
			return "Edge [u=" + u + ", v=" + v + ", w=" + w + "]";
		}
	}

	@SuppressWarnings("unchecked")
	public static void main(String args[]) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int V = Integer.parseInt(br.readLine());
		graph = new ArrayList[V + 1];
		mst = new ArrayList<Edge>();
		visited = new boolean[V + 1];
		
		for (int u = 1; u <= V; u++) {
			graph[u] = new ArrayList<Edge>();			
		}

		for (int u = 1; u <= V; u++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			for (int v = 1; v <= V; v++) {
				int w = Integer.parseInt(st.nextToken());
				if (u != v) {
					graph[u].add(new Edge(u, v, w));
					//graph[v].add(new Edge(v, u, w));
				}
			}
		}
		
		//System.out.println(Arrays.deepToString(graph));

		PriorityQueue<Edge> pq = new PriorityQueue<Edge>();
		Queue<Integer> queue = new LinkedList<>();
		
		queue.add(1);
        long min_cost = 0;
		while (!queue.isEmpty()) {
			int u = queue.poll();		
			visited[u] = true;
			for (Edge v : graph[u]) {
				if (!visited[v.v]) {
					pq.add(v);
				}
			}
			
			while(!pq.isEmpty()) {
				Edge e = pq.poll();
				if(!visited[e.v]) {
					queue.add(e.v);
					visited[e.v] = true;
					//mst.add(e);
					min_cost += e.w;
					break;
				}
			}
		}
		
		//System.out.println(mst.toString());
		System.out.println(min_cost);
	}
}
```
{% endtab %}
{% endtabs %}





#### 참고자료

https://www.weeklyps.com/entry/프림-알고리즘-Prims-algorithm#d2

[https://www.acmicpc.net/problem/1700](https://www.acmicpc.net/problem/1700)

[https://sohee-dev.tistory.com/69](https://sohee-dev.tistory.com/69)



