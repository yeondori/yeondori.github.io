---
title: "[데이터 구조론][과제] 04 Structure, Selection sort, Push & Pop"

excerpt: "데이터 구조론 과제"
categories: [ERICA, Data Structures]
tags: [C++, Data Structures]
date: 2023-09-21
last_modified_at: 2023-09-22
render_with_liquid: false
---
- **과제1**: Structure
- **과제2**: Selection sort
- **과제3**: Push & Pop

# Structure, Binary Search, 2차원 배열

## 2019-09-27

### 프로그램 리스트

#### **과제1** Structure

```cpp
#include<iostream>
using namespace std;

struct age_name {
    int age;
    string name;
};

struct student {
	age_name a_n;
	int score[4];
	double ave;
};

double find_ave(student a[], int size);

double find_ave(student a[], int size)  {
	double sum = 0;
	double totalsum = 0;

	for (int i = 0; i < size; i++)  {
		for (int j = 0; j < 4; j++) {
		    sum = sum + a[i].score[j];
		}
		a[i].ave = sum/4;
		totalsum = totalsum + sum;
		sum = 0;
	}

	return totalsum/5;
}

int main() {
	student a[5] = { { {20,"Kim"},{90,70,80,90},0 },
					 {{30,"Lee"},{90,90,80,100},0 },
					 {{25,"Park"},{70,100,80,90},0},
					  {{21,"Cha"},{60,100,80,50},0},
					{{20,"Yang"},{70,90,80,60},0} };
	double total_ave = find_ave(a, 5) / 4;
	cout << "전체 평균 : " << total_ave << endl;

	for (int i = 0; i < 5; i++) {
		if (a[i].a_n.name == "Park")    {
			cout << "Park의 성적 : ";
			for (int j = 0; j < 4; j++) {
				cout << a[i].score[j] << " ";
			}
			cout << endl << "평균 : " << a[i].ave;
		}
	}
	return 0;
}

```

#### **과제2** Selection sort

```cpp
#include<iostream>
using namespace std;

void selection_sort(int a[], int size) {
	int i = 0;
	int j = 0;
	int position = 0;
	int first = 0;

	for (int j= first; j < size; j++)   {
		int min = a[first];
		for (i = first ; i < size; i++) {
			if (a[i] < min) {
				min = a[i];
				position = i;
			}
			else if (a[i] == min)   {
				min = a[i];
				position = i;
			}
		}
		cout << first << "회차 배열의 최솟값 : " << min << " 이전 위치 : " << position << "째" << endl;
		int temp;
		temp = a[first];
		a[first] = min;
		a[position] = temp;
		first = first + 1;
	}
}

void print(int a[]) {
	for (int k = 0; k < 8; k++) {
		cout << a[k] << endl ;
	}
}

int main()  {
	int a[8] = { 10, 5, 20, 30, 100,1,15,25 };
	selection_sort(a,8);
	cout << endl << "정렬된 배열 : " << endl;
	print(a);
	return 0;
}
```

#### **과제3** Push & Pop

```cpp
#include<iostream>
using namespace std;

void push(int a[], int x,int& top)  {
	a[top] = x;
	top++;
}

void pop(int a[], int& top) {
	top--;
}

int main()  {
	int top = 0;
	int a[5];
	push(a, 10, top);
	push(a, 20, top);
	pop(a,top);
	push(a, 20,top);
	push(a, 50,top);
	pop(a,top);
	pop(a,top);
	push(a, 100,top);

	for (int i = 0; i < top; i++)   {
		cout << a[i] << endl;
	}
	return 0;
}

```

### 소감문

이번 주는 구조체를 이용하여 학생의 성적 4개의 평균을 각각 구하고, 전체 학생의 평균을 구하는 것이 첫째, 배열 중 최솟값을 찾아 swap을 이용하여 순차적으로 정렬(sort)하는 것이 둘째, stack에서 push 함수와 pop 함수를 작성하는 것이 셋째 과제였습니다.

