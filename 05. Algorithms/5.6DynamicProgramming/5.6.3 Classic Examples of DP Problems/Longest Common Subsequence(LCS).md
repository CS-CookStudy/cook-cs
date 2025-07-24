# 최장 공통 부분 수열(LCS)
- Logest Common Subsequence
- 두 수열이 주어졌을 때, 가장 긴 공통 부분 문자열을 **최장 공통 부분 수열**이라함
- 최장 공통 문자열(Longest Common String)과 비슷하지만 둘의 차이점은 공통 부분이 연속하는지 아닌지 여부
- *수열 : 문자들이 연속되어 있을 필요는 없지만 순서는 지켜야 함
- 예)
  - 최장 공통 부분 수열 : ACAYKP와 CAPCAK의 LCS는 ACAK
  - 최장 공통 문자열 : ACAYKP와 CAPCAK의 CA

## 문제 풀이
- 문제 번호 : 백준 9251 LCS
- 문제 내용 : 두 수열이 주어졌을 때, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾으시오.


- 풀이 방식
> 같은 문자 비교식 : `A[i-1] == B[j-1]이면 → dp[i][j] = dp[i-1][j-1] + 1`

> 다른 문자 비교식 : `dp[i][j] = max(dp[i-1][j], dp[i][j-1])` 

1. dp 테이블(2차원 배열)을 만들어 비교
    - (문자열 1의 길이 + 1) * (문자열 2의 길이 + 1) 크기의 2차원 배열을 만들기
    - 같은 문자 "ca" 와 "ca" 일 경우는 같은문자 비교식(대각선+1)
    - 다른문자 " cA" 와 "a"일 경우는 다른문자 비교식(위/왼쪽 중 큰 값)
   
    |    | "" | C | A | P | C | A | K |
    | -- | -- | - | - | - | - | - | - |
    | "" | 0  | 0 | 0 | 0 | 0 | 0 | 0 |
    | A  | 0  | 0 | 1 | 1 | 1 | 1 | 1 |
    | C  | 0  | 1 | 1 | 1 | 2 | 2 | 2 |
    | A  | 0  | 1 | 2 | 2 | 2 | 3 | 3 |
    | Y  | 0  | 1 | 2 | 2 | 2 | 3 | 3 |
    | K  | 0  | 1 | 2 | 2 | 2 | 3 | 4 |
    | P  | 0  | 1 | 2 | 3 | 3 | 3 | 4 |


```C++
<C++>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main() {
    string A, B;
    cin >> A >> B;

    int n = A.length();
    int m = B.length();

    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            if (A[i - 1] == B[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    cout << dp[n][m] << '\n'; // LCS의 길이
    return 0;
}

```