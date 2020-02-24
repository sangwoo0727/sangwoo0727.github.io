---
title: "[CodeForces] CodeForces Round #623 (div2) 풀이 (A~C)"
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
며칠만에 코드포스 컨테스트에 참가할 시간이 되어서 포부를 가지고 참가했다가 탈탈 털렸다..

B에서 작은 실수로 디버깅하다가 시간을 다쏟았다.

오늘 밤에 Div3가 열려서 참가해서 자신감을 회복할 예정이다.

바로 풀이 겸 리팩토링 들어가자!

## A. Dead Pixel

![](/assets/img/CodeForce/20200224_1.png)

직사각형의 가로,세로 길이가 주어지고, 죽은 픽셀의 좌표가 주어진다. 이때, 죽은 픽셀을 포함하지 않는 영역의 크기 중 최대를 찾는 문제이다.

매우 간단하다. 죽은 픽셀 좌표 기준으로 상하좌우 직사각형의 넓이 중 최대를 출력하면 되는 문제.

```java
import java.util.*;
import java.io.*;
 
public class Main {
	static int m, n, M, N;
	public static void main(String[] args) throws Exception {
		StringBuilder sb = new StringBuilder();
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = null;
		int tc = Integer.parseInt(br.readLine());
		while(tc-- >0) {
			st = new StringTokenizer(br.readLine());
			M = Integer.parseInt(st.nextToken());
			N = Integer.parseInt(st.nextToken());
			m = Integer.parseInt(st.nextToken());
			n = Integer.parseInt(st.nextToken());
			int k = N*m;
			k = Math.max(k, N*(M-m-1));
			k = Math.max(k, M*n);
			k = Math.max(k, M*(N-n-1));
			sb.append(k).append("\n");
		}
		System.out.println(sb);
	}
}
```

## B. Homecoming

![](/assets/img/CodeForce/20200224_2.png)

문자열 문제는 항상 헷갈리고, 시간에 쫓기면서 풀 때 항상 정확하게 구조를 짜고 들어가지 않고, 대강 들어가면서 디버깅에 시간을 많이 쏟게되는 것 같다.

기업 코테에서도 그렇고, 문자열 문제는 항상 쉬운 듯 하면서 끝까지 내 발목을 잡을 것 같다.

시간에 쫓기더라도 정확하게 설계를 하고 들어가는 연습을 해야할 것 같다.

Petya라는 아이가 파티가 끝나고 집에가려한다. 파티장소에서 집까지는 A와 B로 이루어진 문자열로 표현되는데 A와 B는 각각 a,b의 요금을 가진 버스와 tram? 의 노선이다.

AAABBAAA 일 경우, a요금을 내고 i=1~4까지 갈 수 있다. 그리고 b요금을 내고 i=4~6까지 갈 수 있으며, 다시 a요금을 내고 6~끝까지 갈 수 있다.

문자열이 주어지고, a,b,p 즉, a요금과 b요금, 그리고 Petya가 가지고 있는 돈이 주어질 때, Petya가 걸어가야하는 거리의 최소를 구하는 문제이다.

단, 대중교통을 타고난 후 부터는 집에 도착할 때까지 계속해서 대중교통만 타야한다.

결국, 이 문제에서는 Petya가 대중교통을 타야지 마음먹고나서부터는 무조건 집에 도착할 때까지 대중교통을 타야하므로, 뒤에서부터 가진 돈 p를 쓰면서 문자열을 먹어나가면 되는 문제였다.

어떤 대중교통을 선택해야지 최소로 걷는 양일까 라는 문제가 아니라 간단한 문제였던 것 같다. 

그냥 뒤에서부터 문자에 해당하게 a,b만큼 요금 빼주면서 p요금을 최대한 쓰고, 더 이상 못 쓸 때의 인덱스를 반환해주면 되는 문제였다.

같이 시험 본 분 이야기 들어보니깐, 앞에서부터 풀면 반례가 생기는 것 같더라.

