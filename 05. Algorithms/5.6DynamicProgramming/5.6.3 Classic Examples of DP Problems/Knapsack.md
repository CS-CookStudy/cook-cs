# 배낭 문제(Knapsack)

- 문제 번호 : 백준 12865
- 문제 내용 : 준서가 여행에 필요하다고 생각하는 N개의 물건이 있다. 각 물건은 무게 W와 가치 V를 가지는데, 해당 물건을 배낭에 넣어서 가면 준서가 V만큼 즐길 수 있다. 아직 행군을 해본 적이 없는 준서는 최대 K만큼의 무게만을 넣을 수 있는 배낭만 들고 다닐 수 있다. 준서가 최대한 즐거운 여행을 하기 위해 배낭에 넣을 수 있는 물건들의 가치의 최댓값을 알려주자.

```c++
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<iostream>
#include<string>
#include<vector>
using namespace std;

int items, capacity;
int volume[100], need[100];
int cache[1001][100];
string name[100];

// 캐리어에 남은 용량이 capacity 일 때, item 이후의 물건들을 담아 얻을 수 있는 최대
// 절박도의 합을 반환한다
int pack(int capacity, int item) {
	// 기저 사례: 더 담을 물건이 없을 때
	if(item == items) return 0;
	// 메모이제이션
	int& ret = cache[capacity][item];
	if(ret != -1) return ret;
	// 이 물건을 담지 않을 경우
	ret = pack(capacity, item+1);
	// 이 물건을 담을 경우
	if(capacity >= volume[item])
		ret = max(ret, pack(capacity - volume[item], item+1) + need[item]);
	return ret;
}

// pack(capacity, item) 이 선택한 물건들의 목록을 picked 에 저장한다
void reconstruct(int capacity, int item, vector<string>& picked) {
	if(item == items) return;
	if(pack(capacity, item) == pack(capacity, item+1))
		reconstruct(capacity, item+1, picked);
	else {
		picked.push_back(name[item]);
		reconstruct(capacity - volume[item], item+1, picked);
	}
}

int greedy() {
	vector<pair<double,int> > order;
	for(int i = 0; i < items; i++) {
		order.push_back(make_pair(-need[i] / double(volume[i]), i));
	}
	sort(order.begin(), order.end());
	int cap = capacity;
	int val = 0;
  vector<string> chosen;
	for(int i = 0; i < items; i++) {
		int idx = order[i].second;
		if(volume[idx] <= cap) {
			// fprintf(stderr, "Greedy adding #%d: %s (%g)\n", i, name[idx].c_str(), order[i].first);
			cap -= volume[idx];
			val += need[idx];
      chosen.push_back(name[idx]);
		}
	}
  printf("%d %d\n", val, chosen.size());
  for(int i = 0; i < chosen.size(); ++i) 
    printf("%s\n", chosen[i].c_str());
	return val;
}

int main() {
    int cases;
    cin >> cases;
    for(int cc = 0; cc < cases; ++cc) {
    	cin >> items >> capacity;
    	for(int i = 0; i < items; i++) {
    		cin >> name[i] >> volume[i] >> need[i];
    	}
    	memset(cache, -1, sizeof(cache));

    	vector<string> picked;
    	reconstruct(capacity, 0, picked);
    	printf("%d %d\n", pack(capacity, 0), int(picked.size()));
    	for(int i = 0; i < picked.size(); ++i)
    		printf("%s\n", picked[i].c_str());
      //
    	// int grdy = greedy();
    	// if(pack(capacity, 0) != grdy) fprintf(stderr, "greedy fails: return %d\n", grdy);
    }
}


```