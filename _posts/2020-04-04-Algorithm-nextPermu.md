---
title: "[알고리즘 설계] 자바를 이용한 next_permutation 구현"
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
C++에는 STL에 다음 순열을 탐색해주는 next_permutation 함수가 구현되어 있다.

안타깝게도 자바는 next_permutation 함수를 직접 구현해야해서, 이 코드를 정리해둔다.

이를 통해 전체 순열을 탐색하는 알고리즘을 작성할 수 있다.

아래와 같이 네가지 단계를 기억해야 한다.

![](/assets/img/Algorithm/20200404_1.png)

첫 번째 단계로는 맨 뒤에서부터 탐색하며, 교환할 위치를 찾아야 한다.

뒤쪽부터 탐색하며, i보다 값이 작은 i-1 인덱스를 발견하는 순간이 교환할 위치 i-1이 된다.

두 번째 단계로는 내가 찾은 i-1인덱스의 배열 값과, 교환할 i-1보다 큰 위치 j를 찾는 것이다.

이렇게 찾은 i-1자리와 j의 값을 교환한다.

마지막으로는, 다음 순열의 사전순의 특징을 위해 i번째 인덱스부터 맨마지막 인덱스의 배열 값을 오름차순으로 바꿔주는 작업이 필요하다.


```java
boolean nextPermutation(int[] arr) {
	total++;
	// step1
	int i=N-1;
	while( i>0 && arr[i-1] >= arr[i] ) --i; 
	
    //i가 0이라는 말은 이 순열이 내림차순 형태로 정렬되어 있다는 것.
    //다음 순열이 없는 경우이기 때문에 false를 반환해준다.
	if(i==0) return false;
	
	//step2
    //내가 찾은 교환위치와 교환할 큰 값을 찾는 과정이다.
	int j = N-1;
	while(arr[i-1]>=arr[j]) --j;
	
	//step3
    //j와 i-1번째의 배열 값을 서로 교환해준다.
	int temp = input[i-1];
	input[i-1] = input[j];
	input[j] = temp;
	
	//step4
    //i부터 맨뒤에까지를 오름차순으로 교환해준다.
	int k = N-1;
	while(i<k) {
	    temp=input[i];
		input[i]=input[k];
		input[k]=temp;
		++i; --k;
	}
	return true;		
}
```

## 참고

__(www.nayuki.io/page/next-lexicographical-permutation-algorithm)[https://www.nayuki.io/pagenext-lexicographical-permutation-algorithm]__




