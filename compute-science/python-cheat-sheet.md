---
description: Data Structure
---

# Data Structure

### 유용한 코드 정리

{% tabs %}
{% tab title="Python" %}
```python
type
max, min
int, str,
list<append, pop, insert, del, index, reverse, sort>
sorted(some_dict.items(), lambda x: x[1], reverse=True)
reversed(some_list)
''.join(some_list)
len(some_list)
''.join(reversed(some_str))
some_str.startswith(prefix_str)
zip(items, items[1:])
enumerate(some_list)
range(0, len(some_list))
map(str, some_list)
map(''.join, permutations(items, 2))
map(''.join, combinations(items, 2))
```
{% endtab %}

{% tab title="Java" %}

{% endtab %}
{% endtabs %}

찾고자 하는 문자열A가 문자열B의 맨 앞에 있는지 여부

{% tabs %}
{% tab title="Python" %}
```python
some_str = 'abcde'
print(some_str.startswith('ab'))
print(some_str.startswith('cde'))

True
False
```
{% endtab %}

{% tab title="Java" %}
```python
```
{% endtab %}
{% endtabs %}

앞뒤 값 비교하기

{% tabs %}
{% tab title="Python" %}
```python
items = [1,2,3]
for i in zip(items, items[1:]):
  print(i)

(1, 2)
(2, 3)
```


{% endtab %}

{% tab title="Java" %}

{% endtab %}
{% endtabs %}

문자열 뒤집기

{% tabs %}
{% tab title="Python" %}
```python
some_str = 'ABC'
print(''.join(reversed(some_str)))

CBA
```
{% endtab %}

{% tab title="Java" %}
```python
```
{% endtab %}
{% endtabs %}

리스트 & 딕셔너리 정렬

{% tabs %}
{% tab title="Python" %}
```python
some_list = {1, 2, 3}
print(sorted(some_list))
some_dict = {'a':1, 'b':2, 'c':3}
print(sorted(some_dict.items(), reverse=True))
tuple_dict = {'a':(1, 3), 'b':(2, 2), 'C':(3, 1)}
print(sorted(tuple_dict.items(), key=lambda x: x[1]))

[1, 2, 3]
[('c', 3), ('b', 2), ('a', 1)]
[('a', (1, 3)), ('b', (2, 2)), ('C', (3, 1))]
```
{% endtab %}

{% tab title="Java" %}

{% endtab %}
{% endtabs %}

가장큰수 (문자열비교는 아스키 값으로 치환되어 정렬)

{% tabs %}
{% tab title="Python" %}
```python
numbers = [12, 10, 22]
some_list = list(map(str, numbers))
print(some_list)
some_list.sort(key=lambda x: x*3, reverse=True)
print(some_list)
print(''.join(some_list))

['12', '10', '22']
['22', '12', '10']
221210
```
{% endtab %}

{% tab title="Java" %}

{% endtab %}
{% endtabs %}

순열과 조합

순열은 순서대로 뽑는 것

{% tabs %}
{% tab title="Python" %}
```python
from itertools import permutations
items = ['A', 'B', 'C']
print(list(permutations(items)))
print(list(map(''.join, permutations(items)))) # items의 모든 원소를 가지고 순열을 만든다.
print(list(map(''.join, permutations(items, 2)))) # 2개의 원소를 가지고 순열을 만든다

[('A', 'B', 'C'), ('A', 'C', 'B'), ('B', 'A', 'C'), ('B', 'C', 'A'), ('C', 'A', 'B'), ('C', 'B', 'A')]
['ABC', 'ACB', 'BAC', 'BCA', 'CAB', 'CBA']
['AB', 'AC', 'BA', 'BC', 'CA', 'CB']
```
{% endtab %}

{% tab title="Java" %}

{% endtab %}
{% endtabs %}

조합은 순열과 다르게 뽑아서 순서를 고려하지 않는 것 예를들어, 1,2,3,4 중 2개의 숫자를 뽑아 자연수를 만드는 경우 순열은 12,21가 나올 때, 조합은 12만 나옴

{% tabs %}
{% tab title="Python" %}
```python
from itertools import permutations, combinations
items = ['1', '2', '3']
print("순열:{}".format(list(map(''.join, permutations(items, 1)))))
print("순열:{}".format(list(permutations(items, 2))))
print("순열:{}".format(list(map(''.join, permutations(items, 2)))))
print("조합:{}".format(list(map(''.join, combinations(items, 1))))) # 조합을 만들려는 아이템과 조합의 수를 반드시넘겨줘야한다.
print("조합:{}".format(list(combinations(items, 2))))
print("조합:{}".format(list(map(''.join, combinations(items, 2)))))

순열:['1', '2', '3']
순열:[('1', '2'), ('1', '3'), ('2', '1'), ('2', '3'), ('3', '1'), ('3', '2')]
순열:['12', '13', '21', '23', '31', '32']
조합:['1', '2', '3']
조합:[('1', '2'), ('1', '3'), ('2', '3')]
조합:['12', '13', '23']
```
{% endtab %}

{% tab title="Java" %}

{% endtab %}
{% endtabs %}

소수찾기

{% tabs %}
{% tab title="Python" %}
```python
def prime_check(n):
  if n < 2:
    return False
  for i in range(2, n):
    if n % i == 0:
      return False
  return True

print(prime_check(1))
print(prime_check(2))
print(prime_check(3))
print(prime_check(9))

False
True
True
False
```
{% endtab %}

{% tab title="Java" %}

{% endtab %}
{% endtabs %}

매번 정렬이 필요한 경우 사용

{% tabs %}
{% tab title="Python" %}
```python
import heapq
l = [3,2,1]
heapq.heapify(l)
print(l)
print(heapq.heappop(l))
print(l)
heapq.heappush(l, 4)
print(l)

[1,2,3]
1
[2,3]
[2,3,4]
```
{% endtab %}

{% tab title="Java" %}
```
// Some code
```
{% endtab %}
{% endtabs %}

