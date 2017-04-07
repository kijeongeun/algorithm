### Codeground "미궁 속의 방"
(https://www.codeground.org/practice/practiceProbView.do)

*하 이문제 너무 많이 봐서 외울것 같다.....*

####풀이 방법

일단, N = 5인 표가 다음과 같이 있다고 치자. 

| (0,0) | (0,1) | (0,2) | (0,3) | (0,4) |
|-------|-------|-------|-------|-------|
| (1,0) | (1,1) | (1,2) | (1,3) | (1,4) |
| (2,0) | (2,1) | (2,2) | (2,3) | (2,4) |
| (3,0) | (3,1) | (3,2) | (3,3) | (3,4) |
| (4,0) | (4,1) | (4,2) | (4,3) | (4,4) |

대각선에 따라 좌표를 분류해 보자면 

    ↗: (0, 0) = 합 0
    
    ↙: (0, 1), (1, 0) = 합 1

    ↗: (2, 0), (1, 1), (0, 2) = 합 2
    
    ↙: (0, 3), (1, 2), (2, 1), (3, 0) = 합 3
    
    ↗: (4, 0), (3, 1), (2, 2), (1, 3), (0, 4) = 합 4
    
    ...

이런식으로 합 8까지 순차적으로 가는 것을 확인 할 수 있다.

**대각선의 합**이 **짝수인지, 홀수인지**에 따라 처리를 달리해서 풀었다.

현재 좌표의 번호가 몇번인지 알기 위해 **대각선마다 시작 번호**를 구했다.

1, 2, 4, 7, 11, 16, 20, 23, 25 이런 수열이 나왔다.

이 수열을 살펴보면

 1, 2, 3, 4, 5 을 더한 만큼 순차적으로 증가하지만, 

더하는 숫자가 N이 넘어가면 또다시 N-1, N-2, N-3.. 만큼 감소한 숫자가 나타났다.

이것을 w 배열에 저장했다.

그 후, 현재 대각선이 w의 몇번째 인덱스의 대각선 시작번호를 가지고있고, 

거기서 몇번을 더 가야하는지를 sum() 함수에서 구했다.

####코드
```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>
#define MAX 100000
using namespace std;

long long w[MAX * 2 + 100];
int y, x;
int n;

long long sum()
{
	if (y == 0 && x == 0) return 1;
	if (y == n - 1 && x == n - 1) return n*n;

	long long ret = 0;
	if ((y + x) % 2 == 0) { // 왼->오른
		if (y + x < n) {
			ret += w[y + x] + x;
		}
		else {
			ret += w[y + x] + n - 1 - y;
		}
	}
	else {
		if (y + x < n) {
			ret += w[y + x] + y;
		}
		else {
			ret += w[y + x] + n - 1 - x;
		}
	}
	return ret;
}

int main()
{
	int tc;
	cin >> tc;
	for (int t = 1; t <= tc; t++) {
		int k;
		cin >> n >> k;

		int idx = 1;
		w[0] = 1;
		for (; idx <= n; idx++) {
			w[idx] = w[idx - 1] + idx;
		}
		for (int i = n - 1; i > 2; i--) {
			w[idx] = w[idx - 1] + i;
			idx++;
		}

		y = x = 0;
		long long ret = 1;

		for (int i = 0; i < k; i++) {
			char a;
			cin >> a;

			if (a == 'L') x--;
			else if (a == 'R') x++;
			else if (a == 'U') y--;
			else if (a == 'D') y++;

			ret += sum();
		}

		cout << "Case #" << t << '\n' << ret << '\n';
	}
	return 0;
}
```

