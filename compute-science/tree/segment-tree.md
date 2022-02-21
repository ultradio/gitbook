---
description: Segment Tree
---

# Segment Tree



{% tabs %}
{% tab title="Python" %}
```java
```
{% endtab %}

{% tab title="Java" %}
```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

enum Type {
	SUM, PRODUCT, MIN, MAX, MIN_MAX
}

public class Main {
	static int N, M, K;
	static long[] input;
	static final int MOD = 1000000007;
	static Type type;

	public static long init(long[] tree, int v, int s, int e) {
		if (s == e) {
			return tree[v] = input[s];
		}

		int m = (s + e) / 2;
		if (type == Type.PRODUCT) {
			return tree[v] = init(tree, 2 * v, s, m) * init(tree, 2 * v + 1, m + 1, e) % MOD;
		} else if (type == Type.MIN) {
			return tree[v] = Math.min(init(tree, 2 * v, s, m), init(tree, 2 * v + 1, m + 1, e));
		} else if (type == Type.MAX) {
			return tree[v] = Math.max(init(tree, 2 * v, s, m), init(tree, 2 * v + 1, m + 1, e));
		} else {
			return tree[v] = init(tree, 2 * v, s, m) + init(tree, 2 * v + 1, m + 1, e);
		}
	}

	public static long query(long[] tree, int v, int s, int e, int l, int r) {
		if (l > e || r < s) {
			if (type == Type.PRODUCT) {
				return 1;
			} else if (type == Type.MIN) {
				return Integer.MAX_VALUE;
			} else if (type == Type.MAX) {
				return Integer.MIN_VALUE;
			} else {
				return 0;
			}
		}

		if (l <= s && r >= e) {
			return tree[v];
		}

		int m = (s + e) / 2;
		if (type == Type.PRODUCT) {
			return query(tree, 2 * v, s, m, l, r) * query(tree, 2 * v + 1, m + 1, e, l, r) % MOD;
		} else if (type == Type.MIN) {
			return Math.min(query(tree, 2 * v, s, m, l, r), query(tree, 2 * v + 1, m + 1, e, l, r));
		} else if (type == Type.MAX) {
			return Math.max(query(tree, 2 * v, s, m, l, r), query(tree, 2 * v + 1, m + 1, e, l, r));
		} else {
			return query(tree, 2 * v, s, m, l, r) + query(tree, 2 * v + 1, m + 1, e, l, r);
		}
	}

	public static long update(long[] tree, int v, int s, int e, int i, long d) {
		if (i > e || i < s) {
			if (type == Type.PRODUCT) {
				return tree[v];
			} else {
				return -1;
			}
		}

		if (type == Type.SUM) {
			tree[v] += d;
		}

		if (s == e) {
			if (type == Type.PRODUCT) {
				return tree[v] = d;
			} else {
				input[i] = tree[v];
				return -1;
			}
		}
		int m = (s + e) / 2;
		if (type == Type.PRODUCT) {
			return tree[v] = update(tree, 2 * v, s, m, i, d) * update(tree, 2 * v + 1, m + 1, e, i, d) % MOD;
		} else {
			update(tree, 2 * v, s, m, i, d);
			update(tree, 2 * v + 1, m + 1, e, i, d);
			return -1;
		}
	}

	public static void main(String[] args) throws Exception {
		System.setIn(new FileInputStream("D:\\dev\\java\\wks\\cs\\src\\segment\\segment_min_max_2357.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		N = Integer.parseInt(st.nextToken()); // 수의 개수
		M = Integer.parseInt(st.nextToken()); // 변경 횟수 a, b, c (if a==1, b를 c로 변경)

		type = Type.MIN_MAX;
		
		if (type == Type.PRODUCT || type == Type.SUM) {
			K = Integer.parseInt(st.nextToken()); // 구간 곱 a, b, c (if a==1, b~c 까지 곱)
		} else {
			K = 0;
		}

		input = new long[N + 1]; // ! [N]

		for (int i = 1; i <= N; i++) { // ! i = 0
			input[i] = Integer.parseInt(br.readLine());
		}

		int v = 1;
		int s = 1;// ! s = 0
		int e = input.length - 1;
		long[] tree = new long[N * 4];
		init(tree, v, s, e);
		// System.out.println(Arrays.toString(input));
		// System.out.println(Arrays.toString(tree));

		StringBuilder sb = new StringBuilder();
		if (type == Type.PRODUCT || type == Type.SUM) {
			for (int i = 0; i < M + K; i++) {
				st = new StringTokenizer(br.readLine());
				int a = Integer.parseInt(st.nextToken());
				int b = Integer.parseInt(st.nextToken());
				long c = Long.parseLong(st.nextToken());

				if (a == 1) {
					// update(v, s, e, b, c);
					// [[
					long d = c - input[b];
					input[b] = c;
					update(tree, v, s, e, b, d);
					// ]
				} else {
					long ret = query(tree, v, s, e, b, (int) c);
					if (type == Type.PRODUCT) {
						sb.append(ret % MOD).append("\n");
					} else {
						sb.append(ret).append("\n");
					}
				}
			}
		} else if (type == Type.MIN_MAX) {
			type = Type.MIN;
			long[] min_tree = new long[N * 4];				
			init(min_tree, v, s, e);

			type = Type.MAX;
			long[] max_tree = new long[N * 4];				
			init(max_tree, v, s, e);

			for (int i = 0; i < M + K; i++) {
				st = new StringTokenizer(br.readLine());
				int a = Integer.parseInt(st.nextToken());
				int b = Integer.parseInt(st.nextToken());

				type = Type.MIN;
				long ret1 = query(min_tree, v, s, e, a, b);
				sb.append(ret1).append(" ");
				type = Type.MAX;
				long ret2 = query(max_tree, v, s, e, a, b);
				sb.append(ret2).append("\n");
			}
		}
		System.out.println(sb.toString());
	}
}
```
{% endtab %}
{% endtabs %}



### 참고자료

[https://m.blog.naver.com/ndb796/221282210534](https://m.blog.naver.com/ndb796/221282210534)
