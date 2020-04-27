---
title: "[프로그래머스] 2019카카오 겨울 인턴십 2번 - 튜플"
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
## 문제 설명
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/64065)

셀수 있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플이라 한다.

n개의 요소를 가진 튜플을 n-튜플이라하며, 튜플은 (a1,a2,...,an)으로 표현할 수 있다.

튜플은 중복된 원소를 가질 수 있으며 원소는 정해진 순서가 있다. 원소의 순서가 다르면 서로 다른 튜플이다.

이 문제에서는 중복 원소가 없는 튜플이 주어진다. 중복 원소가 없는 튜플이 주어질 때 집합 기호를 이용해 나타낼 수 있다.

```
예를 들어, 튜플이 (a1,a2,a3...,an)일 경우 {{a1},{a1,a2},{a1,a2,a3}...{a1,a2,a3,....,an}} 로 표현할 수 있다.
```

또한 집합은 원소의 순서가 바뀌어도 되므로 중괄호 안에있는 원소들은 순서를 바꿀 수 있다.

특정 튜플을 표현하는 집합이 담김 문자열 s가 주어질 때, s가 표현하는 튜플을 리턴하는 문제이다.

## 코드 리뷰
__풀이 시간: 25분__

이 문제는 다양한 풀이가 나올 수 있을거라고 생각한다.

먼저, 문자열을 집합을 기준으로 나눈 후, 집합을 기준으로 각 집합에 원소를 넣어나가는 방법을 생각했다. 실제로 첫 인턴 코테를 볼 때는 이 방식으로 풀었던 것 같다.

이렇게 풀면, 배열의 길이 순서로 오름차순 정렬을 하면, 1,2,3,4.. 순으로 정렬이 될 것이고, 길이가 1인 집합에 있는 원소가 튜플의 첫 번째 원소라는 것을 알 수 있다.

이런 식으로 다음 집합은 이전 집합에 포함되어있지 않은 원소를 뽑아낼 수 있고, 그 원소가 튜플의 다음 원소라는 방법으로 구현해주면 된다.

이번에는 조금 다른 방법으로 접근해서 풀었다.

애초에 들어온 문자열을 한번에 파싱을 시켜주어 한번에 정수 배열을 만들어 주는 것이다.

그 후, 이 배열을 한번 쭉 순차탐색하면서 map에 키가 정수, value에는 그 정수가 발견된 횟수를 저장해나간다.

그 후, map을 value 값으로 내림차순 정렬 시키면 이때 map의 키값이 발견되는 순서가 바로 튜플의 순서가 된다.

조금 더 간단한 방법인 것 같다.

이전에 포스팅한 1번과 2번문제를 합쳐서 코딩테스트를 볼 때 30분안에 풀면 합격 확률이 많이 높아질 거라고 생각한다.

```java
import java.util.*;
public class Solution{
	static HashMap<Integer,Integer> map = new HashMap<>();
	public int[] solution(String s) {
		int[] answer = {};
		StringTokenizer st = new StringTokenizer(s,"{},");
		while(st.hasMoreTokens()) {
			int tmp = Integer.parseInt(st.nextToken());
			if(map.containsKey(tmp)) map.put(tmp, map.get(tmp)+1);
			else map.put(tmp, 1);
		}
		List<Integer> keyList = new ArrayList<>(map.keySet());
		Collections.sort(keyList,(o1, o2)->{
			return -map.get(o1).compareTo(map.get(o2));
		});
		answer = new int[map.size()];
		int idx=0;
		for(Integer key : keyList) {
			answer[idx++] = key;
		}
		return answer;
	}
}
```

## 참고 내용
* HashMap을 value를 통해 키값을 정렬하려 할 경우, ketSet()을 통해 키 리스트를 만들어준 후, value로 다시 한번 정렬해주면 된다.