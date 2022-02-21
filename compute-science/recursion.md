# Recursion

&#x20;재귀함수는 base case와 recursive case로 나눠서 문제를 해결할 수 있다. recursive case는 같은 형태의 더 작은 부분 문제를 재귀적으로 푸는 case이고 base case는 더 작은 부분 문제로 나누지 않고도 바로 답을 알 수 있는 경우이다. \
재귀함수와 반복문은 서로 전환이 가능하다.

#### **피보나치 수열**

```python
print("# n번째 피보나치 수를 리턴")
# 0 1 1 2 3 5 8 13 21 ...
def fib(n):   
    if n ==1 or n == 2: # base case
        return 1
    else: # recursive case
        return fib(n-1) + fib(n-2)

# 테스트: fib(1)부터 fib(10)까지 출력
for i in range(1, 11):
    print(fib(i))
```

&#x20;**합**

```python
print("# 1~10까지 합구하기")
def triangle_number(n):
    if n == 1: # base case
        return 1    
    return n + triangle_number(n - 1) # recursive case

for i in range(1, 11):
    print(triangle_number(i))
```

```python
print("# n의 각 자릿수의 합을 리턴")
# n = 123 → 1+2+3=6
def sum_digits(n):
    if n < 10: # base case
        return n
    else: # recursive case   
        divide = n // 10
        remain = n % 10
        return sum_digits(divide) + remain

# 테스트
print(sum_digits(22541))
print(sum_digits(92130))
print(sum_digits(12634))
print(sum_digits(704))
print(sum_digits(3755))
```

```python
print("# 1부터 n 까지 연속한 숫자의 합을 구하는 알고리즘")
def sum_number(n):
    return n*(n+1) // 2
   
print(sum_number(10))
```

```python
print("# 1부터 n 까지 연속한 숫자 제곱의 합을 구하는 알고리즘")
def _sum_number(n) :
    return n*(n+1)*(2*n+1) // 6
    
print(_sum_number(3))
```

&#x20;**리스트 뒤집기**

```python
print("# 파라미터 some_list를 거꾸로 뒤집는 함수")
def flip(some_list):
    if len(some_list) < 2:
        return some_list
    return flip(some_list[1:]) + some_list[:1]

# 테스트
some_list = [1, 2, 3, 4, 5, 6, 7, 8, 9]
some_list = flip(some_list)
print(some_list)
```

&#x20;
