---
title: "[데이터 구조론][과제] 06 스도쿠 Hidden Single"

excerpt: "데이터 구조론 과제"
categories: [ERICA, Data Structures]
tags: [C++, Data Structures]
date: 2023-09-25
last_modified_at: 2023-09-25
render_with_liquid: false
---
# 스도쿠 - Hidden Single

## 2019-10-15

### 프로그램 리스트

```cpp

include<iostream>
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
void push(cell sudoku[][9], hint_cell st[81], int& top, int& total_sol) {
	for (int i = 0; i < 9; i++) {
		for (int j = 0; j < 9; j++) {
			if (sudoku[i][j].count == 1 && sudoku[i][j].sol == 0) {
				for (int k = 0; k < 9; k++) {
					if (sudoku[i][j].candidate[k] == 0&&sudoku[i][j].count != 0) {
						sudoku[i][j].sol = k + 1;
						for (int l = 0; l<9; l++) { 
						sudoku[i][j].candidate[k] = 1;
						}
						sudoku[i][j].count = 0;
						st[top] = { k + 1, i ,j, sudoku[i][j].box };
						top++;
						total_sol++;
					}
				}
			}
		}
	}
	
}

void row_cross_out(cell sudoku[][9], hint_cell st[81], int top) {
	int pos = st[top].sol - 1;
	for (int i = 0; i < 9; i++) {
		for (int j = 0; j < 9; j++) {
			if (st[top].row == i && sudoku[i][j].sol == 0) {
				if (sudoku[i][j].candidate[pos] == 0 && sudoku[i][j].count != 0) {
					sudoku[i][j].candidate[pos] = 1;
					sudoku[i][j].count--;
				}
			}
		}
	}
}
void col_cross_out(cell sudoku[][9], hint_cell st[81], int top) {
	int pos = st[top].sol - 1;
	for (int i = 0; i < 9; i++) {
		for (int j = 0; j < 9; j++) {
			if (st[top].col == j && sudoku[i][j].sol == 0) {
				if (sudoku[i][j].candidate[pos] == 0 && sudoku[i][j].count != 0) {
					sudoku[i][j].candidate[pos] = 1;
					sudoku[i][j].count--;
				}
			}
		}
	}
}
void box_cross_out(cell sudoku[][9], hint_cell st[81], int top) {
	int pos = st[top].sol - 1;
	for (int i = 0; i < 9; i++) {
		for (int j = 0; j < 9; j++) {
			if (st[top].box == sudoku[i][j].box && sudoku[i][j].sol == 0) {
				if (sudoku[i][j].candidate[pos] == 0 && sudoku[i][j].count != 0) {
					sudoku[i][j].candidate[pos] = 1;
					sudoku[i][j].count--;
				}
			}
		}
	}
}
void cross_out(cell sudoku[][9], hint_cell st[81], int top) {
	row_cross_out(sudoku, st, top);
	col_cross_out(sudoku, st, top);
	box_cross_out(sudoku, st, top);
}

void sum_row(cell sudoku[][9], hint_cell st[81], int& top, int& total_sol) {
	for (int i = 0; i < 9; i++) {
		int sum_row[9] = { 0 };		
		int sum_count=0;
		for (int k = 0; k < 9; k++) {
			for (int j =0; j<9; j++) {
				sum_row[k] = sum_row[k] + sudoku[i][j].candidate[k];
				sum_count++;	
			}
			if (sum_row[k] == 8 && sum_count ==9) {		
				for (int l = 0; l < 9; l++) {
					if (sudoku[i][l].candidate[k] == 0&&sudoku[i][l].sol ==0) {
 						sudoku[i][l].sol = k + 1;
						for (int m = 0; m < 9; m++) {
							sudoku[i][l].candidate[m] = 1;
					    }
						sudoku[i][l].count = 0;
						st[top] = { k + 1, i ,l, sudoku[i][l].box };
						top++;
						total_sol++;
						pop(st, top);
						cross_out(sudoku, st, top);	
					}
				}
			}
			sum_count = 0;
		}
	}
}
void sum_col(cell sudoku[][9], hint_cell st[81], int& top, int& total_sol) {

	for (int i = 0; i < 9; i++) {
		int sum_col[9] = { 0 };
		int sum_count = 0;
		for (int k = 0; k < 9; k++) {
			for (int j = 0; j < 9; j++) {
				sum_col[k] = sum_col[k] + sudoku[j][i].candidate[k];
				sum_count++;	
			}
			if (sum_col[k] == 8 && sum_count == 9) {
				for (int l = 0;l < 9; l++) {
					if (sudoku[l][i].candidate[k] == 0&&sudoku[l][i].sol==0) {
						sudoku[l][i].sol = k + 1;
						for (int m = 0; m < 9; m++) {
							sudoku[l][i].candidate[m] = 1;
						}
						sudoku[l][i].count = 0;
						st[top] = { k + 1, l ,i, sudoku[l][i].box };
							top++;
							total_sol++;
							pop(st, top);
							cross_out(sudoku, st, top);
					}
				}
			}
			sum_count = 0;
		}
	}
}
void sum_box(cell sudoku[][9], hint_cell st[81], int& top, int& total_sol) {
	for (int l = 0; l < 9; l++) {
		int sum_box[9] = { 0 };
		int sum_count = 0;
		for (int i = 0; i < 9; i++) {
			for (int j = 0; j < 9; j++) {
				if (sudoku[i][j].box == l + 1) {
					for (int k = 0; k < 9; k++) {
						sum_box[k] = sum_box[k] + sudoku[i][j].candidate[k];
						sum_count++;
					}
				}
			}
			if (sum_box[i] == 8 && sum_count == 9) {
				for (int j = 0; j < 9; j++) {
					for (int k = 0; k < 9; k++) {
						if (sudoku[j][k].box == l + 1 && sudoku[j][k].candidate[i] == 0) {
							sudoku[j][k].sol = i + 1;
							for (int m = 0; m < 9; m++) {
								sudoku[j][k].candidate[m] = 1;
							}
							sudoku[j][k].count = 0;
							st[top] = {i + 1, j ,k, sudoku[j][k].box };
							top++;
							total_sol++;
							pop(st, top);
							cross_out(sudoku, st, top);
						}
					}
				}
			}
			sum_count = 0;
		}
	}
}
void find_hidden(cell sudoku[][9], hint_cell st[81], int& top, int& total_sol) {
	sum_row(sudoku, st, top, total_sol);
	sum_col(sudoku, st, top, total_sol);
	sum_box(sudoku, st, top, total_sol);
}

void print(cell sudoku[][9]) {
	for (int i = 0; i < 9; i++) {
		for (int j = 0; j < 9; j++) {
			cout << sudoku[i][j].sol << " ";

			if (j % 9 == 8) {
				cout << endl;
			}
		}
	}
}

int main() {
	cell sudoku[9][9] = { {{{0},0,9,1}, {{0},0,9,1}, {{0},0,9,1}, {{0},0,9,2}, {{1,1,1,1,1,1,1,1,1},8,0,2}, {{1,1,1,1,1,1,1,1,1},3,0,2}, {{0},0,9,3}, {{0},0,9,3}, {{0},0,9,3}}, {{{1,1,1,1,1,1,1,1,1},7,0,1}, {{0},0,9,1}, {{0},0,9,1}, {{0},0,9,2}, {{1,1,1,1,1,1,1,1,1},6,0,2}, {{0},0,9,2}, {{0},0,9,3}, {{1,1,1,1,1,1,1,1,1},9,0,3}, {{1,1,1,1,1,1,1,1,1},8,0,3}},{{{1,1,1,1,1,1,1,1,1},9,0,1},{{0},0,9,1},{{1,1,1,1,1,1,1,1,1},8,0,1},{{1,1,1,1,1,1,1,1,1},7,0,2}, {{0},0,9,2}, {{0},0,9,2}, {{0},0,9,3}, {{0},0,9,3}, {{0},0,9,3}}, {{{0},0,9,4}, {{0},0,9,4}, {{1,1,1,1,1,1,1,1,1},1,0,4}, {{1,1,1,1,1,1,1,1,1},3,0,5}, {{0},0,9,5}, {{0},0,9,5}, {{1,1,1,1,1,1,1,1,1},8,0,6}, {{0},0,9,6},{{0},0,9,6}},{{{1,1,1,1,1,1,1,1,1},5,0,4},{{1,1,1,1,1,1,1,1,1},9,0,4},{{0},0,9,4}, {{1,1,1,1,1,1,1,1,1},1,0,5}, {{1,1,1,1,1,1,1,1,1},7,0,5}, {{1,1,1,1,1,1,1,1,1},8,0,5}, {{0},0,9,6}, {{1,1,1,1,1,1,1,1,1},3,0,6}, {{1,1,1,1,1,1,1,1,1},4,0,6}}, {{{0},0,9,4}, {{0},0,9,4}, {{1,1,1,1,1,1,1,1,1},3,0,4}, {{0},0,9,5}, {{0},0,9,5}, {{1,1,1,1,1,1,1,1,1},2,0,5}, {{1,1,1,1,1,1,1,1,1},9,0,6}, {{0},0,9,6}, {{0},0,9,6}},{{{0},0,9,7}, {{0},0,9,7}, {{0},0,9,7}, {{0},0,9,8}, {{0},0,9,8}, {{1,1,1,1,1,1,1,1,1},7,0,8}, {{1,1,1,1,1,1,1,1,1},4,0,9}, {{0},0,9,9}, {{1,1,1,1,1,1,1,1,1},2,0,9}},{{{1,1,1,1,1,1,1,1,1},8,0,7},{{1,1,1,1,1,1,1,1,1},2,0,7}, {{0},0,9,7}, {{0},0,9,8}, {{1,1,1,1,1,1,1,1,1},9,0,8}, {{0},0,9,8}, {{0},0,9,9}, {{0},0,9,9}, {{1,1,1,1,1,1,1,1,1},7,0,9}},{{{0},0,9,7}, {{0},0,9,7}, {{0},0,9,7}, {{1,1,1,1,1,1,1,1,1},5,0,8}, {{1,1,1,1,1,1,1,1,1},2,0,8}, {{0},0,9,8}, {{0},0,9,9}, {{0},0,9,9}, {{0},0,9,9}} };

	hint_cell st[81] = { {2,8,4,8}, {5,8,3,8}, {7,7,8,9}, {9,7,4,8}, {2,7,1,7}, {8,7,0,7}, {2,6,8,9}, {4,6,6,9}, {7,6,5,8}, {9,5,6,6}, {2,5,5,5}, {3,5,2,4}, {4,4,8,6}, {3,4,7,6}, {8,4,5,5}, {7,4,4,5}, {1,4,3,5}, {9,4,1,4}, {5,4,0,4}, {8,3,6,6}, {3,3,3,5}, {1,3,2,4}, {7,2,3,2}, {8,2,2,1}, {9,2,0,1}, {8,1,8,3}, {9,1,7,3}, {6,1,4,2}, {7,1,0,1}, {3,0,5,2} , {8,0,4,2} };
	int top = 31;
	int total_sol = 31;

	cout << "problem : " << endl << endl;
	print(sudoku);                                                   
	cout << endl << endl << "soluiton : " << endl << endl;

	while (total_sol != 81) {
		while (top > 0) {
			pop(st, top);
			cross_out(sudoku, st, top);
			push(sudoku, st, top, total_sol);
		}
		find_hidden(sudoku, st, top, total_sol);
	}
	print(sudoku);                               
	return 0;
}

```

