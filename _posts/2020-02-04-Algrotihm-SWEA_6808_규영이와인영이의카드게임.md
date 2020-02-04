---
title: "[SWEA6808] 규영이와 인영이의 카드게임"
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
[문제 링크(로그인이 필요합니다.)](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWgv9va6HnkDFAW0&categoryId=AWgv9va6HnkDFAW0&categoryType=CODE)

## 문제 설명
규영이와 인영이가 1~18까지의 수가 적힌 18장의 카드를 9장씩 나눠가진다.

한 라운드에 한 장씩 카드를 낸 다음 두 사람이 낸 카드에 적힌 수를 비교해서 점수를 계산한다.

높은 수가 적힌 카드를 낸 사람은 두 카드에 적힌 수의 합만큼의 점수를 얻고,

낮은 수가 적힌 카드를 낸 사람은 아무런 점수도 못받는다.

9라운드까지 끝났을 때, 총점이 높은 사람이 이기게 된다. 총점이 같을 경우는 무승부이다.

규영이가 내는 카드와 내는 순서를 알고있다고 할때, 규영이가 이기는 경우와 지는 경우의 가지수를 구하는 문제이다.

## 코드 리뷰
많아봤자 9!가지를 완전탐색하면 되는 문제이므로 제한시간안에는 충분히 들어온다.

9! 가지를 완탐하는 방법은 순열의 가지수이므로 순열 코드에 맞춰 작성을 하였다.

풀고나서 카페에 가는 길에 가지치기 하는 방법이 생각나서 가지치기 방법으로 풀었고, 시간이 1초이상 줄어들었다.

먼저 모든 가지수를 보는 코드를 봐보자. 자세한 설명은 주석 참고.

```java
import java.io.*;
import java.util.*;

public class Solution {
	static boolean[] numeric = new boolean[19];
	static boolean[] ck;
	static int[] bd1 = new int[9];
	static int[] bd2 = new int[9];
	static int ans,cnt,mucnt;
	static void permu(int r,int sum1, int sum2) {
		if(r==9) { //9라운드가 지나고, 값 비교를 통해 
			cnt++; //모든 가지수 
			if(sum1 > sum2) ans++; //이긴 가지수
			if(sum1 == sum2) mucnt++; //비긴 가지수 증가
			return;
		}
		for(int i=0; i<9; i++) { // 한 라운드당 9개의 숫자 중, 이전 라운드에서 선택이 안된 숫자를 선택.
			if(!ck[i]) {
				ck[i]=true;
				if(bd1[r]>bd2[i]) {
					permu(r+1,sum1+bd1[r]+bd2[i],sum2); //bd1의 숫자가 클 경우, sum1에 값을 더해준다.
				}else {
					permu(r+1,sum1,sum2+bd1[r]+bd2[i]); //bd2의 숫자가 클 경우, sum2에 값을 더해준다.
				}
				ck[i]=false;
			}
		}
	}
	public static void main(String[] args) throws Exception {
		StringBuilder sb = new StringBuilder();
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = null;
		int T = Integer.parseInt(br.readLine());
		for(int t=1; t<=T; t++) {
			ans = cnt = mucnt = 0;
			ck = new boolean[9];
			numeric = new boolean[19];
			st = new StringTokenizer(br.readLine());
			for(int i=0; i<9; i++) {
				bd1[i]=Integer.parseInt(st.nextToken());
				numeric[bd1[i]]=true; //규영이가 선택한 카드는 true로 체크
			}
			int tmp=0;
			for(int i=1; i<=18; i++) {
				if(!numeric[i]) //카드가 체크가 안되어있는 경우, bd2에 카드 숫자 넣는다.
					bd2[tmp++]=i;
			}
			permu(0,0,0);
			sb.append("#"+t+" "+ans+" "+(cnt-ans-mucnt)+"\n"); //ans는 이긴가지수,
            //cnt-ans-mucnt 는 전체가지수에서 이긴가지수와 비긴가지수 뺀 가지수.
		}
		System.out.println(sb.toString());
	}	
}
```

이 경우는 정말 말그대로 모든 가지수를 다 본 경우이다.

하지만 걸어가면서 생각난건데, 모든 라운드를 다 이겼다고 가정하면 총점은 171점이 된다. 

즉, 중간라운드에서 171/2를 넘어가는 점수를 이미 얻었으면, 그 뒤에는 지던 이기던 상관이 없이 모든 경우의 수를 다 171/2 이상을 얻은 사람이 가져갈 수 있는 가지수가 된다.

이런식으로 가지를 쳐나간 풀이를 아래에 첨부한다.

```java
import java.io.*;
import java.util.*;

public class Solution {
	static boolean[] numeric = new boolean[19];
	static boolean[] ck;
	static int[] bd1 = new int[9];
	static int[] bd2 = new int[9];
	static int win,lose;
	static void permu(int r,int sum1, int sum2) {
		if(sum1 > (171/2) || sum2 > (171/2)) { //둘 중 하나라도 기준점을 넘어가면,
			int oth = 1;
			for(int tmp = 9-r; tmp>0; tmp--) {
				oth *=tmp; //나머지 라운드에 대해 팩토리얼 계산만 한다.
			}
            //이기고 있는 쪽에 oth 를 더해준다.
			if(sum1 > sum2) win+= oth; 
			else lose+= oth;
			return;
		}
		for(int i=0; i<9; i++) {
			if(!ck[i]) {
				ck[i]=true;
				if(bd1[r]>bd2[i]) {
					permu(r+1,sum1+bd1[r]+bd2[i],sum2);
				}else {
					permu(r+1,sum1,sum2+bd1[r]+bd2[i]);
				}
				ck[i]=false;
			}
		}
	}
	public static void main(String[] args) throws Exception {
		StringBuilder sb = new StringBuilder();
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = null;
		int T = Integer.parseInt(br.readLine());
		for(int t=1; t<=T; t++) {
			win = lose = 0;
			ck = new boolean[9];
			numeric = new boolean[19];
			st = new StringTokenizer(br.readLine());
			for(int i=0; i<9; i++) {
				bd1[i]=Integer.parseInt(st.nextToken());
				numeric[bd1[i]]=true;
			}
			int tmp=0;
			for(int i=1; i<=18; i++) {
				if(!numeric[i])
					bd2[tmp++]=i;
			}
			permu(0,0,0);
			sb.append("#"+t+" "+win+" "+lose+"\n");
		}
		System.out.println(sb.toString());
	}	
}
```

![](/assets/img/Algorithm/20200204_1.png)


두 경우의 실행시간이 엄청 차이나는 것을 알 수 있다.


![](/assets/img/Algorithm/20200204_2.png)

