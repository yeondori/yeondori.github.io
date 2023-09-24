---
title: "[데이터 구조론][과제] 05 스도쿠 Naked Single"

excerpt: "데이터 구조론 과제"
categories: [ERICA, Data Structures]
tags: [C++, Data Structures]
date: 2023-09-24
last_modified_at: 2023-09-24
render_with_liquid: false
---
# 스도쿠 - Naked Single

## 2019-10-6

### 프로그램 리스트

```cpp
#include<iostream>
using namespace std;

struct cell {
	int candidate[9];
	int sol;
	int count;
	int box;
};

struct hint_cell {
	int sol;
	int row;
	int col;
	int box;
};

void pop(hint_cell st[81], int& top) {
	top--;
}

void push(cell sudoku[][9], hint_cell st[81], int& top) {
	for (int i = 0; i < 9; i++) {
		for (int j = 0; j < 9; j++) {
			if (sudoku[i][j].count == 1) {
				for (int k = 0; k < 9; k++) {
					if (sudoku[i][j].candidate[k] == 0) {
						sudoku[i][j].sol = k + 1;
						sudoku[i][j].candidate[k] = 1;
						st[top] = { k + 1, i ,j, sudoku[i][j].box };
						top++;
					}
				}
			}
		}
	}
}

void row_cross_out(cell sudoku[][9], hint_cell st[81], int top){
	int pos = st[top].sol - 1;
	for (int i = 0; i < 9; i++)  {
		for (int j = 0; j < 9; j++)  {
			if (st[top].row == i && sudoku[i][j].sol == 0)  {
				if (sudoku[i][j].candidate[pos] == 0)  {
					sudoku[i][j].candidate[pos] = 1;
					sudoku[i][j].count--;
				}
			}
		}
	}
}

void col_cross_out(cell sudoku[][9], hint_cell st[81], int top)  {
	int pos = st[top].sol - 1;
	for (int i = 0; i < 9; i++)  {
		for (int j = 0; j < 9; j++)  {
			if (st[top].col == j && sudoku[i][j].sol == 0) {
        if (sudoku[i][j].candidate[pos] == 0)  {
					sudoku[i][j].candidate[pos] = 1;
					sudoku[i][j].count--;
				}
			}
		}
	}
}

void box_cross_out(cell sudoku[][9], hint_cell st[81], int top)  {
	int pos = st[top].sol - 1;
	for (int i = 0; i < 9; i++)  {
		for (int j = 0; j < 9; j++)  {
			if (st[top].box == sudoku[i][j].box && sudoku[i][j].sol == 0)  {
				if (sudoku[i][j].candidate[pos] == 0)  {
					sudoku[i][j].candidate[pos] = 1;
					sudoku[i][j].count--;
				}
			}
		}
	}
}


void print(cell sudoku[][9])  {
	for (int i = 0; i < 9; i++)  {
		for (int j = 0; j < 9; j++)  {
			cout << sudoku[i][j].sol << " ";

			if (j % 9 == 8)  {
				cout << endl;
			}
		}
	}
}

void print_count(cell sudoku[][9])  {
	for (int i = 0; i < 9; i++)  {
		for (int j = 0; j < 9; j++)  {
			cout << sudoku[i][j].count << " ";

			if (j % 9 == 8)  {
				cout << endl;
			}
		}
	}
}

int main()  {
	cell sudoku[9][9] = { {{{0},0,9,1}, {{0},0,9,1}, {{0},0,9,1}, {{0},0,9,2}, {{1},8,0,2}, {{1},3,0,2}, {{0},0,9,3}, {{0},0,9,3}, {{0},0,9,3}},
						  {{{1},7,0,1}, {{0},0,9,1}, {{0},0,9,1}, {{0},0,9,2}, {{1},6,0,2}, {{0},0,9,2}, {{0},0,9,3}, {{1},9,0,3}, {{1},8,0,3}},
						  {{{1},9,0,1}, {{0},0,9,1}, {{1},8,0,1}, {{1},7,0,2}, {{0},0,9,2}, {{0},0,9,2}, {{0},0,9,3}, {{0},0,9,3}, {{0},0,9,3}},
						  {{{0},0,9,4}, {{0},0,9,4}, {{1},1,0,4}, {{1},3,0,5}, {{0},0,9,5}, {{0},0,9,5}, {{1},8,0,6}, {{0},0,9,6}, {{0},0,9,6}},
						  {{{1},5,0,4}, {{1},9,0,4}, {{0},0,9,4}, {{1},1,0,5}, {{1},7,0,5}, {{1},8,0,5}, {{0},0,9,6}, {{1},3,0,6}, {{1},4,0,6}},
						  {{{0},0,9,4}, {{0},0,9,4}, {{1},3,0,4}, {{0},0,9,5}, {{0},0,9,5}, {{1},2,0,5}, {{1},9,0,6}, {{0},0,9,6}, {{0},0,9,6}},
						  {{{0},0,9,7}, {{0},0,9,7}, {{0},0,9,7}, {{0},0,9,8}, {{0},0,9,8}, {{1},7,0,8}, {{1},4,0,9}, {{0},0,9,9}, {{1},2,0,9}},
						  {{{1},8,0,7}, {{1},2,0,7}, {{0},0,9,7}, {{0},0,9,8}, {{1},9,0,8}, {{0},0,9,8}, {{0},0,9,9}, {{0},0,9,9}, {{1},7,0,9}},
						  {{{0},0,9,7}, {{0},0,9,7}, {{0},0,9,7}, {{1},5,0,8}, {{1},2,0,8}, {{0},0,9,8}, {{0},0,9,9}, {{0},0,9,9}, {{0},0,9,9}} };

	hint_cell st[81] = { {2,8,4,8}, {5,8,3,8}, {7,7,8,9}, {9,7,4,8}, {2,7,1,7}, {8,7,0,7}, {2,6,8,9}, {4,6,6,9}, {7,6,5,8}, {9,5,6,6}
					   , {2,5,5,5}, {3,5,2,4}, {4,4,8,6}, {3,4,7,6}, {8,4,5,5}, {7,4,4,5}, {1,4,3,5}, {9,4,1,4}, {5,4,0,4}, {8,3,6,6}
					   , {3,3,3,5}, {1,3,2,4}, {7,2,3,2}, {8,2,1,1}, {9,2,0,1}, {8,1,8,3}, {9,1,7,3}, {6,1,4,2}, {7,1,0,1}, {3,0,5,2}
					   , {8,0,4,2} };
	int top = 31;

	print(sudoku);
	cout << endl << endl << "soluiton : " << endl << endl;
	while (top > 0)  {
		pop(st, top);
		row_cross_out(sudoku, st, top);
		col_cross_out(sudoku, st, top);
		box_cross_out(sudoku, st, top);
		push(sudoku, st, top);
	}
	print(sudoku);
	cout << endl << endl << "candidate : " << endl << endl;
	print_count(sudoku);
	return 0;
}


```

