## Codeground "김씨만 행복한 세상"
(https://www.codeground.org/practice/practiceProbView.do#)

----------

### 풀이 방법

이분그래프처럼 풀면 된다.

색깔 두 개(빨간색, 파란색)으로 넣어서 풀었다.

1. queue에 색깔을 지정해서 넣어준다.

2. 인접한 노드를 갈 때 다른 색깔로 넣어준다.

3. 인접한 노드에 같은 색이 들어있을 때 0을 출력해준다.

----------

### 코드
```cpp
#include <cstdio>
#include <iostream>
#include <queue>
#include <vector>
#define RED 1
#define BLUE 2
using namespace std;

vector<int> edge[201];
int color[201];

bool bfs(int sp)
{
	queue<int> q;
	q.push(sp);
	color[sp] = RED;

	while (!q.empty()) {
		int curNode = q.front();
		int curColor = color[curNode];
		q.pop();

		for (int i = 0; i < edge[curNode].size(); i++) {
			int nextNode = edge[curNode][i];
			if (color[nextNode] != 0) {
				if (color[nextNode] == curColor) {
					return false;
				}
			}
			else {
				if (curColor == RED) color[nextNode] = BLUE;
				else color[nextNode] = RED;
				q.push(nextNode);
			}
		}
	}
	return true;
}

int main()
{
	int T;
	int test_case;

	scanf("%d", &T);
	for (test_case = 1; test_case <= T; test_case++) {
		int n, l;
		cin >> n >> l;
		// 초기화
		for (int i = 1; i <= n; i++) {
			edge[i].clear();
			color[i] = 0;
		}

		for (int i = 0; i < l; i++) {
			int a, b;
			cin >> a >> b;
			edge[a].push_back(b);
			edge[b].push_back(a);
		}

		bool flag = false;
		for (int i = 1; i <= n; i++) {
			if (color[i] == 0) {
				if (bfs(i) == false) flag = true;
			}
		}

		printf("Case #%d\n", test_case);
		if (flag) printf("0\n");
		else printf("1\n");
	}
	return 0;
}
```

