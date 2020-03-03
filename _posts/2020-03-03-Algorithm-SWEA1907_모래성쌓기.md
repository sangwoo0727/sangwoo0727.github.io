---
title: "[SWEA1907] 모래성 쌓기"
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
[문제 링크(로그인이 필요합니다.)](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5PNx_KACIDFAUq&categoryId=AV5PNx_KACIDFAUq&categoryType=CODE)

## 문제 설명
2차원 배열이 주어지는데, 모래성이 없는 곳은 '.'으로 모래성이 있는 칸은 그 칸의 튼튼함을 나타내는 1~9 숫자로 표현된다.

모래성 각 칸은 파도가 칠 때마다 칸 주변 8방향에 '.'의 개수가 각칸의 숫자보다 크거나 같으면 모래성이 무너진다.

총 몇번의 파도가 치고나면 모래성이 더 이상 변화가 없는지 구하는 문제이다.

## 코드 리뷰
너무 당연하게도 모래성을 기준으로 8방향을 확인하며 무너지지않으면 다음 큐에 넣는 식으로 진행을 하였는데 시간초과가 났다.

시간복잡도를 대략적으로 계산해보니깐 시간초과가 충분히 날 수 있는 상황인데 왜 그냥 들어갔는지..

아무튼 조금 더 고민하다가 모래성이 아닌 '.'을 기준으로 8방향을 보며, 모래성이 있는 칸의 값을 -1 시켜주며, 만약 0이 되면 그 배열 칸을 '.'으로 바꿔주고 그 칸을 다시 큐에 넣어 확인하는 식으로 해결하였다.

이렇게 되면 큐에 들어가는 개수가 중복되지 않게 풀 수 있어 시간을 엄청 줄일 수 있다.

```java

import java.io.*;
import java.util.*;

public class SWEA1907_모래성쌓기 {
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static StringTokenizer st;
	static StringBuilder sb = new StringBuilder();
	static int H,W;
	static char[][] bd;
	static int ans = 0;
	static int dc[] = {0,-1,-1,-1,0,1,1,1};
	static int dr[] = {-1,-1,0,1,1,1,0,-1};
	
	public static void main(String[] args) throws Exception{
		int tc = Integer.parseInt(br.readLine());
		for(int t=1; t<=tc; t++) {
			sb.append("#").append(t).append(" ");
			st = new StringTokenizer(br.readLine());
			H = Integer.parseInt(st.nextToken());
			W = Integer.parseInt(st.nextToken());
			ans = 0;
			bd = new char[H][W];
			for(int h=0; h<H; h++) {
				bd[h] = br.readLine().toCharArray();
			}
			bfs();
			sb.append(ans).append("\n");
		}
		System.out.println(sb);
	}
	static void bfs() {
		Queue<Node> q = new LinkedList<>();
		for(int h=0; h<H; h++) {
			for(int w=0; w<W; w++) {
				if(bd[h][w]=='.') {
					q.offer(new Node(h,w));
				}
			}
		}
		int lev = -1;
		while(!q.isEmpty()) {
			lev++;
			int size = q.size();
			for(int s=0; s<size; s++) {
				int n = q.peek().h;
				int m = q.poll().w;
				for(int k=0; k<8; k++) {
					int nn = n+dc[k];
					int nm = m+dr[k];
					if(inner(nn,nm)) {
						bd[nn][nm] -= 1;	
						if(bd[nn][nm]=='0') {
							q.offer(new Node(nn,nm));
						}
					}
				}
			}	
		}
		if(lev == -1) lev = 0;
		ans = lev;
	}
	static boolean inner(int n, int m) {
		return n>=0 && n<H && m>=0 && m<W && bd[n][m]!='.';
	}
	static class Node {
		int h,w;
		public Node(int h, int w) {
			this.h = h; this.w = w;
		}
	}
}
```

![](/assets/img/Algorithm/20200303_2.png)
