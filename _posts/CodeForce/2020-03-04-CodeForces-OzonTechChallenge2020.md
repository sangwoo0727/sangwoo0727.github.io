---
title: "[CodeForces] OzonTechChallenge 2020 (Div1+Div2) C번 풀이"
categories:
  - CodeForces
read_time: false
tags:
  - Algorithm
comments:
  - true
toc: true
toc_sticky: true
---
어제 밤 코드포스 Div1+Div2에 참가했다가 참신했던 문제가 있어서 풀이를 남긴다.

## C. Kuroni and Impossible Calculation

![](/assets/img/CodeForce/20200304_1.png)

n과 m이 주어지고, n개수만큼의 수가 주어진다. (a1,a2,.....,an)

이때 이 수들에 대해 위의 문제에서 주어진대로의 연산의 수행 결과를 출력하는 문제이다.

모든 경우를 보는 완전탐색을 이용하면 n의 개수가 2*10^5이기 때문에 시간초과가 난다.

어떻게 접근해야할까? 답은 mod 연산에 있었다.

## 비둘기집의 원리

* 비둘기집 원리는 n+1개의 물건을 n개의 상자에 넣을 때 적어도 어느 한 상자에는 두 개 이상의 물건이 들어 있다는 원리를 말한다. 보통 비둘기와 비둘기집의 형태로 비유되어 쓰이며, '서랍과 양말'로 비유하여 서랍 원칙 또는 디리클레의 방 나누기 원칙이라고 부르기도 하며 구두 상자의 원리라고도 한다. - 위키 백과

n개의 수에 대해 모두 m으로 나눈 나머지를 구해봤을 때, 나머지가 같은 수(a,b라 가정)가 있을 경우, 그 두 수를 빼보면 (a-b)%m 은 0이 된다.

근데 주어진 n의 개수는 최대 이십만이었고, m은 1000이므로, n>m인 경우에 대해서는 비둘기집의 원리에 따라 나머지가 같은 두수의 개수가 무조건 1개 이상 존재한다.

즉, n이 m보다 큰 경우는 무조건 답이 0이되고, m은 1000이므로 1000이하의 n이 주어진 경우는 완전탐색을 통해 구하여도 시간안에 충분히 구할 수 있다는 결론이 나온다.

```java
import java.io.*;
import java.util.*;

public class Solution {
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static StringTokenizer st;
	static int n,m;
	static int[] a;
	static long ans = 1;
	public static void main(String[] args) throws Exception {
		st = new StringTokenizer(br.readLine());
		n = Integer.parseInt(st.nextToken());
		m = Integer.parseInt(st.nextToken());
		a = new int[n];
		st = new StringTokenizer(br.readLine());
		for(int i=0; i<n; i++) {
			a[i] = Integer.parseInt(st.nextToken());
		}
		if(n>m) {
			System.out.println("0");
		}
		else {
			for(int i=0; i<n-1; i++) {
				for(int j=i+1; j<n; j++) {
					ans = (ans * Math.abs(a[i]-a[j]))%m;
				}
			}
			System.out.println(ans);
		}
	}
}
```

