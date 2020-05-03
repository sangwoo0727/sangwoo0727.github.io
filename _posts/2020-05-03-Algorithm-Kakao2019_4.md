---
title: "[프로그래머스] 2019카카오 겨울 인턴십 4번 - 호텔 방 배정"
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
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/64063)

![](/assets/img/Algorithm/20200503_2.png)

방의 개수는 1~ 10^12승 이하의 자연수로 주어진다.

고객들은 최대 200,000명이 자신이 원하는 방 번호를 순차적으로 말한다.

원하는 방번호가 비어있을 경우, 즉시 배정하고, 차있을 경우 원하는 방보다 크면서 비어있는 방 중 가장 번호가 작은 방을 배정한다.

모든 고객이 방을 배정받을 수 있는 경우만 입력으로 주어진다.

## 코드 리뷰
__풀이 시간: 1시간__

고객 한명이 말한 방번호를 통해 들어갈 수 있는 방번호를 부모(집합의 루트)로 가지고 있는 유니온 파인드 알고리즘을 먼저 생각해냈다.

예를 들어, 1번 방이 비어있을 경우 부모는 1번 방이고, 그 1번 방을 배정해 준 후, 다음 번호가 속해있는 집합과 union을 해주고, 만약 1번 방을 요청했는데 1번 방이 차있을 경우 1번 방의 부모 번호를 할당해주고 다시 다음 번호가 속해있는 집합과 union 해주는 식의 유니온 파인드 아이디어를 냈는데, 일반적인 유니온 파인드 방법으로는 부모번호를 저장하는 배열의 크기가 10^12승개가 필요해서 이 부분에 대한 아이디어가 필요하였다.

그래서 처음부터 모든 부모 번호를 저장하는 배열을 만들지 않고, HashMap을 통하여 key에 번호, value에 부모 번호를 저장해나가는 식으로 메모리를 절약해주었다.

유니온 파인드를 통해 시간 효율성을 잡았고, HashMap을 이용하여 메모리 효율성을 잡아야 하는 꽤나 헷갈렸던 문제였다.

```java
import java.util.*;

public class Kakao_2019겨울인턴십_4_호텔방배정 {
    static HashMap<Long, Long> map = new HashMap<>();
    public long[] solution(long k, long[] room_number) {
        long[] answer = new long[room_number.length];
        for(int i=0; i<room_number.length; i++) {
        	long n = room_number[i];
        	answer[i] = find(n);
        	merge(answer[i],answer[i]+1);
        }
        return answer;
    }
    static long find(long n) {
		if(!map.containsKey(n) || map.get(n)==n) {
			map.put(n, n);
			return n;			
		}else {
			map.put(n, find(map.get(n)));
			return map.get(n);
		}
	}
    static void merge(long n, long m) {
		n = find(n);
		m = find(m);
		if(n<m) map.put(n, m);
		else map.put(m, n);
	}
}

```
## 전체 후기
2019 카카오 개발자 겨울 인턴십의 개인적인 난이도는 4.호텔 방 배정 > 5. 징검다리 건너기 > 3.불량 사용자 > 2.튜플 > 1.크레인 인형뽑기 게임 이었던 것 같다.

이전에 3번까지는 풀었던 문제였기 때문에 5문제를 3시간 정도에 푼 것 같지만, 실제 시험을 볼 때는 긴장때문에 실수도 은근히 많이해서 디버깅 하는데 시간이 꽤 걸린다.

최대한 앞 부분 문제들은 실수를 줄이기 위해 완전탐색을 통해 간결한 코드로 풀어나가고, 효율성 문제를 천천히 생각해나가며 풀이를 떠올리고 정확한 시간 복잡도를 계산하고 난 후 코드를 깔끔하게 짜는 연습을 해야겠다.

그럼 이만~