### 출력 결과

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/ea93e89c-5b0d-40ee-91ba-4b9eb2742de1)
순서대로 cross out 전의 스도쿠, cross out 후의 스도쿠, 스도쿠의 candidate

### 소감문

이번 주의 과제는 대망의 스도쿠였습니다. 코딩을 하기 앞서, 저는 스도쿠를 직접 풀어보았습니다. 히든 싱글을 생각하지 않고, cross out만 하면서 naked single을 찾았습니다. 그러나 몇 번을 해봐도 naked single이 나오지 않았습니다. 화요일 하루 동안은 naked single만 한 것 같습니다. 그래서 동기들에게 말해보았더니 다들 naked single이 나오지 않는다고 하여, 교수님께 메일을 보냈습니다. naked single 코드를 만들기 위하여 인터넷에서 naked single만으로도 풀리는 스도쿠를 찾아 코드를 작성해보았습니다. 우선, 알고리즘은 이렇습니다. candidate 배열을 모두 0으로 초기화한 뒤, cross out을 진행하여 힌트와 대응하는 candidate 위치에 0 대신 1을 대입하고, count를 줄였습니다. 이러한 과정을 마친 뒤, count가 1이 됐을 때(즉, candidate 배열 속에는 0인 부분이 한 자리밖에 없음), 0인 곳의 위치를 출력하는 방식으로 구상했습니다. 알고리즘을 구상하고 코드를 작성하는데는 그렇게 오랜 시간이 걸리지 않았지만, 구현하는 데 오류가 있어 스도쿠를 완성하지 못했습니다. while문이 끝나지 않을 때도 있었고, 잘못된 값이 대입될 때도 있었고, 첫 번째 naked single이 덜 나오는 경우도 있었습니다. 그래서 cross out 함수를 잘게 쪼개고, 부분마다 cout으로 진행상황을 확인하였습니다. 그 결과, 오류는 힌트 셀 하나를 잘못 입력했기 때문에 발생했다는 것을 파악할 수 있었습니다. 말이 쉽지 사실 오류 하나를 발견하는 데 2일이 걸렸고, 지금도 급하게 소감문을 작성하고 있습니다. 울기 직전에 오류를 발견해서 참 다행이라는 생각이 듭니다. 하고 싶었던 말이 정말 많았지만 제출 시간이 다가오고 있어서 여기까지만 하겠습니다. 아직까지 한 끼도 못 먹었는데 밥을 먹고 와서 히든 싱글을 생각해 볼 예정입니다. 아래 첨부한 사진은 제 고민의 흔적입니다…
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/aeb43f16-8817-4a38-92ce-68903551d8a3)

## 2023-09-24

애증의 스도쿠! 매일매일 수업 끝나면 놀리지팩토리로 달려가서 자리 잡고 스도쿠만 주구장창 붙잡고 있던 과거 그 시절은 몇년이 지나도 잊지 못할 거고,, C++을 D+ 맞은 실력으로 이걸 해냈다는 게 아직도 너무나 자랑스럽고 대견하다🥲
네이키드 싱글은 답을 찾고자 하는 해당 셀에 대해 행과 열, 그리고 그 셀이 속해 있는 박스(9*9) 내에 등장하지 않은 숫자가 유일할 때를 의미한다. 예를 들어 다음의 그림에서 행과 열, 박스가 교차하는 지점에서 오직 6만 해답이 될 수 있다. 이러한 셀을 네이키드 싱글이라고 한다.

![image]((https://github.com/yeondori/yeondori.github.io/assets/93027942/1a67139e-b817-4392-8ca8-0bd086efc7e4) [참고](https://m.blog.naver.com/roty22/220697379898)

수행 순서는 다음과 같다.
- 힌트 스택에서 Pop으로 힌트를 가져온다.
- 힌트를 바탕으로 행과 열, 박스를 순차적으로 탐색한다.(cross out)) 해당 영역에 존재하는 숫자는 정답이 될 수 없으므로 해당 후보군에서 소거한다. (구현 코드에서는 candidate라는 배열에 해당 숫자의 인덱스에 1을 넣고 count를 줄이는 방식)
- cross out이 모두 끝나면 카운트가 1인 셀의 candidate를 확인해 0인 곳의 index를 바탕으로 솔루션을 등록하고 힌트 스택에 해당 셀을 push한다.
- 위와 같은 과정을 힌트 스택을 전부 탐색할 때까지 반복한다.

