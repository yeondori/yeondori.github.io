---
title: "[데이터 구조론][과제] 11 외판원 문제 – Nearest Neighbor(수정중)"

excerpt: "데이터 구조론 과제"
categories: [ERICA, Data Structures]
tags: [C++, Data Structures]
date: 2023-09-27
last_modified_at: 2023-10-04
render_with_liquid: false
---
# 외판원 문제 – Nearest Neighbor

## 2019-11-16

### 프로그램 리스트

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

void print_vec(vector<vector<int>> x) {
	for (int row = 0; row < 5; ++row) {
		for (int col = 0; col < 5; ++col) {
			cout << x[row][col] << " ";
		}
		cout << endl;
	}
}

void print_path(int path[6]) {
	cout << "path : ";

	for (int i = 0; i < 6; i++) {
		cout << path[i] << "  ";
	}
}

void print_total_cost(int total_cost[5]) {
	cout << endl << "total cost : ";
	int sum = 0;
	for (int i = 0; i < 5; i++) {
		sum = sum + total_cost[i];
	}
	cout << sum << endl;
}

int find_min(vector<vector<int>> x, int row) {
	int pos = 0;
	int min = x[row][0];
	for (int col = 0; col < 5; ++col) {
		if (min > x[row][col]) {
			min = x[row][col];
			pos = col;
		}
	}
	return pos;
}

int main() {
	vector<vector<int>> cost;
	vector<int> a1 = { 999, 25, 40, 31, 27 };
	vector<int> a2 = { 5, 999, 17, 30, 25 };
	vector<int> a3 = { 19, 15, 999, 6, 1 };
	vector<int> a4 = { 9, 50, 24, 999, 6 };
	vector<int> a5 = { 22, 8, 7, 10, 999 };
	
	cost.push_back(a1);
	cost.push_back(a2);
	cost.push_back(a3);
	cost.push_back(a4);
	cost.push_back(a5);

	int path[6] = { 0 };
	int total_cost[5] = { 0 };
	cout << "비용 : " << endl;
	print_vec(cost);
	cout << endl << endl;

	for (int j = 0; j < 5; j++) {
		vector<vector<int>> c_cost(cost);
		c_cost.push_back(a1);
		c_cost.push_back(a2);
		c_cost.push_back(a3);
		c_cost.push_back(a4);
		c_cost.push_back(a5);

		int count = 0;
		path[0] = j+1; 
		int i = j; 
		while (count != 4) {
			for (int row = 0; row < 5; row++) {
				c_cost[row][i] = 999;
			}
			int pos = find_min(c_cost, i);
			path[count + 1] = pos + 1;
			total_cost[count] = c_cost[i][pos];
			i = pos;
			count++;
		}
		path[5] = j+1; 
		total_cost[4] = cost[i][j]; 
		c_cost.clear();

		cout << "본점이 " << j+1 << "일 때, " << endl;
		print_path(path);
		print_total_cost(total_cost);
		cout << endl;
	}
return 0;
}

```

### 출력 결과

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/041b6e2a-9f40-4d22-bca9-53132ca6e20b)

### 소감문

이번 과제는 최소의 비용으로 모든 곳에 딱 한번씩만 들려야 하는 외판원 문제였습니다. 
그 중에서도 nearest neighbor를 이용하는 것이었는데, nearest neighbor는 우선 본점에서 가장 가까운(최소 비용의) 점을 찾은 뒤, 그 점을 기준으로 가장 가까운 점을 찾는 방식입니다. 
제가 초기에 구상한 알고리즘은 다음과 같습니다. 먼저, cost와 관련된 2차원 vector를 복사하여 c_cost라고 합니다. 
이미 지나간 점을 다시 한번 지나지 않게 하려면 값을 덮어 씌워야 하는데, 기존의 cost 배열에 덮어쓰면 cost 배열을 다시 활용하기가 어렵기 때문이었습니다. 
모든 탐색과 덮어씌우기 작업은 c_cost에서 진행됩니다. 
본점이 1일 때를 예시로 들자면, 먼저 1행을 탐색하여 min값과 그 위치를 찾습니다. 
min값은 total_cost 배열에 저장하고, 그 위치인 pos은 path 배열에 저장합니다. 
다른 점에서 1열의 값을 최소값으로 찾지 못하도록 (1번으로 돌아가지 못하도록) 1열의 모든 값을 아주 큰 값(999)로 덮어 씌웁니다. 
이제 pos(=2)을 기준으로 위와 같은 작업을 반복합니다. 경로의 시작과 끝은 항상 본점임을 유의하여 다음의 과정을 반복하면 본점에 따른 경로와 총 비용을 계산할 수 있게 됩니다. 
과제를 하면서 2차원을 vector로 표현하려면 스도쿠 문제에서 struct를 이용하듯, vector<vector<int>>의 형식으로 사용해야 된다는 것을 알았습니다. 
그러나 문제는 이 안의 원소들을 어떻게 지정할 지였습니다. 고민 끝에 전 2D 배열을 사용하듯 사용하기로 했습니다. 
아쉬웠던 점은 STL을 잘 활용하지 못했다는 것이었습니다. Find_min과 같은 경우도 find함수와 min함수가 모두 있음에도 불구하고 2차원 vector에서 활용하지 못해 제가 직접 함수를 만들어 주어야 했습니다. 
또 처음에 구상했던 방식과는 다르게, 새로 만들어 주어야하는 인수들과 반복문이 많아 불편했습니다.

## 2023-09-27


(수정중)