---
title: "[자료구조]더미노드를 활용한 정렬기능이 추가된 연결리스트"
categories:
  - DataStructure
tags:
  - Stack
  - Array
  - DataStructure
comments:
  - true
---

## [자료구조] 배열 기반 스택의 구현

* 연결리스트가 아닌 배열 기반으로 스택을 구현하였다.

```cpp
#include <iostream>
#include <cstring>
#define STACK_LEN 100
using namespace std;

typedef struct {
	int stackArr[STACK_LEN];
	int topIndex;
}Stack;

void StackInit(Stack * pstack) { //스택 초기화
	pstack->topIndex = -1;
}
int SIsEmpty(Stack * pstack) {
	if (pstack->topIndex == -1) return 1;
	else return 0;
}
void SPush(Stack * pstack, int data) {
	pstack->topIndex += 1;
	pstack->stackArr[pstack->topIndex] = data;
}
int SPop(Stack * pstack) {
	int rIdx;
	if (SIsEmpty(pstack)) {
		printf("Stack Memory Error!");
		exit(-1);
	}
	rIdx = pstack->topIndex;
	pstack->topIndex -= 1;
	return pstack->stackArr[rIdx];
}
int SPeek(Stack * pstack) {
	if (SIsEmpty(pstack)) {
		printf("Stack Memory Error!");
		exit(-1);
	}
	return pstack->stackArr[pstack->topIndex];
}

int main() {
	Stack stack;
	StackInit(&stack);
	SPush(&stack, 1);
	SPush(&stack, 2);
	SPush(&stack, 3);
	SPush(&stack, 4);
	SPush(&stack, 5);
	while (!SIsEmpty(&stack)) printf("%d ", SPop(&stack));
	return 0;
}
```

---
### 출처 : 윤성우의 열혈 자료구조 (윤성우 지음, 오렌지미디어) 
