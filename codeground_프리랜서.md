### Codeground "프리랜서"
(https://www.codeground.org/practice/practiceProbView.do#)


----------

#### 풀이 방법

첫 주는 Q도 포함한다. 라는 것을 잘 읽어야 할듯.

D[n] = max(Q[n] + D[n - 2], P[n] + D[n - 1])

----------

#### 코드
```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int P[10001];
int Q[10001];
int dp[10001];

int max(int a, int b)
{
	if (a > b) return a;
	return b;
}

int main(int argc, char** argv) {
	setbuf(stdout, NULL);

	int T;
	int test_case;

	scanf("%d", &T);	// Codeground 시스템에서는 C++에서도 scanf 함수 사용을 권장하며, cin을 사용하셔도 됩니다.
	for (test_case = 1; test_case <= T; test_case++) {
		int n; 
		scanf("%d", &n);
		fill(dp, dp + n, 0);
		for (int i = 0; i<n; i++) {
			scanf("%d", &P[i]);
		}
		for (int i = 0; i<n; i++) {
			scanf("%d", &Q[i]);
		}
		dp[0] = max(P[0], Q[0]);
		if (n >= 2) {
			dp[1] = max(dp[0] + P[1], Q[1]);
			for (int i = 2; i < n; i++) {
				dp[i] = max(Q[i] + dp[i - 2], P[i] + dp[i - 1]);
			}
		}
		printf("Case #%d\n", test_case);
		printf("%d\n", dp[n - 1]);
	}

	return 0;	// 정상종료 시 반드시 0을 리턴해야 합니다.
}
```

