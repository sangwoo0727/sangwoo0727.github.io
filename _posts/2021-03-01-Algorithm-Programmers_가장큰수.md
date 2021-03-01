---
title: "[프로그래머스] 가장 큰 수"
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

![](/assets/img/Algorithm/20210301.png)

* 배열로 들어온 숫자들의 순서를 바꿔서 가장 큰 수를 만드는 문제이다.

## 풀이 과정
* 정렬을 통해 풀이에 접근하는데, 어떻게 정렬할지에 대해 고민을 좀 오래해서 그에 대한 설명을 적어두려 한다.
* 예시 테스트 케이스처럼 [6,10,2] 가 input으로 들어오는 경우, 6,2,10으로 정렬을 해야 우리가 원하는 답을 구할 수 있다.
* int 자료형으로 들어오기 때문에, 단순히 크기로는 비교를 할 수 없었다.
* 또한 문자열로 배열을 변경 후에 자릿수를 일일이 비교하기에도 꽤 복잡했다 (자리수 일일이 비교)
* 이 부분에서 조금 오래 고민했는데 명쾌한 방법이 떠올랐다.
* 10과 2를 비교할 때 우리가 원하는 순서는 2,10 이다.
* 그러면 문자열로 배열의 원소를 변경했으므로, "10"+"2" 와 "2"+"10"을 비교해서 더 큰 값을 앞에 오도록 정렬을 시키면 모든 조건이 만족하며 해결이 되었다.
* 즉, "10"+"2" = "102" 이고, "2"+"10" = "210" 이므로, 문제에서 원하는 조건에 맞게 딱 정렬을 시킬 수 있게 된다!
* 이 방법이 떠오르고 나니 금새 문제를 해결할 수 있었다.

```java
import java.util.*;

public class LEV2_가장큰수 {
    public String solution(int[] numbers) {
        String[] numbersToString = new String[numbers.length];
        for (int i = 0; i < numbers.length; i++) {
            numbersToString[i] = String.valueOf(numbers[i]);
        }
        Arrays.sort(numbersToString, (o1, o2) -> -(o1 + o2).compareTo(o2 + o1));
        StringBuilder output = new StringBuilder();
        boolean flag = false;
        for (String s : numbersToString) {
            for (char c : s.toCharArray()) {
                if (c != '0') {
                    flag = true;
                    break;
                }
            }
            output.append(s);
        }
        return flag ? output.toString() : "0";
    }
}
```

