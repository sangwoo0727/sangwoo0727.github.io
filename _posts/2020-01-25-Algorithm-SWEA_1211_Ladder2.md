---
title: "[SWEA1211] Ladder2"
categories:
  - Algorithm
tags:
  - BruteForce
  - Algorithm
comments:
  - true
toc: true
toc_sticky: true
---
[문제 링크(로그인이 필요합니다.)](https://swexpertacademy.com/main/code/problem/problemSolver.do?contestProbId=AV14BgD6AEECFAYh)

## 문제 설명
100 * 100 배열이 0과 1로만 되어있고, 흔히 말하는 사다리 타기를 하는 배열이다.

1로만 다닐 수 있고, 시작부분에서 출발하여 도착 지점까지 사다리 타기를 할 때 도착 지점까지의 거리가 가장 짧은 출발지점을 구하는 문제이다.

## 코드 리뷰
자바언어로 알고리즘 문제풀이를 갈아타면서 코드 짤때 손이 마비가 된다..

딱히 리뷰 할 부분은 없는 것 같다. 사다리타기 원리 상, 하나의 시작 지점에서 도착 지점까지는 길이 한개밖에 안생기니깐, 시작 지점에서 탐색을 하여, 도착지점에 도착했을 때의 거리를 계산하면 될 듯.

ladder1을 풀어서 나는 그냥 도착지점에서 출발하게 구현하였다.

```java
import java.util.*;
import java.io.*;

class ans{
	int m, cnt;
	ans (int m,int cnt){
		this.m = m;
		this.cnt = cnt;
	}
}

public class Solution {
	static int[][] arr;
	static boolean[][] check;
	static int[] dc = {0,0,-1};
	static int[] dr = {1,-1,0};
	
	static void ckClear() {
		for(int i=0;i<100;i++) {
			for(int j=0;j<100;j++) {
				check[i][j]=false;
			}
		}
	}
	
	static ans go(int n,int m,int cnt) {
		if(n==0) {
			return new ans(m,cnt);
		}
		else {
			int nn=0,mn=0;
			for(int k=0;k<3;k++) {
				nn = n+dc[k];
				mn = m+dr[k];
				if(nn<0 || nn>=100 || mn <0 || mn >=100)
					continue;
				else if(check[nn][mn] || arr[nn][mn]==0)
					continue;
				check[nn][mn]=true;
				break;
			}
			return go(nn,mn,cnt+1);
		}
	}
	
    public static void main(String[] args) throws Exception {
    	int n = 0,m = 0, result = 0;
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	arr = new int[101][101];
    	check = new boolean[101][101];
    	for(int t=1; t<=10;t++) {
    		int tc = Integer.parseInt(br.readLine());
    		int Min = Integer.MAX_VALUE;
    		for(int i=0;i<100;i++) {
    			StringTokenizer st = new StringTokenizer(br.readLine());
    			for(int j=0;j<100;j++) {
    				arr[i][j] = Integer.parseInt(st.nextToken());
    			}
    		}
    		for(int j=0;j<100;j++) {
    			if(arr[99][j]==1) {
    				ckClear();
    				ans res = go(99,j,0);
    				if(Min >= res.cnt) {
    					Min = res.cnt;
    					result = res.m;
    				}
    			}
    		}
    		System.out.println("#"+tc+" "+ result);
    	}
    }
     
}
```

![](/assets/img/Algorithm/swea1211.png)



