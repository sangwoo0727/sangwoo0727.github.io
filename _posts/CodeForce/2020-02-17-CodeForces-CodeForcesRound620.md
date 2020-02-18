---
title: "[CodeForces] CodeForces Round #620 (div2) 풀이 (A~D)"
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
코드포스 해야지 해야지 하면서 저번 주말, 처음으로 콘테스트 620라운드에 참가를 해보았다. 

일단 굉장히 나에게 도움이 될만한 시험이라는 것은 느낄 수 있었다.

온라인 기업 코테를 보는 느낌처럼 문제들을 빠르게 풀어야한다는 압박감 + 무슨 유형인지 내가 스스로 파악해야하는 어려움을 극복하는 것이 나에겐 가장 필요했다.

주기적으로 열리기때문에 주말에 열리는 시험들은 최대한 앞으로도 참석해보려 한다.

![](/assets/img/CodeForce/20200217_1.png)

첫 코드포스 콘테스트의 나의 레이팅이다.

4000등정도 하였고, 문제는 3문제를 풀었다.

일단 당분간은 문제를 풀건 못풀건 A~D번까지는 지속적으로 풀이를 할 예정이고, 목표 또한 4문제를 푸는걸 목표로 할 것이다.

그럼 문제 풀이를 해보겠다.

## A. Two Rabbits

![](/assets/img/CodeForce/20200217_2.png)

키큰 토끼는 x에 위치해서 1초마다 양의 방향으로 a만큼 이동하고, 키 작은 토끼는 y위치에서 1초마다 음의 방향으로 b만큼 이동한다.

이 둘이 동시에 같은 곳에 있는 순간이 있다면 몇 초 후인지를 구하는 문제이다.

만약 그런 순간이 없다면 -1을 출력하는 문제.

대회라는 생각과 빨리 풀어야한다는 긴장감에 문제를 보자마자 while문에 넣어서 1초씩 움직이며 확인하는 미친짓을 저질렀다..

제출하고 time limit이 뜨고 정신이 들어서 바로 풀었다.

y는 항상 x보다 크기때문에 (y-x)%(a+b) 가 0인지를 판단해서 0이라면 몫을 출력하고 아니면 -1을 출력하면 O(1)에 풀 수 있는 문제.

```java
import java.io.*;
import java.util.*;
 
public class A_TwoRabbits {
	public static void main(String[] args) throws Exception {
		StringBuilder sb = new StringBuilder();
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int tc = Integer.parseInt(br.readLine());
		while(tc-->0) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int x = Integer.parseInt(st.nextToken());
			int y = Integer.parseInt(st.nextToken());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			sb.append((y-x)%(a+b) == 0 ? (y-x)/(a+b) : -1).append("\n");
		}
		System.out.println(sb);
	}
}
```

## B. Longest Palindrome

![](/assets/img/CodeForce/20200217_2.png)

문자열 길이가 m으로 같은 문자열들이 n개 주어진다.

이때, 이 문자열들을 조합해서 가장 긴 팰린드롬(pop, noon 등과 같이 앞으로 시작하나 뒤로 시작하나 똑같은 문자열)을 만드는 문제였다.

팰린드롬에 관련해서는 문자열 처리 문제에서 많이 나오는 문제라 B번 문제 자체는 어렵지는 않았다.

다만, 자바를 시작한지 얼마 안돼서 구현에 있어서 다소 어색해서 시간이 오래 걸렸던 것 같다.

그래서 시험이 끝나고 다시 좀 정리한 코드와 풀이법으로 첨부한다.

문자열을 n개 입력받을 때, 자기자신이 바로 팰린드롬이 되는 문자열이 있다면 하나만 따로 빼놓는다. 중복되는 문자열은 주어지지 않고, 이 문제에선 주어진 문자열들을 자르거나 쪼개지않고 그냥 이어붙여야 하는 문제기 때문에 자기자신이 팰린드롬인 문자열은 하나만 추가할 수 있다.

그 후 나머지 문자열들은 Hash 테이블에 넣어둔다.

하나씩 문자열의 팰린드롬이 존재하면, 답을 출력할 문자열의 길이/2 에 s와 rev를 이어서 붙혀준다.

s와 rev는 각각 입력받은 문자열과 그 문자열의 역순 문자열이다.

```java
import java.io.*;
import java.util.*;
 
public class B_LongestPalindrome {
	static int N,M;
	static HashMap<String,Integer> map = new HashMap<>();
	public static void main(String[] args) throws Exception {
		StringBuilder sb = new StringBuilder();
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		N = Integer.parseInt(st.nextToken()); M = Integer.parseInt(st.nextToken());

		String OwnPalind = null;
		boolean flg = false;
		for(int n=0; n<N; n++) {
			String s = br.readLine();
			String rev = reverseString(s);
			if(s.equals(rev) && !flg) {
				OwnPalind = s;
				flg = true;
			}else {
				if(s.equals(rev)) continue;
				map.put(s, 0);
			}
		}
		Iterator<String> it = map.keySet().iterator();
		while(it.hasNext()) {
			String key = it.next();
			String rev = reverseString(key);
			if(map.get(key)==0) {
				if(map.containsKey(rev)){
					sb.insert(sb.length()/2, key+rev);
					map.put(key, 1);
					map.put(reverseString(key), 1);
				}
			}
		}
		if(flg) {
			sb.insert(sb.length()/2, OwnPalind);
		}
		System.out.println(sb.length());
		System.out.println(sb);
	}
	public static String reverseString(String s) {
		return (new StringBuilder(s)).reverse().toString();
	}
}
```

## C. Air Conditioner