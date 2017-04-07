### Codeground "좋은 수"
[문제 링크](https://www.codeground.org/practice/practiceProbView.do)



**풀이 방법**

a + b + c = d 를 구하는 문제이다.

1.  a, b, c는 d보다 적어도 하나의 index 앞에 위치해야 한다.

2. a + b + c = d 는, a + b = d - c 와 같다.

3. a + b 를 입력 받을 때 마다 계산해서 체크해 두고, d - c 와 비교한다. 

4. 입력받은 값은 -10만 ~ 10만의 값이므로, 20만을 더해서 체크한다.

    


----------
예를 들어 설명하자면, 

계산할 배열이 1, 2, 3, 5, 9 라고 하자.

0번째 인덱스 일 때는 arr[0] + arr[0] 을 계산해서 배열에 체크한다.

    sumArr[20만 + arr[0] + arr[0]] = 1 을 넣어준다.

1번째 인덱스 일 때는 arr[1] + arr[0], arr[1] + arr[1] 을 계산해서 배열에 체크한다.

    sumArr[20만 + arr[1] + arr[0]] = 1, 

    sumArr[20만 + arr[1] + arr[1]] = 1 을 넣어준다.

2번째 인덱스 일 때는 arr[2] + arr[0], arr[2] + arr[1], arr[2] + arr[2] 을 계산해서 배열에 체크한다.

    sumArr[20만 + arr[2] + arr[0]] = 1, 

    sumArr[20만 + arr[2] + arr[1]] = 1,

    sumArr[20만 + arr[2] + arr[2]] = 1 을 넣어준다.

    ...

이런식으로 n-1까지 배열에 체크한다. 

(마지막 index를 사용하지 않는 이유는, "a, b, c는 d보다 적어도 하나의 index 앞에 위치해야 한다."에 위배되기 때문.)

이렇게 각 index마다 **sumArr배열을 구하기 전에 해야할 것**이 있다.

**index가 1 이상일 때, d - c 를 체크**해줘야 한다.

sumArr[20만 + arr[현재 인덱스] - arr[0 ~ 현재인덱스 전까지]] == 1 인지 확인하여 최종 출력값을 1 증가시킨다.

이 부분이 sumArr 배열을 구하기 전에 나와야 하는 이유는, 현재 인덱스의 값이 들어와서 추가된 sumArr배열이 영향을 줄 수 있기 때문이다.

이것또한 간단한 예제를 통해 확인하자면 

배열 [0, -1, 4] 가 input으로 들어왔다. 

index가 1일 때 arr[1] = -1를 보자.

sumArr[20만 + 0 + -1] = 1 로 체크 된다면

sumArr[20만 + -1 - 0] == 1이므로 출력값 카운트가 증가될 것이다.

0 + 0 + -1 = -1 이라고 생각하는 것이다. (a + b + c = d에서 a,b,c,는 d보다 적어도 하나의 index 앞에 위치해야 하기 때문)

그래서 d - c 를 체크해 주는 부분이 a + c 보다 앞에 나와야 한다.

```cpp
#include <cstdio>
#include <iostream>
#define MAX 100000
using namespace std;

int arr[5001];
int sumArr[MAX*5];

int main()
{
	setbuf(stdout, NULL);
	int tc;
	cin >> tc;
	for (int T = 1; T <= tc; T++) {
		int n;
		cin >> n;
		fill(sumArr, sumArr + MAX * 5, 0);
		int cnt = 0;

		for (int i = 0; i < n; i++) {
			cin >> arr[i];
			if (i > 0) {
				for (int j = 0; j < i; j++) {
					if (sumArr[MAX * 2 + arr[i] - arr[j]] == 1) {
						//cout << arr[i] << ' ';
						cnt++;
						break;
					}
				}
			}
			for (int j = 0; i<n-1 && j <= i; j++) {
				sumArr[MAX * 2 + arr[i] + arr[j]] = 1;
			}
		}

		cout << "Case #" << T << '\n' << cnt << '\n';
	}
	return 0;
}
```
