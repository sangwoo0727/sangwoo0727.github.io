---
title: "[SWEA1231] 중위순회"
categories:
  - Algorithm
tags:
  - Math
  - Algorithm
comments:
  - true
---
## [SWEA1231] 중위순회

[문제 링크(로그인이 필요합니다.)](https://www.swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV140YnqAIECFAYD)

### 문제 설명
* 이진트리를 중위순회하여 노드에 들어있는 데이터값을 중위순회 순서대로 표기하는 문제이다.
* 문제자체에서 루트노드는 번호가 1, 그리고 완전이진트리만 취급한다고 조건을 걸어서 사실 배열로 풀면 굉장히 쉬운 문제이다.
* 헤더파일에 의존하지 않기 위해 포인터와 연결리스트에 대한 연습 겸 트리를 리스트로 짜보았다.
* 트리를 리스트로 짜본 경험이 거의 없어 고생을 좀 하였고 내 풀이가 정확한지는 모르겠다. 

### 코드 리뷰
* 일단 노드를 구조체로 선언하여, num, data, left, right, next 변수를 이용하였는데, left, right 포인터 변수에 다른 노드를 연결하기 위하여 고생을 하다가 next 포인터 변수를 이용하였다. left, right는 트리의 왼쪽자식 노드 , 오른쪽 자식 노드를 연결하는 포인터 변수이고, next는 단순히 1번노드-> 2번노드 -> 3번노드로 연결해놓기 위함이다. 이 부분에 대해 쉽게 하는 방법이 있으신 분들은 피드백 해주시면 감사드리겠습니다!
* 저부분 말고는 문제자체는 쉬운문제여서 리뷰할 부분이 없는 것 같다.

```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

typedef struct Node {
	Node *left;
	Node *right;
	Node *next;
	int num;
	char data;
}node;
typedef struct{
	node * head;
	node * cur;
}tree;
int N;

void Inorder(node * bt) {
	if (bt == NULL) return;
	Inorder(bt->left);
	cout << bt->data;
	Inorder(bt->right);
}
int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	for (int t = 1; t <= 10; t++) {
		cin >> N;
		tree tl;
		tl.head = NULL;
		tl.cur = NULL;
		for (int i = 1; i <= N; i++) {
			node *newnode = (node*)malloc(sizeof(node));
			if (tl.head == NULL && tl.cur == NULL) {
				tl.head = newnode;
				tl.cur = newnode;
			}
			tl.cur->next = newnode;
			tl.cur = newnode;
		}
		tl.cur = tl.head;
		for (int i = 1; i <= N; i++) {
			int left, right;
			cin >> tl.cur->num;
			cin >> tl.cur->data;
			if (tl.cur->num * 2 < N) {
				cin >> left >> right;
				node * temp = tl.cur;
				for (int j = i; j < left; j++) {
					temp = temp->next;
				}
				tl.cur->left = temp;
				temp->num = left;
				temp = tl.cur;
				for (int j = i; j < right; j++) {
					temp = temp->next;
				}
				tl.cur->right = temp;
				temp->num = right;
			}
			else if (tl.cur->num * 2 == N) {
				cin >> left;
				node * temp = tl.cur;
				for (int j = i; j < left; j++) {
					temp = temp->next;
				}
				tl.cur->left = temp;
				temp->num = left;
				tl.cur->right = NULL;
			}
			else {
				tl.cur->left = NULL;
				tl.cur->right = NULL;
			}
			tl.cur = tl.cur->next;
		}
		tl.cur = tl.head;
		cout << '#' << t << ' ';
		Inorder(tl.cur);
		cout << '\n';
		tl.cur = tl.head;
		for (int i = 1; i < N; i++) {
			node * rpos = tl.cur->next;
			free(tl.cur);
			tl.cur = rpos;
		}
	}
}
```
![](/assets/img/Algorithm/1907111.png)
