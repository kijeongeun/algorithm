## BOJ 2931번 "가스관"
(https://www.acmicpc.net/problem/2931)

----------
### 분류: BFS

----------
### 풀이 방법

가스관이 딱 하나 빠진거라는 사실을 이용하면 의외로 쉽게 풀린다.

M부터 BFS를 돌면서 주변에 가스관을 보고, 현재 가스관이 어떤 모양일지 추측한다.

주변 모양과 맞으면 가스관 모양을 출력한다.

----------

### 코드
```cpp
#include <iostream>
#include <queue>
#define BLOCK_VER	'|'
#define BLOCK_1		'1'
#define BLOCK_2		'2'
#define BLOCK_3		'3'
#define BLOCK_4		'4'
#define BLOCK_HOR	'-'
#define BLOCK_CRO	'+'
using namespace std;

char arr[26][26];
bool visit[26][26];
int dy[] = { -1,1,0,0 };
int dx[] = { 0,0,-1,1 };
int n, m;
struct Q {
	int y; int x;
	char bridge;
};
queue<Q> q;

bool check_plus_y(int ny, int nx)
{
	ny = ny + 1;
	if (ny<= n - 1 &&
		(arr[ny][nx] == BLOCK_VER || arr[ny][nx] == BLOCK_CRO ||
			arr[ny][nx] == BLOCK_2 || arr[ny][nx] == BLOCK_3)) {
		return true;
	}
	return false;
}

bool check_minus_y(int ny, int nx)
{
	ny = ny - 1;
	if (ny >= 0 &&
		(arr[ny][nx] == BLOCK_VER || arr[ny][nx] == BLOCK_CRO ||
			arr[ny][nx] == BLOCK_1 || arr[ny][nx] == BLOCK_4)) {
		return true;
	}
	return false;
}

bool check_plus_x(int ny, int nx)
{
	nx = nx + 1;
	if (nx <= m - 1 &&
		(arr[ny][nx] == BLOCK_HOR || arr[ny][nx] == BLOCK_CRO ||
			arr[ny][nx] == BLOCK_3 || arr[ny][nx] == BLOCK_4)) {
		return true;
	}
	return false;
}

bool check_minus_x(int ny, int nx)
{
	nx = nx - 1;
	if (nx >= 0 &&
		(arr[ny][nx] == BLOCK_HOR || arr[ny][nx] == BLOCK_CRO ||
			arr[ny][nx] == BLOCK_1 || arr[ny][nx] == BLOCK_2)) {
		return true;
	}
	return false;
}

void eq_minus_x(int ny, int nx, char bridge)
{
	if (nx >= 0 && !visit[ny][nx]) {
		if (arr[ny][nx] != '.') {
			visit[ny][nx] = true;
			q.push({ ny,nx,bridge });
		}
		else if (arr[ny][nx] == '.' && bridge == NULL) {
			if (check_plus_y(ny, nx) && check_minus_y(ny, nx) && check_minus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_CRO });
			}
			else if (check_minus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_HOR });
			}
			else if (check_plus_y(ny, nx)){
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_1 });
			}
			else if (check_minus_y(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_2 });
			}
		}
	}
}

void eq_plus_x(int ny, int nx, char bridge)
{
	if (nx <= m - 1 && !visit[ny][nx]) {
		if (arr[ny][nx] != '.') {
			visit[ny][nx] = true;
			q.push({ ny,nx,bridge });
		}
		else if (arr[ny][nx] == '.' && bridge == NULL) {
			if (check_plus_y(ny, nx) && check_minus_y(ny, nx) && check_plus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_CRO });
			}
			else if(check_plus_y(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_4 });
			}
			else if (check_minus_y(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_3 });
			}
			else if (check_plus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_HOR });
			}
		}
	}
}

void eq_minus_y(int ny, int nx, char bridge)
{
	if (ny >= 0 && !visit[ny][nx]) {
		if (arr[ny][nx] != '.') {
			visit[ny][nx] = true;
			q.push({ ny,nx,bridge });
		}
		else if (arr[ny][nx] == '.' && bridge == NULL) {
			if (check_minus_y(ny, nx) && check_minus_x(ny, nx) && check_plus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_CRO });
			}
			else if (check_minus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_4 });
			}
			else if (check_minus_y(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_VER });
			}
			else if (check_plus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_1 });
			}
		}
	}
}

void eq_plus_y(int ny, int nx, char bridge)
{
	if (ny <= n - 1 && !visit[ny][nx]) {
		if (arr[ny][nx] != '.') {
			visit[ny][nx] = true;
			q.push({ ny,nx,bridge });
		}
		else if (arr[ny][nx] == '.' && bridge == NULL) {
			if (check_plus_y(ny, nx) && check_minus_x(ny, nx) && check_plus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_CRO });
			}
			else if (check_minus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_3 });
			}
			else if (check_plus_y(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_VER });
			}
			else if (check_plus_x(ny, nx)) {
				visit[ny][nx] = true;
				q.push({ ny,nx,BLOCK_2 });
			}
		}
	}
}

void bfs()
{
	while (!q.empty()) {
		int y = q.front().y;
		int x = q.front().x;
		char bridge = q.front().bridge;
		q.pop();

		if (bridge != NULL) {
			cout << y+1 << ' ' << x+1 << ' ' << bridge;
			return;
		}

		if (arr[y][x] == 'M') {
			for (int i = 0; i < 4; i++) {
				int ny = dy[i] + y;
				int nx = dx[i] + x;
				if (ny >= 0 && nx >= 0 && ny <= n - 1 && nx <= m - 1 && !visit[ny][nx]) {
					visit[ny][nx] = true;
					q.push({ ny,nx,bridge });
				}
			}
		}
		else if (arr[y][x] == BLOCK_VER) {
			eq_plus_y(y + 1, x, bridge);
			eq_minus_y(y-1, x, bridge);
		}
		else if (arr[y][x] == BLOCK_1) {
			eq_plus_y(y + 1, x, bridge);
			eq_plus_x(y, x + 1, bridge);
		}
		else if (arr[y][x] == BLOCK_2) {
			eq_plus_x(y, x + 1, bridge);
			eq_minus_y(y - 1, x, bridge);
		}
		else if (arr[y][x] == BLOCK_3) {
			eq_minus_x(y, x - 1, bridge);
			eq_minus_y(y - 1, x, bridge);
		}
		else if (arr[y][x] == BLOCK_4) {
			eq_plus_y(y + 1, x, bridge);
			eq_minus_x(y, x - 1, bridge);
		}
		else if (arr[y][x] == BLOCK_HOR) {
			eq_minus_x(y, x - 1, bridge);
			eq_plus_x(y, x + 1, bridge);
		}
		else if (arr[y][x] == BLOCK_CRO) {
			eq_minus_x(y, x - 1, bridge);
			eq_plus_x(y, x + 1, bridge);
			eq_minus_y(y - 1, x, bridge);
			eq_plus_y(y + 1, x, bridge);
		}
		else { // arr[y][x] == '.' 또는 도착점
			if (bridge == BLOCK_VER) {
				eq_plus_y(y + 1, x, bridge);
				eq_minus_y(y - 1, x, bridge);
			}
			else if (bridge == BLOCK_1) {
				eq_plus_y(y + 1, x, bridge);
				eq_plus_x(y, x + 1, bridge);
			}
			else if (bridge == BLOCK_2) {
				eq_plus_x(y, x + 1, bridge);
				eq_minus_y(y - 1, x, bridge);
			}
			else if (bridge == BLOCK_3) {
				eq_minus_x(y, x - 1, bridge);
				eq_minus_y(y - 1, x, bridge);
			}
			else if (bridge == BLOCK_4) {
				eq_plus_y(y + 1, x, bridge);
				eq_minus_x(y, x - 1, bridge);
			}
			else if (bridge == BLOCK_HOR) {
				eq_minus_x(y, x - 1, bridge);
				eq_plus_x(y, x + 1, bridge);
			}
			else if (bridge == BLOCK_CRO) {
				eq_minus_x(y, x - 1, bridge);
				eq_plus_x(y, x + 1, bridge);
				eq_minus_y(y - 1, x, bridge);
				eq_plus_y(y + 1, x, bridge);
			}
		}
	}
}

int main()
{
	cin >> n >> m;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			cin >> arr[i][j];
			if (arr[i][j] == 'M') 
				q.push({ i,j,NULL });
		}
	}

	bfs();
	return 0;
}
```

