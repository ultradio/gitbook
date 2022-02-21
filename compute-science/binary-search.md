---
description: Binary Search
---

# Binary Search

### **이진 탐색**

![](https://lh3.googleusercontent.com/-q9yUxZgrzn8/YROE13rcnZI/AAAAAAAAZpM/\_8fv7VDMRUQ76iMjh9Wh9t6SkScdzoV8ACLcBGAsYHQ/w493-h315/1628669142352856-0.png)

{% tabs %}
{% tab title="Python" %}
```python
print("# 이진탐색 반복문 구현")
seek_element = 7
some_list = [2, 3, 5, 7, 11]

def binary_search(seek, some_list):
    
    some_list = sorted(some_list)
    start = 0
    end = len(some_list)-1
    mid = (start + end)//2

    for i in range(len(some_list)//2 + 1):
        if seek == some_list[mid]:
            return mid
        
        if seek < some_list[mid]:
            end = mid - 1
        else:
            start = mid + 1
        mid = (start + end) // 2
    
    return -1

print(binary_search(seek_element, some_list))
```
{% endtab %}

{% tab title="Java" %}
```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.StringTokenizer;

public class BinarySearch {
	static ArrayList<Integer> some_list = new ArrayList<Integer>();

	public static void main(String[] args) throws Exception {
		System.setIn(new FileInputStream("D:\\CS\\wks\\pro\\src\\basic\\binary_search.txt"));

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int N = Integer.parseInt(st.nextToken());
		int seek = Integer.parseInt(st.nextToken());
		// int[] some_list = new int[N];

		for (int i = 0; i < N; i++) {
			some_list.add(Integer.parseInt(br.readLine()));
			// some_list[i] = Integer.parseInt(br.readLine());
		}
		
		int index = binary_search(seek);
		System.out.println(index);
	}

	public static int binary_search(int seek) {	
		Collections.sort(some_list);
		int start = 0;
		int end = some_list.size() - 1;
		int mid = (start + end) / 2;

		System.out.println(some_list);
		System.out.println(seek);
		System.out.println(String.format("%s, %s, %s", start, end, mid));

		for (int i = 0; i < some_list.size() / 2 +1; i++) {
			if (seek == some_list.get(mid)) {
				return mid;
			}
			if (seek < some_list.get(mid)) {
				end = mid - 1;
			} else {
				start = mid + 1;
			}

			mid = (start + end) / 2;
		}

		return -1;
	}
}
```
{% endtab %}
{% endtabs %}

### 참고자료

[https://kids.kiddle.co/Binary\_search](https://kids.kiddle.co/Binary\_search)
