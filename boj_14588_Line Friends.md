## BOJ 14588번 "Line Friends (Small)"
(https://www.acmicpc.net/problem/14588)

<br></br>

### 풀이 방법

1. 입력받을 때 left, right를 pair로 "edge" vector에 넣는다.

2. 입력받은 후 1부터 N까지 for문을 돌면서 겹치지 않는 것을 "line" vector에 넣었다.

3. bfs를 돌며 친구 관계를 계산한다.


----------


<br></br>

### 사용 알고리즘: BFS



---------

### 코드
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;

int n;
pair<int, int> edge[400];
vector<int> line[400];
bool visit[400];

int bfs(int sp, int ep)
{
	fill(visit, visit + n + 1, false);
	queue<pair<int, int>> q;
	q.push(make_pair(0, sp));

	while (!q.empty()) {
		int w = q.front().first;
		int curNode = q.front().second;
		q.pop();

		if (curNode == ep) return w;

		for (int i = 0; i < line[curNode].size(); i++) {
			int nextNode = line[curNode][i];
			if (visit[nextNode]) continue;
			q.push(make_pair(w + 1, nextNode));
			visit[nextNode] = true;
		}
	}
	return -1;
}

int main()
{
	ios::sync_with_stdio(false); cin.tie(0);
	int l, r, q;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> l >> r;
		edge[i].first = l;
		edge[i].second = r;
	}

	for (int i = 1; i <= n; i++) {
		l = edge[i].first;
		r = edge[i].second;
		for (int j = 1; j <= n; j++) {
			if (i == j) continue;
			int nl = edge[j].first;
			int nr = edge[j].second;
			if (nr < l || r < nr) continue;
			line[i].push_back(j);
			line[j].push_back(i);
		}
	}

	cin >> q;
	for (int i = 0; i < q; i++) {
		cin >> l >> r;
		cout << bfs(l, r) << '\n';
	}

	return 0;
}
```

