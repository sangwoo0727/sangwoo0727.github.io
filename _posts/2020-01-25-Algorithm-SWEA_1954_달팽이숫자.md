---
title: "[SWEA1954] 달팽이 숫자"
categories:
  - Algorithm
tags:
  - Algorithm
read_time: false
comments:
  - true
toc: true
toc_sticky: true
---
[문제 링크(로그인이 필요합니다.)](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5PobmqAPoDFAUq&categoryId=AV5PobmqAPoDFAUq&categoryType=CODE)

## 문제 설명
배열을 달팽이 모양으로 순회하여 숫자를 넣는 문제이다.

정보처리기사나 기본 알고리즘 할 때 등장하는 문제인데 은근 짜려고하니 헷갈려서 풀이를 써놓으려 한다.

## 코드 리뷰
리뷰는 주석으로 하겠다.

```java
import java.util.*;
import java.io.*;

public class Solution {
	static int[][] arr;
	static int N;
	public static void main(String[] args) throws Exception {
        //System.setIn(new FileInputStream("res/sample_input.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		for(int t = 1; t<=T; t++) {
			N = Integer.parseInt(br.readLine());
			arr = new int[N][N];
			int dir = 1, len = N, num = 1;
			//dir은 1일때는 증가하는 쪽으로 더해주고 -1 일때는 감소하는 쪽으로 더해주는 변수
			//len은 점점 줄어들면서 달팽이가 한쪽으로 갈 수 있는 거리.
			//num은 배열에 숫자를 찍는 변수
			int i=0,j=-1; //코드의 흐름 상 j는 -1에서 시작한다.
			while(true) {
				for(int k=0; k<len; k++) { //증가하는 쪽으로 갈때,
					j += dir; //j가 점점 커짐
					arr[i][j]= num++;
				}
				len--; // 증가하는 쪽으로 가로 이동 후, len은 줄어들어야함
				if(len < 0) break;
				for(int k=0;k<len;k++) { //세로 이동
					i += dir;
					arr[i][j] = num++;
				}
				dir *= -1;
			}
			System.out.println("#"+t);
			for(int n=0;n<N;n++) {
				for(int m=0;m<N;m++) {
					System.out.print(arr[n][m]+" ");
				}
				System.out.println();
			}
		}
    }

}

```





