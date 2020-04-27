---
title: "[프로그래머스] 2019카카오 겨울 인턴십 3번 - 불량 사용자"
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
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/64064)

![](/assets/img/Algorithm/20200428_1.png)

전체 사용자 아이디 목록과 불량 사용자 아이디 목록이 String 배열 형태로 들어온다.

불량 사용자는 일반 사용자 아이디와 다르게 개인 정보 보호를 위해 아이디 중 일부 문자를 '*'로 전달 한다.(그림 참조)

전체 사용자 아이디 목록을 바탕으로 불량 사용자에 매핑이 되어 나타낸 아이디 목록을 제재 아이디 목록이라고 한다.

구하고자 하는 건 나올 수 있는 제재 아이디 목록의 가지수이다.

이때, 제재 아이디 목록에서 나열된 아이디의 순서와 관계없이 아이디 목록의 내용이 동일하면 같은 가지수로 처리한다.

## 코드 리뷰
__풀이 시간 : 50분__ 

전체 사용자 아이디 목록(user_id 배열)과 불량 사용자 아이디 목록(banned_id 배열)의 크기는 1이상 8이하이다.

또한 아이디 하나 당 길이 역시 8이하의 문자열이다.

범위를 보고 모든 경우의 수를 탐색하는 완탐문제를 파악해야한다. 범위가 이렇게 주어졌는데 효율적인 코드 생각하는 순간 시험 볼 때 큰일난다.

즉, 전체 사용자 아이디 목록에서 불량 사용자 아이디 목록의 가지수를 뽑는 순열(nPr)의 문제이다.

조합이 아닌 이유에 대해 잘 생각해봐야 하는데, 각각의 불량 아이디 목록에 들어가는 사용자 아이디의 순서는 상관이 있다(매핑의 경우 때문에)

다만, *rodo, *rodo로 주어진 경우, 순열로 구하게되면 prodo,crodo 혹은 crodo,prodo 로 들어가 중복이 발생하는 경우에 대한 예외 처리를 해주어야 한다.

중복을 확인하기 위해 나는 HashSet과 비트마스킹을 이용하였다.

전체 사용자 아이디 목록 중 하나를 뽑을 때마다 그 배열 인덱스 번호에 맞게 비트마스킹을 해주었고, 불량 사용자 아이디 개수만큼 다 뽑은 경우, 비트마스킹을 한 정수를 HashSet에 넣어주었다.

이러면, 같은 정수는 HashSet에 들어가지 않으므로 중복을 제거하고 나온 가지수를 셀 수 있다.

풀이 시간을 줄이기 위해 이런 유형의 문제를 많이 풀어봐야겠다..

```java
import java.util.*;

public class Solution{
	static Set<Integer> set = new HashSet<>();
	static int R;
	public int solution(String[] user_id, String[] banned_id) {
		R = banned_id.length;
		permu(0,0,user_id,banned_id);
		return set.size();
	}
	static void permu(int r,int bit, String[] uid, String[] bid) {
		if(r==R) {
			set.add(bit);
			return;
		}
		for(int i=0; i<uid.length; i++) {
			if(((bit>>i)&1)==0) {
				boolean flg = true;
				String us = uid[i];
				String bs = bid[r];
				if(us.length() == bs.length()) {
					for(int j=0; j<us.length(); j++) {
						if(us.charAt(j) == bs.charAt(j) || bs.charAt(j)=='*') {
							continue;
						}else flg = false;
					}
				}else flg = false;
				if(flg) {
					permu(r+1,(bit|(1<<i)),uid,bid);
				}
			}
		}
	}
}
```