```java
import java.util.*;
import java.io.*;

public class Main {
	static int a,b,p;
    static StringBuilder sb = new StringBuilder();
    static BufferedReader br = null;
    static StringTokenizer st = null;
	public static void main(String[] args) throws Exception {
		br = new BufferedReader(new InputStreamReader(System.in));
		st = null;
		int tc = Integer.parseInt(br.readLine());
		while(tc-- >0) {
			st = new StringTokenizer(br.readLine());
			a = Integer.parseInt(st.nextToken());
			b = Integer.parseInt(st.nextToken());
			p = Integer.parseInt(st.nextToken());
			String s = br.readLine();
			int n = s.length()-1;
			int pidx = n;
			for(int i=n-1; i>=0; i--) {
                if(i==0){
                    if(s.charAt(0)=='A' && p-a>=0) {
					    pidx = 0;
				    }else if(s.charAt(0)=='B' && p-b>=0){
                        pidx = 0;
				    }
                }
				else if(s.charAt(i)!=s.charAt(i-1)) {
					if(s.charAt(i)=='B') {
						if(p-b>=0) {
							p-=b;
							pidx = i;
						}else break;
					}else {
						if(p-a>=0) {
							p-=a;
							pidx = i;
						}else break;
					}
				}
			}
			sb.append(pidx+1).append("\n");
		}
		System.out.println(sb);
	}
}
```

## C. Restoring Permutation

![](/assets/img/CodeForce/20200224_3.png)

c번치고 굉장히 간단한 문제였다.

n이 주어지고, n개만큼의 b1,b2,.....,bn이 주어진다. 이 때, bi = min(a2i-1, a2i),  1<= bi <= 2n , 1<= ai <= 2n 을 만족하는 ai로 이루어진 숫자열의 나열 중 가장 순열의 순서 중 작은 순열의 순서를 구하는 문제이다.

음.. 예를 들어, 1,2,3,4의 숫자열과 1,2,4,3의 숫자열이 있을 때 순열 순서가 작은 1,2,3,4를 출력해주면 된다.

2n크기의 체크배열을 만들어서, bi를 먼저 다 체크해주고, 앞에서부터 a2i-1과 a2i에 bi와, bi보다 큰 숫자를 순서대로 보면서 체크배열에 표시가 안된 애들이 발견되면 바로 a2i에 넣어주는 식으로 진행하였다. 그 후, 그 숫자에 대한 체크배열을 true로 체크해준다.

2n조건을 보면서 조건을 벗어나면 -1을 출력하는 예외만 넣어주면 반례없이 풀 수 있다.

또한 n이 100이고, 테스트케이스 수 역시 100으로 작아서 시간초과에 대한 걱정은 없다.

```java
import java.util.*;
import java.io.*;

public class Main {
    static StringBuilder sb = new StringBuilder();
    static BufferedReader br = null;
    static StringTokenizer st = null;
    static int n;
    static int[] ba;
    static boolean[] ck;
    
	public static void main(String[] args) throws Exception {
		br = new BufferedReader(new InputStreamReader(System.in));
		st = null;
		int tc = Integer.parseInt(br.readLine());
		while(tc-- >0) {
			n = Integer.parseInt(br.readLine());
			st = new StringTokenizer(br.readLine());
			ba = new int[n+1];
			ck = new boolean[2*n+2];
			boolean flg = false;
			for(int i=1; i<=n; i++) {
				ba[i] = Integer.parseInt(st.nextToken());
				if(ba[i]>=2 * n) {
					flg = true;
				}else {
					ck[ba[i]]=true;
				}
			}
			StringBuilder ans = new StringBuilder();
			for(int i=1; i<=n; i++) {
				ans.append(ba[i]).append(" ");
				int tmp = ++ba[i];
				while(ck[tmp] && tmp<=2*n) {
					tmp++;
				}
				if(tmp<=2*n) {
					ans.append(tmp).append(" ");
					ck[tmp]=true;	
				}else {
					flg = true;
					break;
				}
			}
			if(flg) sb.append("-1").append("\n");
			else sb.append(ans).append("\n");
		}
		System.out.println(sb);
	}
}
```