첫번째 과제는 교수님께서 많이 가르쳐 주셔서 금방 끝날 것 같았지만, main 함수 안의 ave 값을 어떻게 변화시켜주어야 하는지에 대해 고민이 많았고, 실제로 코딩에 어려움이 있었습니다. 또 sum값을 리턴하는 것에도 어려움이 있었습니다. 고민 끝에 totalsum 이라는 변수를 하나 더 지정해주었고, 문제를 해결할 수 있었습니다.

두번째 과제는 시간이 가장 오래 걸렸습니다. 작성을 완료하는 데까지 4시간은 족히 걸렸던 것 같습니다. 알고리즘을 구현하는 것까지는 그다지 오랜 시간이 걸리지 않았지만, 코딩에서 계속 오류가 발생했습니다. 분명히 temp를 이용하여 값을 바꾸어주었음에도 불구하고, 몇몇 부분에서 값이 덮어씌워져 있었습니다. 제가 작성해놓은 코드를 보며 제 자신이 컴퓨터라고 생각하면서 일일이 과정을 전개해보았더니 최솟값 min과 a[i]가 같은 경우를 지정해 주지 않아서 생긴 오류임을 깨달았고, 수정한 뒤에 원하는 결과를 얻을 수 있었습니다. 시간은 가장 오래 걸렸지만, 하고 나서 매우 뿌듯했습니다.
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/f7e3e275-3daa-43a0-9b95-2826d1a12944)

세번째 과제는 생각보다 쉽게 끝낼 수 있었습니다. 어려운 점이 있었다면, 제가 작성한 코드는 push 함수를 호출할 때는 원래 값에 새 값을 덮어 씌우고, pop 함수를 호출할 때는 단순히 top을 하나 줄이는 것이기 때문에 값이 지워지지 않았던 것입니다. pop함수 안에 a[top] = NULL을 추가해야하나 고민했으나 그것도 옳은 방법은 아닐 것이라는 생각이 들었습니다. 고민하던 중에 top은 stack의 최상단 부분을 가르키는 값임을 떠올렸고, print할 때 i<top 으로 범위를 설정함으로써, 문제를 해결할 수 있었습니다.
이번 주는 인천 국제기계전도 다녀와야 하고, 아르바이트도 3일 연속으로 해야해서 과제를 할 시간이 매우 적었습니다. 계획한 시간 내에 스스로 과제를 완성했다는 것에 제 자신이 매우 자랑스럽습니다. 누가 과제가 갈수록 쉬워진다고 했다던데 저는 절대 그렇게 생각하지 않습니다. 정말 매주 뇌가 과부하가 걸린 느낌입니다. 스도쿠를 제가 생각해낼 수 있을지는 모르겠지만, 남은 시간동안은 스도쿠에 대해 생각해보고, 지금까지 과제를 복습할 예정입니다.

## 2023-09-21

매주 뇌에 과부하 ㅋㅋㅋㅋㅋㅋㅋㅋㅋ 어떻게 한 주도 빠짐없이 어렵고 힘들다는 말을 꼬박꼬박 했을까? 어차피 할 거였으면서... 저렇게 말 안하면 교수님께서 더 어렵게 과제 내주실까봐 일부러 더 힘들다고 어필한 것도 사실 있던 것 같다.
이제는 최대 난관이었던 스도쿠 과제에는 도대체 뭐라고 소감 썼을 지 궁금하고 기대되는 지경이다.

먼저 과제 1의 경우에는 student 구조체 내부에 나이와 이름을 가지고 있는 age_name 이라는 구조체가 들어가 있는 중첩 구조체의 형태이다. 이를 이용해 개별 학생의 성적과 전체 학생의 성적을 조회할 수 있어야 하는데, 사실상 구조체 내부의 average 변수를 활용하고 있지 않고 있다. 
전체 평균을 계산하기 전에 개별 학생의 성적 평균값을 먼저 계산해 average 변수에 넣어주는 함수를 생성했고, 전체 학생의 평균을 계산하는 함수와 출력 함수들도 따로 만들어 주었으며 조회하고자 하는 학생을 직접 입력받을 수 있도록 코드를 변경해보았다. 
구조체와 포인터 개념이 부실해서 생각보다 시행착오가 많았고, 특히나 구조체 배열을 어떻게 함수 인자로 넘겨주어야 하는지도 어려웠다. 사실 지금도 너무 지저분해서 추후에 수정을 다시 해봐야할 것 같다. 

