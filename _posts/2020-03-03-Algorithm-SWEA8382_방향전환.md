---
title: "[SWEA8382] 방향전환"
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
[문제 링크(로그인이 필요합니다.)](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWyNQrCahHcDFAVP&categoryId=AWyNQrCahHcDFAVP&categoryType=CODE)

## 문제 설명
두 좌표가 주어졌을 때, 한 좌표에서 다른 좌표까지의 이동 횟수를 구하는 문제이다.

단, 가로이동을 한번하였으면, 그 다음엔 세로이동을 해야하는 조건이 붙는다.

## 코드 리뷰
일단, 두 좌표의 x길이와 y길이의 차이가 0이면, x길이+y길이만큼만 이동하면 한 좌표에서 다른 좌표로 도달할 수 있다.

2이상 차이가 날 경우, 짧은 길이로 갈 수 있는 만큼 간 후(정사각형 모양) 남은 길이에 대한 규칙을 찾아서 해결하였다.

남은 길이가 홀수일 경우, 남은길이 * 2 -1 만큼만 이동하면 도달이 가능하고,

남은 길이가 짝수일 경우, 남은길이 * 2 만큼만 이동하면 도달이 가능하다.

코드포스를 하면서 수학적 사고력이 조금씩 올라가는 느낌이라 기분이 좋다.

```java
import java.io.*;
import java.util.*;

public class Solution {
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static StringBuilder sb = new StringBuilder();
	static StringTokenizer st;
	static int n,m,nn,nm;
	
	public static void main(String[] args) throws Exception {
		int tc = Integer.parseInt(br.readLine());
		for(int t=1; t<=tc; t++) {
			sb.append("#").append(t).append(" ");
			st = new StringTokenizer(br.readLine());
			n = Integer.parseInt(st.nextToken());
			m = Integer.parseInt(st.nextToken());
			nn = Integer.parseInt(st.nextToken());
			nm = Integer.parseInt(st.nextToken());
			int ans = 0;
			int xlen = Math.abs(nn-n);
			int ylen = Math.abs(nm-m);
			if(xlen<ylen) {
				int tmp = xlen; xlen = ylen; ylen = tmp; 
			}
			ans += 2*ylen;
			if(((xlen-ylen)&1)==0) ans += (xlen-ylen)*2;
			else ans += (xlen-ylen)*2-1;
			sb.append(ans).append("\n");
		}
		System.out.println(sb);
	}
}
```

![](/assets/img/Algorithm/20200303_1.png)
