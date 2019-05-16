---
title: "BFS를 이용한 위상정렬"
categories:
  - Algorithm
tags:
  - BFS
  - TopologicalSorting
  - Algorithm
comments:
  - true
---

## BFS를 이용한 위상정렬

### 코드 리뷰

* 접점과 간선의 정보를 통해 방향그래프를 만든다.

* 위상정렬을 하려면 방향그래프에 사이클이 없어야 한다.

* 또한 방향그래프에 사이클이 없으면 위상정렬을 할 수 있으므로, 둘은 동치관계가 된다.

* 사이클이 없는 그래프에는 indegree(v)가 0인 vertex v가 항상 존재한다.

* 즉 모든 vertex에 대해 indegree 개수를 저장한다.

* indegree가 0인 vertex 들을 queue에 집어넣는다.

* queue의 맨앞에 있는 접점에 대하여 bfs 탐색을 시작하는데, 기준 접점에서 인접한 다음접점으로 이어지는 간선을 뺀 나머지 indegree개수가 0인 경우, queue에 집어넣는다.

* queue에서 pop되는 순서대로 출력하면 위상정렬이 완성된다.

* 사이클이 있는 방향그래프에선 bfs를 진행하다보면, indegree가 0인 접점이 더이상 존재하지 않는 경우가 생기므로, 그런 경우 위상정렬 불가능.

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int V, E;
int check[500];
int indegree[500];
int ans[500];
vector <vector <int>> adj(500);
queue <int> q;

void bfs() {
	int idx = 0;
	while (!q.empty()) {
		int now = q.front();
		check[now] = 1;
		ans[idx++] = now;
		q.pop();
		int size = adj[now].size();
		for (int i = 0; i < size; i++) {
			int next = adj[now][i];
			indegree[next]--;
			if (indegree[next] == 0) q.push(next);
		}
	}
}

int main() {
	int a, b;
	printf("접점의 개수: ");
	scanf("%d", &V);
	printf("간선의 개수: ");
	scanf("%d", &E);
	for (int i = 0; i < E; i++) {
		scanf("%d %d", &a, &b); //a -> b로 이어지게 방향그래프 만듬
		adj[a].push_back(b);
		indegree[b]++;
	}
	for (int i = 1; i <= V; i++) {
		if (indegree[i] == 0) {
			q.push(i);
		}
	}
	bfs();
	if (ans[V - 1] == 0) {
		printf("사이클이 있는 방향그래프\n");
		return 0;
	}
	for (int i = 0; i < V; i++) {
		printf("%d ", ans[i]);
	}
	return 0;
}
```
