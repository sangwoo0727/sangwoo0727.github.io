---
title: "[프로그래머스] 2019카카오 겨울 인턴십 1번 - 크레인 인형뽑기 게임"
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
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/64061?language=java)

2차원 정사각형 배열(board)이 주어진다. 이 안에는 숫자로 표현된 인형들이 아래부터 차곡차곡 쌓여있다.

숫자가 다르면 서로 다른 인형이다.

moves라는 1차원 배열에는 크레인이 board배열 위를 좌우로 움직이다가 멈춘 위치가 적혀있다.

멈춘 위치에서 크레인은 그 열에 있는 인형 중 가장 위에 있는 인형을 뽑게된다.

뽑은 인형을 바구니에 쌓아나가되, 새로 뽑은 인형이 바구니의 맨위에 있는 인형과 같을 경우 그 인형 두개는 터지게 된다.

크레인의 작동이 끝났을 경우 사라진 인형의 개수를 구하는 문제이다.

## 코드 리뷰
__풀이 시간 : 6분__

(이 부분은 지극히 제 개인적인 생각이므로 참고해주시기 바랍니다!)

이 문제는 1번 문제로 나온 문제이다. 누구나 다 풀 수 있고, 맞추냐 못맞추냐의 문제가 아니라 다른 어려운 문제를 풀기 위해 누가 얼마나 빨리 푸냐가 중요하다.

어떻게 풀지에 대해 고민하지 말고 어떻게 효율성을 높일지에 대해 고민하지 말자.

그냥 무조건 풀이 방법 떠올리자마자 바로 코딩 시작해야 한다.

board 배열 역시 제일 큰 크기가 30*30이다. 실행 시간 줄인다고 첫 인형 발견 위치 자료구조로 저장해놓을 생각하고 그러면 코드 짜는데 시간이 더 오래 걸릴 수 있다.

다른 문제의 풀이 시간 절약을 위해 그냥 무조건 완전탐색으로 5분 안에 끝내자.

바구니는 스택으로 풀면 금방 해결할 수 있다.

```java
import java.util.*;
class Solution {
   static Deque<Integer> st = new ArrayDeque<>();
	public int solution(int[][] bd,int[] moves) {
		int ans = 0;
		for(int m=0; m<moves.length; m++) {
			int j = moves[m]-1;
			for(int i=0; i<bd.length; i++) {
				if(bd[i][j]!=0) {
					if(st.isEmpty() || st.peekLast()!=bd[i][j]) {
						st.offerLast(bd[i][j]);
					}else {
						st.pollLast();
						ans+=2;
					}
					bd[i][j]=0;
					break;
				}
			}
		}
		return ans;
	}
}
```

## 참고 내용
Stack<E> 컬렉션 클래스는 자바 초기에 정의된 클래스로써, 현재는 거의 사용되지 않고 있다. Stack<E>가 상속하는 Vector<E> 클래스 역시 현재는 거의 사용되지 않는다.

Stack<E>는 동기화된 클래스로 멀티스레드에 안전하지만, 그만큼 성능 저하가 발생한다.

자바를 통해 스택을 구현하려 할 때는 스택 대신 덱을 사용하자.
