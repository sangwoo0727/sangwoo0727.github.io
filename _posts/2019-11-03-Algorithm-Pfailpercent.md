---
title: "[프로그래머스] 실패율"
categories:
  - Algorithm
read_time: false
tags:
- sort
comments:
  - true
---
## [프로그래머스] 실패율

[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/42889)

* 카카오 공채 코딩테스트 문제

### 문제 설명
* 스테이지의 개수와, 각 사용자가 현재 도착해있는 스테이지의 번호가 담긴 배열이 주어진다.
* 스테이지별로 실패율을 계산하여, 실패율이 높은 스테이지부터 낮은 스테이지 번호로 내림차순하여 출력하는 문제.
* 만약 스테이지의 실패율이 동일하다면, 번호가 작은 스테이지를 먼저 출력하면 된다.
* __실패율__ 이란, __해당 스테이지에 도달하였지만 아직 클리어하지 못한 플레이어수 /해당 스테이지를 성공하였든 실패하였든 해당 스테이지에 도달한 모든 플레이어 수__ 를 나타낸다.

### 코드 리뷰
* 총 스테이지의 개수가 500이하로 주어지기 때문에, __스테이지 번호를 인덱스 삼는 1차원 배열__ 을 하나 만들어, 사용자가 멈춰있는 스테이지 번호가 담긴 배열을 순회하며, 1차원 배열안에 값으로 몇명의 사용자가 그 스테이지에 있는지를 표시하였다.
* 이렇게 1차원 배열을 구해놓게 되면, 해당 __i번 인덱스에 있는 배열 값은 현재 i번 스테이지를 깨고 있는 사람__ 을 뜻하게 된다. 이 말은 다시 말하자면, __i번 스테이지에 도착하였지만 i번 스테이지를 못깨고 있는 사용자로, 실패율의 분자에 속하는 값__ 이 된다.
* 실패율을 구하기 위해 순회를 하되, __단 한번만의 배열 순회__ 로 실패율 모두를 구하였다.
* 처음 sum에는 모든 사용자의 수가 들어간다. 그 후 1번부터 N번까지를 순회하며, 1번 스테이지의 실패율은 board[1]/sum 이 된다. 즉, 모든 사용자는 1번 스테이지에 도달하였고, 그 중 board[1]값은 1번 스테이지에 도달하였으나 1번 스테이지를 아직 완료하지 못한 사용자의 수가 되므로, 이것은 1번 스테이지의 실패율이 된다.
* 이렇게 각 스테이지의 실패율을 구한 후, 다음 스테이지의 실패율을 구하기 위하여 __sum -= board[i]__ 를 통하여 다음 스테이지에 도달한 사용자 수로 sum을 update 시켜준다.
* 각각의 실패율을 구하며 pair,int를 쌍으로 갖고있는 배열에 넣어준 후, __문제에서 요구하였던 조건에 맞춰 sort__ 를 시켜준다.
* 그 후, answer 배열에 스테이지 번호만 넣어 출력해주는 식으로 구현하였다.

```cpp
#include <string>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;

bool comp(const pair <double, int>& a, const pair <double, int>& b) { 
    	//문제에서 요구한 조건에 맞춰 sort 구현
	if (a.first > b.first) return true;
	else if (a.first == b.first) return b.second > a.second;
	else return false;
}
vector<int> solution(int N, vector<int> stages) {
	int board[501];
	int sum = stages.size(); 
    	//모든 사용자의 수
	vector <pair <double, int>> fp; 
    	// double에는 실패율이, int에는 실패율에 해당하는 스테이지 번호가 들어간다.
	vector<int> answer;
	memset(board, 0, sizeof(board));
	for (int i = 0; i < sum; i++) {
		board[stages[i]]++; 
        	//스테이지 번호를 인덱스 삼고있는 배열에 해당 스테이지 번호를 깨고있는 사용자 수를 넣어준다.
	}
	for (int i = 1; i <= N; i++) {
		if (board[i] == 0) {
			fp.push_back({ 0,i });
			sum -= board[i];
		}
		else {
			double fail_per = (double)board[i] / sum; 
            	//실패율 계산
			fp.push_back({ fail_per,i });
			sum -= board[i]; 
            	// 다음 스테이지에 도달한 사용자 수 sum을 업데이트
		}
	}
	sort(fp.begin(), fp.end(), comp);
	for (int i = 0; i < fp.size(); i++) {
		answer.push_back(fp[i].second);
	}
	return answer;
}
```
