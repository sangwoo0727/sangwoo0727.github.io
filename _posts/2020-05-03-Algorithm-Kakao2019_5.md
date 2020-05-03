---
title: "[프로그래머스] 2019카카오 겨울 인턴십 5번 - 징검다리 건너기"
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
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/64062)

![](/assets/img/Algorithm/20200503_1.png)

징검다리의 돌마다 숫자가 있다. 징검다리를 건널 수 있는 사람의 수는 무제한이고, 건너는 첫번째 사람은 모든 징검다리의 돌을 밟아야 한다.

밟고나면 징검다리의 숫자는 1씩 줄어들게 된다.

한명이 건너고 나면 그 다음 사람이 건너기 시작한다. 만약 징검다리의 돌에 적힌 숫자가 0이 있으면 그 다리를 밟지 않고 건너야 한다.

이렇게 한명 한명 건너기 시작하다가 연속으로 건너뛰어야 하는 돌의 수가 주어진 k보다 커질 경우 더 이상 사람들은 건너지 못한다.

이때 이 징검다리를 건널 수 있는 최대 인원 수를 구하는 문제이다.

## 코드 리뷰
__풀이 시간 : 35분__

이 문제는 마지막 문제여서 실제 코테를 볼 때 시간이 부족하여 문제도 읽지도 못했었다.

k의 범위는 1~200,000,000이고, 징검다리의 배열의 크기는 최대 200,000인 효율성까지 통과해야 하는 문제이다.

마지막 문제이고, 효율성때문에 겁 먹고 막연히 어렵다고만 생각했는데 생각보다 정형화된 문제라서 접근만 잘하면 금방 풀 수 있다.

이 문제는 최적화문제를 결정 문제로 바꾸어 푸는 파라메트릭 서치로 풀 수 있다.

즉, 건널 수 있는 사람의 수를 이진탐색으로 탐색한다. 건널 수 있는 사람의 수는 무한으로 주어졌지만, 결국 최대 2억명까지만 건널 수 있기 때문에 1~2억을 이진탐색 하는 것이다.

결정 문제로 바꾸어서 1억명이 이 다리를 건널 수 있냐? 라는 질문으로 풀어보고, 조건에 맞게 범위를 줄여나가는 것이다.

한번 mid값을 설정할 때마다 징검다리를 건너보아야 하므로 사람의 수(최악의 경우 2억)를 N이라하고 징검다리 배열의 크기를 M(최악의 경우 이십만)이라 할때 시간복잡도는 Mlog(N)이 되어 효율성을 충분히 통과할 수 있게 된다.

이진 탐색 문제는 어떤 것을 이진 탐색 할 것인지를 찾기만 하면 수월하게 풀 수 있다!!

```java
public class Kakao_2019겨울인턴십_5_징검다리건너기 {
	public int solution(int[] stones, int k) {
		int ans = 0;
		int l = 1;
		int r = 200000000;
		while(l<=r) {
			int m = (l+r)/2;
			int cnt=0,tmp=0;
			for(int i=0; i<stones.length; i++) {
				if(stones[i]>m) {
					tmp = Math.max(tmp, cnt);
					cnt = 0;
				}else {
					cnt++;
				}
			}
			tmp = Math.max(tmp, cnt);
			if(tmp<k) {
				l = m+1;
			}else {
				ans = m;
				r = m-1;
			}
		}
		return ans;
	}
}
```