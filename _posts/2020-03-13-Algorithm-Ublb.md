---
title: "[알고리즘 설계] 이진탐색, 그리고 상/하한선(upper bound,lower bound)"
categories:
  - Algorithm
read_time: false
tags:
  - Algorithm
comments:
  - true
toc: true
toc_sticky: true
---
주어진 배열에서 중복되지 않은 값이 주어질 때, 데이터 내에 특정 값이 존재하는지 여부를 찾는 방법 중 이진 탐색 방법을 많이 사용한다.

배열을 정렬한 후, 데이터를 반으로 쪼개 값이 존재하는지 확인해나가는 알고리즘이다.

이진탐색이 특정 값을 정확히 찾는 것이라면, Lower Bound와 Upper Bound는 이러한 이진탐색 방법을 약간 응용하여, 자료를 탐색하는 알고리즘이다.

C++에는 Lower Bound와 Upper Bound가 <algorithm>헤더에 구현되어 있다.

하지만 요즘 자바를 통해 알고리즘 문제를 풀면서, 어제 코드포스 시험을 보며 Upper Bound 문제가 나왔기 때문에 다시 한번 직접 구현을 하며 정리를 해보려 한다.

## Lower Bound
일단 Lower Bound와 Upper Bound는 이진 탐색 방법을 기반으로 사용하기 때문에, 선행 조건으로는 데이터가 정렬이 되어 있어야 한다.
Lower Bound는 데이터 내 특정 K값보다 __같거나 큰 값이__ 처음 나오는 위치를 리턴해주는 알고리즘이다.

![](/assets/img/Algorithm/20200313_1.png)

[출처 http://bajamircea.github.io/coding/cpp/2018/08/09/lower-bound.html](http://bajamircea.github.io/coding/cpp/2018/08/09/lower-bound.html)

중복되는 원소가 여러개 있어도 상관없다. 즉, 찾고자 하는 값 이상이 처음 나타나는 위치를 찾기 위해 사용한다.

이진탐색과 달리, 주어진 값보다 같거나 큰 값이 처음으로 나오는걸 리턴해야 하므로, r = array.length-1이 아닌 array.length로 지정해야 한다.

```java
public static int lowerBound(int[] arr, int l, int r, int key){
    //r은 배열의 사이즈 N을 넣어준다. 
    int m;
    while(l<r){
        m = (l+r)/2;
        if(arr[m]< key) l = m+1;
        else r = m;
    }
    return r;
} 
```

## Upper Bound
Upper Bound는 데이터 내 특정 K값보다 __처음으로 큰 값이 나오는 위치를__ 리턴해주는 알고리즘이다.

역시나, 주어진 값보다 큰 값이 처음으로 나오는걸 리턴해야 하므로 r = array.length-1이 아닌 array.length로 지정해야 한다.

```java
public static int upperBound(int[] arr, int l, int r, int key){
    int m;
    while(l<r){
        m = (l+r)/2;
        if(arr[m]<=key) l=m+1;
        else r=m;
    }
    return r;
}
```