### 출력 결과

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/c855943c-0c6a-4ac6-a7fe-611f8a6dafdb)

### 소감문

제 스도쿠 프로그램의 원리를 설명하자면 이렇습니다.
먼저, 힌트 스택의 top에 있는 힌트와 sudoku cell i행을 비교했을 때 같다는 조건이 성립하면 비어있는 셀의 캔디데이트(힌트 셀의 솔루션 자리)에 0 대신 1을 대입해 주고 카운트를 하나 줄여줍니다. 
행, 열, 박스를 모두 이런 식으로 크로스 아웃하고 나면, 카운트가 1인 셀을 찾아 캔디데이트가 유일하게 0인 자리의 위치를 출력하여 해당 셀의 솔루션을 입력해줍니다. 
Naked single을 찾아도 스도쿠가 해결되지 않을 경우를 대비하여 전체 반복문의 조건을 total_sol != 81으로 걸어주었습니다. Naked single 만으로 풀리지 않는 경우에는 히든 싱글을 찾아내는 과정을 거칩니다. 
Sum_row 라는 새로운 배열을 만들어 row마다 같은 row의 candidate끼리 더해주었고, 솔루션이 있는 셀의 캔디데이트는 1, 그렇지 않은 셀의 캔디데이트는 0이므로, hidden single의 정의에 의해 sum_row의 값이 8인 위치가 sol이 됩니다. 
Sol을 발견했다면, 같은 row 내에서 sol의 candidate가 0인 셀을 찾아 sol을 입력해주고 힌트 셀로 등록시킨 뒤, 다시 cross out 해줍니다. Cross out과 naked single 함수의 반복조건은 힌트 스택의 top이 0보다 클 동안입니다.

## 2023-09-25

히든 싱글은 해당 셀이 속해 있는 행, 열 또는 박스 내에서 유일하게 하나의 솔루션을 갖고 있는 경우를 말한다. 예를 들어 다음의 그림에서 선택된 영역에서 1을 후보로 가지고 있는 셀은 하나 밖에 없다. 따라서 해당 셀의 솔루션은 1이 된다.
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/80d424b5-9961-4d78-a655-d88779712a96) [참고](https://sudoku.com/ko/seudoku-gyuchig/sum-eun-sing-geul/)

지금 다시 보니 불필요한 부분이 많고 직관적으로 이해가 잘 되지 않아서 다시 작성해 아래에 추가할 예정. 

