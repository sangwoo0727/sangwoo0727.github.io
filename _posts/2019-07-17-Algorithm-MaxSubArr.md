---
title: "[알고리즘설계] 분할정복 1"
categories:
  - Algorithm
tags:
  - Divide&conquer
  - Algorithm
  - MaxSubArray
comments:
  - true
toc: true
toc_sticky: true
---

## [알고리즘 설계] 분할정복 1
* 이번 포스트에선 분할정복의 기본적인 설명과 분할정복기반의 대표 문제인 최대부분배열의 문제에 대해서 소개한다.
* 최대부분배열의 문제는 분할정복이 아닌 다이나믹 프로그래밍으로 풀면 더빠르게 풀 수 있지만, 분할정복을 통해 푸는 방법을 소개한다.
  
### 분할정복의 단계
1. 분할 : 현재의 문제와 동일하되 입력의 크기가 더 작은 다수의 부분 문제로 분할한다.
2. 정복 : 부분 문제를 재귀적으로 풀어서 정복한다. 부분 문제의 크기가 충분히 작으면 직접적인 방법으로 푼다.
3. 결합 : 부분 문제의 해를 결합하여 원래 문제의 해가 되도록 만든다.

* 부분 문제가 재귀적으로 풀 수 있을만큼 충분히 클 때, 재귀 대상이라고 한다.
* 더 이상 재귀를 호출 할 수 없을 때는 베이스 케이스라고 한다.

### 점화식
* 점화식은 더 작은 입력에 대한 자신의 값으로 함수를 나타내는 방정식 또는 부등식이다.

```
T(n) = Θ(1) , if n=1
     = 2*T(n/2) + Θ(n) , if n>1
```
* 이런식으로 점화식을 표현할 수 있다.
* 점화식을 푸는 방법은 3가지가 존재한다.

1. 치환법(substitution method) : 한계를 추측한 후 그 추측식이 옳음을 증명하기 위하여 수학적 귀납법을 사용.
2. 재귀 트리 방법(recursion tree method) : 점화식을 각 노드가 재귀 호출의 해당 레벨에 따른 비용을 나타내도록 만든 트리로 변환하여 점화식을 푼다. 후에 합의 한계를 구하는 기법을 이용.
3. 마스터 방법(master method) : 마스터 방법은 세 가지 경우에 대한 암기가 필요하지만, 한 번 기억해 두면 점근적 한계를 쉽게 구할 수 있다. 다음 포스팅에 소개하겠다.

* 때로는 점화식이 등식이 아닌 부등식의 형태로 나타내지는데, T(n) <= 이런 경우는 T(n) 의 상한만을 나타내기 때문에 빅-오를 사용하고, 반대의 경우는 하한만 나타내기 때문에 오메가를 이용한다.

### 최대부분배열
* 하나의 배열이 주어지고, 연속된 구간의 합 중 최대를 구하는 문제가 된다.
* 배열의 값이 모두 양수일 경우, 전체 구간이 답이므로 쉽다. 음수도 포함될 때에 대해 고민해본다.
* n개의 인덱스에 대해, 완전탐색을 이용하여 진행할 경우(모든 구간에 대한 합을 다 구하는 방법) O(n^2)의 시간이 걸린다.
* 분할 정복을 이용하면 쉽게 해결할 수 있다.
* 배열의 가장 작은 인덱스 번호를 low , 가장 큰 인덱스 번호를 high 라고 할 때, 그 구간의 중간 값을 mid라고 한다.
* 이럴 경우, 모든 연속 부분 배열(A[i..j])은 아래의 세가지 상황에 항상 포함된다.
  
1. A[low..mid] 에 완전히 들어가는 경우.( low <= i , j, <= mid)
2. A[mid+1..high]에 완전히 들어가는 경우. (mid+1<= i, j <= high)
3. 중간값을 지나가는 경우 (low <= i <= mid <=j <= high)

* 그러므로 최대 부분배열 역시 반드시 위의 세가지 상황 중 하나에 포함된다.
* 1번과 2번의 경우는 재귀적으로 찾을 수 있다.
* 3번의 경우는 선형적으로 비례하는 시간에 찾을 수 있다.

```
Find-Max-Crossing-SubArray(A, low, mid , high)
    left-sum = - ∞
    sum = 0
    for i = mid downto low
        sum = sum + A[i]
        if sum > left-sum
            left-sum = sum
            max-left = i
    right-sum = - ∞
    sum = 0
    for j = mid+1 to high
        sum = sum + A[j]
        if sum > right-sum
            right-sum = sum
            max-right = j
    return (max-left, max-right, left-sum + right-sum)
```

* 위의 코드는 3번의 경우에 대한 의사 코드이다. 중간값을 지나가는 부분배열의 경우, mid값에서 왼쪽으로 가는 배열중 가장 큰 배열값과, mid+1에서 오른쪽으로 가는 배열 중 가장 큰 배열값의 합이 중간값을 지나는 최대 부분 배열이 된다.

* 위의 코드는 부분 배열 A[low..high]의 원소가 n개를 가지고 있다면, 호출에 걸리는 시간은 Θ(n)이 된다. 첫번째 for 루프에서 mid-low+1 번의 반복과 두 번째 for 루프에서 high-mid 번의 반복이 일어나므로 총 반복횟수가 n이 된다.

* 위의 의사코드를 이용하여 최대부분배열 전체의 의사코드를 작성하면 다음과 같다.
  
```
Find-Max-SubArray(A, low, high)
    if high==low
        return (low,high,A[low])
    else
        mid = (low+high)/2
        (left-low,left-high,left-sum) = Find-Max-SubArray(A,low,mid)
        (right-low,right-high,right-sum) = Find-Max-SubArray(A,mid+1,high)
        (cross-low,cross-high,cross-sum) = Find-Max-Crossing-SubArray(A,low,mid,high)
        if left-sum >= right-sum and left-sum >= cross-sum
            return (left-low,left-high,left-sum)
        elseif right-sum >= left-sum and right-sum >= cross-sum
            return (right-low, right-high, right-sum)
        else return (cross-low, cross-high , cross-sum)
```

* 최대부분배열 문제를 분할정복으로 풀었을 경우 점화식은 다음과 같다.
```
T(n) = Θ(1) , if n=1
     = 2*T(n/2) + Θ(n) , if n>1
```

* 하나의 문제에 대해 두개의 부분 문제를 풀어야 하므로 2*T(n/2) 가 된다. 또한 중간값을 지나가는 부분배열을 해결하기 위해 위에서 Θ(n)의 수행시간이 걸리는 것을 증명하였다. 
* 마스터 정리를 이용하면 쉽게 T(n) = Θ(nlgn) 이 됨을 알 수 있다. 마스터 정리의 증명은 다음에 포스팅 하겠다.
* 이 다음 포스팅은 최대부분배열의 문제에 대한 실제 c++로 구현한 코드이다.

---
### 출처 : Introduction to Algorithms 번역본 (Thomas H.Cormen등 지음, 문병로 등 옮김, 한빛아카데미) 

