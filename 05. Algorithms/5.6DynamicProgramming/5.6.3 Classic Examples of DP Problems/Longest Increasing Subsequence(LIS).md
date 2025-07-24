# 최장 증가 부분 수열(LIS)
- Longest Increasing Subsequence
- 원소가 n개인 배열의 일부 원소를 골라내서 만든 부분 수열 중, 숫자들이 순 증가(strictly increasing)하면 이 부분을 증가 부분 수열이라고 함
- 이러한 증가 부분 수열 중 가장 긴 것을 찾는 문제를 **최장 증가 부분 수열**이라고 함
- 예) 
  - '1 3 4 2 4' 중 '1 2 4'는 증가수열

## 문제 풀이
- 문제 번호 : 백준 11054 가장 긴 바이토닉 부분 수열
- 문제 내용 : 수열 A가 주어졌을 때, 그 수열의 부분 수열 중 바이토닉 수열(증가했다 감소하는 수열)이면서 가장 긴 수열의 길이를 구하는 프로그램을 작성하시오.


- 풀이 방식
> 이 문제는 for문 기반의 tabulation 방식(DP) 으로 풀어야 함

> 이유는 작은 부분 문제(하위 인덱스)부터 차례대로 계산해 나가야 전체 해를 구할 수 있기 때문에

  1. 배열 중 바이토닉 수열을 만들 수 있는 인덱스 i 를 찾기
  2. i 를 기준으로 왼쪽은 증가 
  3. i 를 기준으로 오른쪽은 감소

```C++
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
  // 수열의 길이
  int n;
  std::cin >> n;

  // 수열
  std::vector<int> lis(n);
  for(int i=0; i<n; ++i)
  {
    std::cin >> lis[i];
  }
  // 증가수열 저장(왼쪽)
  std::vector<int> lup(n, 1);    
  // 감소수열 저장(오른쪽)
  std::vector<int> ldown(n, 1);   

  // 정방향 lis 계산
  for(int i=0; i < n; ++i)       // 1. 수열의 모든 위치 i에 대해 반복
  {
    for(int j=0; j<i; ++j)       // 2. i보다 앞선 위치 j를 전부 검사
    {
      if(lis[j] < lis[i])        // 3. lis[j] 가 lis[i] 보다 작은지 검사
      {
        lup[i] =std::max(lup[i], lup[j]+1);
      }
    }
  }

  // 역방향 lis 계산
  for (int i = n-1; i >=0; --i)
  {
    for(int j = n-1; j > i; --j)
    {
      if(lis[j] < lis[i])
      {
        ldown[i] = std::max(ldown[i], ldown[j]+1);
      }
    }
  }

  // 정방향(lup) + 역방향(ldown) 합치기
  int result = 0;
  for (int i=0; i<n; ++i)
  {
    result = std::max(result, lup[i] + ldown[i] -1);
  }
  std::cout << result << std::endl;
  
  return 0;
}
```
- 시간복잡도 : O(n^2)