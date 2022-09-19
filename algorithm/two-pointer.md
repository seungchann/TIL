# 투 포인터  
> *'이것이 **코딩 테스트**다 with 파이썬 - 나동빈'* 내용을 참고하여 정리한 게시글입니다.  
## 투 포인터 알고리즘  
* 리스트에 순차적으로 접근해야 할 때 2개의 점의 위치를 기록하면서 처리하는 알고리즘이다.  

## 예시 문제 1. 특정한 합을 가지는 부분 연속 수열 찾기  
  * `1, 2, 3, 2, 5` 를 차례대로 원소로 갖는 리스트가 있다.  
  * 합계 값을 `5`라고 설정했을 때, 3가지 경우의 수만 존재한다.  
  * `(2,3), (3,2), (5)`  
* 투 포인터 알고리즘의 특징은 `2개의 변수를 이용해 리스트 상의 위치를 기록한다는 점` 이다.  
* 부분 연속 수열의 시작점 (start) 과 끝점 (end) 를 기록한다.  
* 특정한 부분합을 M 이라고 할 때, 구제적인 알고리즘은 다음과 같다.  
  * 1. 시작점 (start) 과 끝점 (end) 이 첫 번째 원소의 인덱스 (0) 를 가리키도록 한다.  
  * 2. 현재 부분합이 M 과 같다면 카운트한다.  
  * 3. 현재 부분합이 M 보다 작으면 end 를 1 증가시킨다.  
  * 4. 현재 부분합이 M 보다 크거나 같으면 start 를 1 증가시킨다.  
  * 5. 모든 경우를 확인할 때까지 `b ~ d` 까지의 과정을 반복한다.  
* 만약에 리스트 내 원소에 음수 데이터가 포함되어 있는 경우에는 투 포인터 알고리즘으로 문제를 해결할 수 없다.  
```python3
n = 5 # 데이터의 개수 N
m = 5 # 찾고자 하는 부분합 M
data = [1, 2, 3, 4, 5] # 전체 수열

count = 0
interval_sum = 0
end = 0

# start 를 차례대로 증가시키며 반복
for start in range(n):
  # end 를 가능한 만큼 이동시키기
  while interval_sum < m and end < n:
    interval_sum += data[end]
    end += 1
  # 부분합이 m일 때 카운트 증가  
  if interval_sum == m:
    count += 1
  interval_sum -= data[start]
  
print(count)
```

## 예시 문제 2. 정렬되어 있는 두 리스트의 합집합  
* 이 문제에서는 이미 정렬되어 있는 2개의 리스트가 입력으로 주어진다.  
* 이때 두 리스트의 모든 원소를 합쳐서 정렬한 결과를 계산하는 것이 문제의 요구사항이다.  
* 2개의 리스트 A, B 가 주어졌을 때, 2개의 포인터를 이용하여 각 리스트에서 처리되지 않은 원소 중 가장 작은 원소를 가리키면 된다.  
* 이 문제에서는 기본적으로 이미 정렬된 결과가 주어지므로 리스트 A, B의 원소를 앞에서부터 확인하면 된다. (O(N + M))  
  * 1. 정렬된 리스트 A 와 B 를 입력받는다.  
  * 2. 리스트 A 에서 처리되지 않은 원소 중 가장 작은 원소를 i 가 가르키도록 한다.  
  * 3. 리스트 B 에서 처리되지 않은 원소 중 가장 작은 원소를 j 가 가르키도록 한다.  
  * 4. A[i] 와 B[j] 중에서 더 작은 원소를 결과 리스트에 담는다.  
  * 5. 리스트 A 와 B 에서 더 이상 처리할 원소가 없을 때까지 `b ~ d` 까지의 과정을 반복한다.  
```python3
# 사전에 정렬된 리스트 A 와 B 선언  
n, m = 3, 4
a = [1, 3, 5]
b = [2, 4, 6, 8]

# 리스트 A 와 B 의 모든 원소를 담을 수 있는 크기의 결과 리스트 초기화
result = [0] * (n + m)
i = 0
j = 0
k = 0

# 모든 원소가 결과 리스트에 담길 때까지 반복
while i < n or j < m:
  # 리스트 B 의 모든 원소가 처리되었거나, 리스트 A 의 원소가 더 작을 떄  
  if j >= m or (i < n and a[i] <= b[j]):
    # 리스트 A 의 원소를 결과 리스트로 옮기기  
    result[k] = a[i]
    i += 1
  # 리스트 A 의 모든 원소가 처리되었거나, 리스트 B 의 원소가 더 작을 떄  
  else:
    # 리스트 B 의 원소를 결과 리스트로 옮기기
    result[k] = b[j]
    j += 1
  k += 1
  
# 결과 리스트 출력
for i in result:
  print(i, end=' ')
```