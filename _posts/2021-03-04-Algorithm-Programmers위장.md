---
title: "[프로그래머스] 위장"
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

![](/assets/img/Algorithm/20210304.jpg)

* 2차원 배열이 ('옷의 이름', '옷의 종류')로 리스트로 들어온다.

## 풀이 과정

* 해쉬맵을 이용해 옷의 종류별로 옷이 몇 개가 존재하는지 저장했다.
* 이후, 조건에 맞게 스파이 옷을 조합시켜야 하는데, 아예 안입는 경우를 제외한 1벌씩 입는 경우, 2벌씩 입는 경우 ... 옷의 종류만큼 입는 경우를 다 탐색할 경우 문제가 생긴다.
* 옷은 최대 30벌이 입력으로 들어올 수 있는데, 모든 옷이 다른 종류에 속할 경우 조합탐색에 있어 `30C1 + 30C2 + 30C3 ...` 의 가지수가 생기기 때문이다.
* 간단하게 계산하는 방법을 떠올려야 했는데, 쉽지 않았다.
* 하나의 종류에 옷이 N개가 있을 경우, 이 종류에서 옷을 꺼내입는 경우는 N+1이 된다.
* ex) headgear 종류의 옷이 N개가 있을 때, 안입는 경우, 1번 옷 입는 경우, 2번 옷 입는 경우 ... N번 옷을 입는 경우.
* 모든 종류의 옷을 선택할 수 있는 가지수를 곱해주면, 빠르게 해결할 수 있는 식이 완성된다.
* 이를 토대로 해쉬맵의 사이즈만큼 돌면서 value+1을 answer에 곱해나가주었다.
* 마지막엔 모든 옷을 안입는 경우가 생기기 때문에 -1을 해주었다.

```java
import java.util.*;

public class LEV2_위장 {
    public int solution(String[][] clothes) {
        int answer = 1;
        Map<String, Integer> map = new HashMap<>();
        for (String[] clothe : clothes) {
            String kind = clothe[1];
            map.put(kind, map.getOrDefault(kind, 0) + 1);
        }
        for (int count : map.values()) {
            answer *= (count + 1);
        }
        return answer - 1;
    }
}
```