```cpp
#include<iostream>
#include<vector>
using namespace std;

const int studentNum = 5;

struct AgeName {
    int age;
    string name;
};

struct Student {
	AgeName ageName;
	int score[4];
	double average;
};

//특정 학생 평균
void calculateOneAverage(Student *student) {
  
  int subjectNum = 0;
  double totalScore = 0;
  
  for(int score: student->score) {
    totalScore += score;
    subjectNum++;
  }
  student->average = totalScore/subjectNum;
}

//전체 학생 평균
double calculateEveryAverage(struct Student* students, int studentNum){
  
  double averageSum = 0;

  for (int i=0; i<studentNum; i++) {
    averageSum += students[i].average;
  }
  return averageSum/studentNum;
}

//score 출력하기
void printScore(string name, int* score, int size){
  cout << name << "의 성적: ";
  for(int i=0; i<size; i++) {
    cout << score[i] << " " ;  
  }
  cout << endl;
}

//개별 학생의 성적과 평균 출력
void printOneScoreInfo(struct Student* students, int studentNum, string name) {
  bool exist = false;

  for (int i=0; i<studentNum; i++) {
    if(name == students[i].ageName.name) {
      int size = sizeof(students[i].score)/sizeof(int);
      printScore(name, students[i].score, size);
      
      cout << name << "의 평균: " << students[i].average << endl;
      exist = true;
    }
  }
  if(!exist) cout << "존재하지 않는 학생입니다." << endl;
}



int main() {
	Student s[studentNum] = { { {20,"Kim"},{90,70,80,90},0 },
					 {{30,"Lee"},{90,90,80,100},0 },
					 {{25,"Park"},{70,100,80,90},0},
					  {{21,"Cha"},{60,100,80,50},0},
					{{20,"Yang"},{70,90,80,60},0} };


  
  for (Student &student: s) {
    calculateOneAverage(&student);
  }

  int totalAverage = calculateEveryAverage(s, studentNum);  
  cout << "전체 평균: " << totalAverage<< endl;
  
  cout << "조회하고자 하는 학생의 이름을 입력하세요.";
  string name;
  cin >> name;

  printOneScoreInfo(s, studentNum, name);
}
```
과제 2의 Selection Sort는 배열 내의 최솟값을 찾아 배열의 첫번째 값과 교환하고, 교환된 첫번째 값을 제외한 나머지 배열에서 이를 반복하는 식으로 정렬하는 방식이다. 사실 과제2는 크게 달라진 부분이 없었고 코드를 좀 더 간결하게 줄여보려고 해도 이게 최선이었다... 그리고 굉장히 비효율적이다.

```cpp
#include<iostream>
using namespace std;

void selection_sort(int a[], int size);
void print(int* a, int size);

int main()  {
const int size = 8;
int a[size] = { 10, 5, 20, 30, 100,1,15,25 };
cout << "기존 배열: ";
print(a,size);

selection_sort(a,size);

cout << endl << "정렬된 배열 : " << endl;
print(a,size);

return 0;
}

void selection_sort(int a[], int size) {

for (int i=0; i<size; i++) {
int min = 2147483647;
int pos = -1;

    for (int j=i; j<size; j++) {
      if(a[j]<min) {
        min = a[j];
        pos = j;
      }
    }
    cout << "["<< i << "회차] 배열의 최솟값: " << min << ", 위치: " << pos << endl;
    int temp = a[i];
    a[i] = min;
    a[pos] = temp;

}
}

void print(int* a, int size) {
for (int k = 0; k < size; k++) {
cout << a[k] << " ";
}
cout << endl;
}
```

과제3의 스택을 라이브러리 없이 구현하는 것은 별도의 글로 정리해 올려야겠다.
