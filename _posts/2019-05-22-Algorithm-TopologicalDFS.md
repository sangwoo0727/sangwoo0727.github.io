---
title: "[알고리즘설계]DFS를 이용한 위상정렬"
categories:
  - Algorithm
tags:
  - BFS
  - TopologicalSorting
  - Algorithm
comments:
  - true
---

## [알고리즘설계] DFS를 이용한 위상정렬

### 코드 리뷰

* 접점과 간선의 정보를 통해 방향그래프를 만든다.

* 위상정렬을 하려면 방향그래프에 사이클이 없어야 한다.

* 한번도 방문하지 않은 정점은 white , 정점에 인접한 정점들을 모두 방문하고 return이 되어 돌아올 경우 black으로, 그리고 아직 인접한 정점들에 대해 dfs가 진행중일 경우 gray로 visit 배열을 채운다.

* 만약 v의 인접한 정점 w를 방문하였는데, 그 w의 색상이 white일 경우 아직 한번도 방문하지 않은 정점이므로 gray로 바꾼다.

* v의 인접한 정점 w를 방문하였는데, 그 w의 색상이 gray일 경우, w에서 dfs를 진행하여 온 v가 다시 w를 바라보게 되는 상황이므로 사이클이 존재하는 방향그래프가 되어 위상정렬을 할수 없게 된다. (gray를 바라본 것과 백에지가 그 정점으로 향한 것은 동치)

* v의 인접한 정점 w를 방문하려는데, 그 w의 색상이 black일 경우, 이미 w는 dfs를 진행하여 후손노드들을 모두 방문하고 return이 된 상황이라 재탐색할 필요가 없으므로, continue 시킨다.

* 이렇게 모든 정점에 대해 dfs를 진행하고, return이 될 때마 배열에 추가한다음 배열을 역순으로 출력하면, 위상정렬이 완료된다.

```cpp
#include <iostream>
#include <vector>
using namespace std;

char color[100];
int time;
int discover_time[100];
int finish_time[100];
int V, E;
int ans[100];
int idx;
bool cycle_exist=false;
vector <vector <int>> adj(100);

void DFS(int v) {
	color[v] = 'g'; //gray
	discover_time[v] = ++time;
	int size = adj[v].size();
	for (int i = 0; i < size; i++) {
		int w = adj[v][i];
		if (color[w] == 'w') {//white 만날때
			color[w] = 'g';
			DFS(w);
		}
		if (color[w] == 'g') {//gray 만날때
			cycle_exist = true;
			return;
		}
		if (color[w] == 'b') {//black
			continue;
		}
	}
	finish_time[v] = ++time;
	color[v] = 'b';
	ans[idx++] = v;
}
int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin >> V >> E;
	int a, b;
	for (int i = 0; i < E; i++) {
		cin >> a >> b;
		adj[a].push_back(b); //a->b로
	}
	for (int i = 1; i <= V; i++) {
		color[i] = 'w';//white
	}
	for (int i = 1; i <= V; i++) {
		if (color[i] == 'w') {
			DFS(i);
		}
	}
	if (cycle_exist) cout << "위상정렬 실패(사이클 존재)\n";
	else {
		for (int i = V - 1; i >= 0; i--) cout << ans[i] << " ";
		cout << "\n";
	}
	return 0;
}

```
![](/assets/img/Algorithm/0522.png)
