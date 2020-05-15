---
title: "[프로그래머스] 2018카카오 신입 공채 1번 - 비밀지도"
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
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/17681)

지도 두개가 행별로 정수 형태로 주어진다. 지도가 만약 " ##  #" 로 주어질 경우, 011001 -> 9로 주어진다.

지도 두개를 합칠 때, 하나의 칸이라도 #인 경우 합친 지도 역시 #가 된다.

합쳐지는 지도를 출력하는 문제이다.

## 코드 리뷰
__풀이 시간: 5분__

단순 비트 연산을 묻는 문제였다.

두개의 지도를 합칠때는 or연산, 합친 지도를 이진수로 바꾸는 과정은 and연산을 통해 풀면 금방 풀 수 있다.

비트연산이 아닌 %연산이나 조건문을 통해 푸는 것보다는 단순 비트연산을 통해 해결해보는 것을 추천한다.

```java
public class Kakao_2018BlindRecruitment_3_비밀지도 {
	public String[] solution(int n, int[] arr1, int[] arr2) {
		String[] answer = new String[arr1.length];
		for(int i=0; i<arr1.length; i++) {
			int bit = arr1[i]|arr2[i];
			StringBuilder sb = new StringBuilder();
			for(int b=arr1.length-1; b>=0; b--) {
				if((bit&(1<<b))>=1) sb.append("#");
				else sb.append(" ");
			}
			answer[i] = sb.toString();
		}
		return answer;
	}
}
```