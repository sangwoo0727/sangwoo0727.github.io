---
title: "[자료구조] 연결리스트 기반 스택의 구현"
categories:
  - DataStructure
tags:
  - Stack
  - Linked List
  - DataStructure
comments:
  - true
toc: true
toc_sticky: true
---

## [자료구조] 연결리스트 기반 스택의 구현

* 연결리스트로 스택을 구현하였다.
* 포인터변수는 노드를 가리킬 수 있는 head 하나만 선언한다.
* 연결리스트와 똑같은데, 삽입의 역순으로 노드를 참조할 수 있게 구현하면 된다.


```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

typedef struct _node {
	int data;
	struct _node *next;
}Node;

typedef struct{
	Node *head;
}stack;

void  StackInit(stack * pstack) {
	pstack->head = NULL;
}

int SIsEmpty(stack *pstack) {
	if (pstack->head == NULL) return 1;
	else return 0;
}

void SPush(stack *pstack, int data) {
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;
	newNode->next = pstack->head;
	pstack->head = newNode;
}

int SPop(stack *pstack) {
	int rdata;
	Node * rnode;
	if (SIsEmpty(pstack)) {
		cout << "Stack Memory Error!" << endl;
		exit(-1);
	}
	rdata = pstack->head->data;
	rnode = pstack->head;
	pstack->head = pstack->head->next;
	free(rnode);
	return rdata;
}

int SPeek(stack *pstack) {
	if (SIsEmpty(pstack)) {
		cout << "Stack Memory Error" << endl;
		exit(-1);
	}
	return pstack->head->data;
}
int main() {
	stack list;
	StackInit(&list);
	SPush(&list, 1);
	SPush(&list, 2);
	SPush(&list, 3);
	SPush(&list, 4);
	SPush(&list, 5);
	while (!SIsEmpty(&list)) cout << SPop(&list) << endl;
	return 0;
}
```

---
### 출처 : 윤성우의 열혈 자료구조 (윤성우 지음, 오렌지미디어) 
