---
title: "[프로그래머스] 2019카카오 신입 공채 3번 - 후보키"
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
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/42890)

![](/assets/img/Algorithm/20200509_1.png)

데이터베이스 테이블이 2차원 배열로 주어지고, 후보키에 정의에 의해 후보키의 개수를 구하는 문제이다.

후보키에 대해 자세히 모르더라도 문제에서 상세하게 알려주었기 때문에 충분히 이해하고 풀 수 있는 문제다.

## 코드 리뷰
어트리뷰트의 조합을 통해 나올 수 있는 모든 부분집합을 구하고, 이를 통해 현재 뽑은 어트리뷰트의 조합이 후보키가 될 수 있는지 판별하는 문제이다.

단, 후보키의 최소성조건에 의해 어트리뷰트의 부분집합의 순서가 매우 중요한데, 이 부분에서 일반적인 부분집합을 구하는 코드로 짰다가 반례를 못잡아서 한참을 고민했다..

세가지의 어트리뷰트가 있다고 할 때, 비트연산을 통해 나올 수 있는 부분집합은(000은 빼야한다) 001, 010, 011, 100, 101, 111 이런식으로 진행이 된다.

하지만, 최소성의 조건에 의해 부분집합은 001,010,100,011,101,111 이런식으로 탐색을 해줘야한다.

그렇기 때문에 먼저 모든 부분집합을 구해주고, 조건에 맞게 정렬을 시켜주었다.

그 후 부분집합을 순서대로 보며, 내가 뽑은 부분집합이 최소성의 조건에 만족할 때, 이 부분집합이 후보키가 될 수 있는지를 판별해주었다.

StringBuilder를 통하여 튜플에서 뽑은 어트리뷰트에 해당되는 값들만 연결시켜, set에 넣어주고, 마지막에 set의 크기가 로우의 크기와 같을 때 후보키의 개수에 포함시켜주었다.

이 경우 단순히 이어주기만 할 경우, 

"one" "two"

"on"  "etwo" 

이런 경우 StringBuilder로 문자열을 이어줄 경우 같은 문자열이 되므로, Set에 두가지 모두 들어가지 못하기 때문에 연결시켜줄 때 중간에 공백을 하나씩 넣어주었다.

```java
import java.util.*;

public class Kakao_2019BlindRecruitment_3_후보키 {
	static HashSet<Integer> set = new HashSet<>();
	static List<Integer> list = new ArrayList<>();
	static HashSet<String> sset;
	static int N,M;
	public int solution(String[][] relation) {
		N = relation.length;
		M = relation[0].length;
		for(int i=1; i<(1<<M); i++) { // 모든 부분집합을 구해준다.
			list.add(i);
		}
		Collections.sort(list, (Integer o1, Integer o2)->{
			int cnt = 0, cnt2 = 0;
			for(int i=0; i<M; i++) {
				if((o1 &(1<<i))>0) cnt++;
				if((o2 &(1<<i))>0) cnt2++;
			}
			if(cnt==cnt2) return o1.compareTo(o2);
			else return cnt-cnt2;
		}); // 위에서 말한 조건대로 정렬을 한다.

		for(int bit: list) { //모든 비트에 대해 순차 탐색
			boolean flg = false;
			for(int n:set) {
				int tmp = n|bit;
				int cnt=0, cnt2=0;
				for(int i=0; i<M; i++) {
					if((bit &(1<<i))>0) cnt++;
					if((tmp &(1<<i))>0) cnt2++;
				}
				if(cnt==cnt2) flg = true;
			}//최소성을 만족하는 지 검사.
			if(flg) continue;
			sset = new HashSet<>();
			for(int i=0; i<N; i++) {
				StringBuilder sb = new StringBuilder();
				for(int j=0; j<M; j++) {
					if((bit&(1<<j))>0) {
						sb.append(relation[i][j]).append(" ");
					}
				}
				sset.add(sb.toString());
			}
			if(sset.size()==N) {//유일성을 만족한다면 후보키에 추가시켜준다.
				set.add(bit);
			}
		}
		return set.size();
	}
}
```

