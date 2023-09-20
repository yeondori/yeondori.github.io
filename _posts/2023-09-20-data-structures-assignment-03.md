---
title: "[과제] 03 Structure, Binary Search, 2차원 배열"

excerpt: "데이터 구조론 과제"
categories: [ERICA, Data Structures]
tags: [C++, Data Structures]
date: 2023-09-20
last_modified_at: 2023-09-20
render_with_liquid: false
---

2주차 과제는 2개였는데 3주차 과제는 무려 3개다.

- **과제1**: Structure
- **과제2**: Binary Search
- **과제3**: 2차원 배열의 행,열, 총합 구하기

# Structure, Binary Search, 2차원 배열

## 2019-09-22

### 프로그램 리스트

#### **과제1** Structure

```cpp
#include<iostream>
#include<string>
using namespace std;

struct Student {
  int age;
  string name;
};


int main() {
	Student s[10] = { {10,"Kim"},{50,"Lee"},{55, "Wu"},{100,"Park"},{20,"Cha"},{40,"Dol"},{70, "Sang"},{101,"Choi"}, {58,"Dal"}, {3,"Sal"} };

	for (int i = 0; i < 10; i++) {
		if (s[i].age == 70){
			cout << "나이가 70인 사람의 이름은 " << s[i].name << "입니다. ";
		}
	}

	return 0;
}

```
#### **과제2** Binary Search

```cpp
#include<iostream>
using namespace std;

int main() {
	int a[7] = { 10, 20, 30, 40, 50, 70, 100 };
	int first = 0;
	int last = 6;
	int middle = (last - first) / 2;

	for (int i=0; i<7; i++) {
	  if (a[i] < 70) { 
	    int first = i + 1;
	    int middle = (last + first) / 2;
	  }
	  else if (a[i] > 70) {
		int last = i - 1;
		int middle = (last - first) / 2;
	  }
	  else
	    cout << "70의 위치는 " << i << "째 입니다." << endl;
    }
    return 0;
}
```
#### **과제3** 2차원 배열의 행,열, 총합 구하기

```cpp
#include<iostream>
using namespace std;

int total_row(int a[3][4]);
int total_column(int a[3][4]);
int total(int a[3][4]);
int total_row(int a[3][4]) {
    int sum_row;
	for (int i = 0; i < 3; i++) {
	    for (int j = 0; j < 4; j++) {
		    sum_row += a[i][j];
		}
		cout << i << "째 row 합 : " << sum_row << endl << endl;
		sum_row = 0;
	}
	return sum_row;
}

int total_column(int a[3][4]) {
    int sum_column;
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 3; j++) {
		    sum_column += a[j][i];
		}
		cout << i << "째 column 합 : " << sum_column << endl << endl;
		sum_column = 0;
	}
	return sum_column;
}

int total(int a[3][4]) {
    int total;
	for (int j = 0; j < 4; j++) {
		for (int i = 0; i < 3; i++) {
		    total += a[i][j];
		}
	}
	cout << "총합 : " << total << endl;
	return total;
}

int main()  {
	int a[3][4] = { {10, 20, 30, 50}, {1,2,3,7}, {50,70,80,40} };
	total_row(a);
	total_column(a);
	total(a);
	return 0;
}

```
### 소감문

이번 주는 과제가 3개인 데다가 축제 기간과도 겹쳐서 매우 힘든 한 주였습니다. 과제를 미루고 미루다 보니 토요일 아르바이트가 끝난 뒤부터 겨우 하기 시작했습니다. 과제는 지난주 과제였던 나이와 이름 배열에 관련된 문제를 구조체를 이용하여 푸는 것이 첫째, Binary Search를 first와 last를 이용하여 푸는 것이 둘째, 2차원 배열에서 행의 총합, 열의 총합, 그리고 배열의 총합을 구하는 것이 셋째 과제였습니다. 구조체를 이용하면 두 개의 별개로 나뉘어 있던 배열도 합쳐서 표현할 수 있으며, 따라서 나이, 이름, 전화번호 등과 같이 개인 혹은 사물별로 정보를 분류할 때 매우 편리하다는 것을 알게 되었습니다.

지난주에 Binary Search를 처음 배웠을 때, 알고리즘을 생각하는 데 매우 오랜 시간이 걸렸습니다. 인터넷이나 친구, 선배의 도움 없이 알고리즘을 완성했기 때문에 굉장히 뿌듯했는데, 제 알고리즘에는 시작점과 끝점을 지정하지 못한다는 한계가 있었습니다. 수업을 바탕으로 과제를 하면서, 좀 더 편리하고 정확한 방법이 있음을 알게 되었던 것 같습니다.

가장 흥미로웠던 것은 2차원 배열이었습니다. 앞의 두 과제는 지난주 수업의 복습이나 보충이었던 반면, 2차원 배열은 처음 접해보는 개념이었습니다. 솔직히 말하면, 수업이 3시간 연속 강의기 때문에 수업이 끝나갈 때 즈음 집중력이 떨어지고 많이 힘듭니다. 2차원 배열을 설명해주실 땐 집중력이 매우 떨어져 있는 상황이어서 도대체 왜 반복을 두 번 해야 하는지 이해가 가지 않았습니다. 심지어 과제를 너무 늦게 시작했고 남은 시간이 별로 없었기 때문에 너무 초조해서 충분한 시간을 갖지 않고 코딩을 하려다가 계속 실패했습니다. 포기하고 어떻게든 도움을 받아보려다가 자존심과 양심이 용납하지 않아서 천천히 다시 생각하기 시작했습니다. 스스로 문제를 풀어 엄청난 성취감을 느꼈지만, 솔직히 말해서 완성한 과제가 마음에 들지는 않습니다. 분명 더 간결한 방법이 있을 거라는 생각이 과제를 완성하면서도 계속 들었기 때문입니다. 그래서 아쉬운 점이 많지만 다음 주 수업 시간에 보충할 수 있을 것이니 저는 마음 편히 자도록 하겠습니다. 앞으로는 과제를 밀리지 않고 제때제때 하겠습니다.

## 2023-09-20

과제1은 2주차 과제에서 2개의 배열로 다루었던 정보들을 구조체로 해결하는 문제였다. 열심히 unordered_map 찾아보고 만들 필요 없이 구조체에 담으면 정말 아주 간결하게 코드를 짤 수 있었구나... 3주차 과제까지 보고 나서 고쳐볼 걸 싶긴 하지만 그래도 많이 배웠다..!

과제2는 first, last, middle 변수는 아무런 역할을 하지 못하고 있고 사실상 binary search가 아니라 단순하게 for문에서 차례대로 탐색하고 있는데 왜 아무런 피드백을 못 받았지..? 소감문에서 제 알고리즘엔 시작점과 끝점을 지정하지 못한다고 했던 게 이 말이었구나 싶다.

우선 **Binary Search(이진 탐색)** 는 리스트에서 검색 범위를 줄여 나가면서 검색 값을 찾는 알고리즘이다. 배열의 중앙값과 탐색하고자 하는 값을 비교하는 알고리즘이기 때문에 정렬된 리스트에만 사용할 수 있다는 단점이 있지만 검색이 반복될 때마다 검색 범위가 절반으로 줄기 때문에 속도가 빠르다는 장점이 있다.

탐색 과정은 다음과 같다. [Image](https://en.wikipedia.org/wiki/Binary_search_algorithm)
![img.png](img.png)

배열에서 찾고자 하는 값이 7인 경우를 예로 들면,

1. 배열의 중앙에 있는 값(14)과 탐색하고자 하는 값(7)을 비교한다. 탐색의 시작점과 끝점은 각각 1, 71이다. (1,71)
2. 값(7) < 중앙값(14)이기 때문에 찾고자 하는 값은 중앙값보다 왼쪽에 있을 것이므로(리스트는 오름차순으로 정렬되어 있음), 중앙값의 왼쪽 부분만 탐색한다.(1,13)
3. (1~13)의 중앙에 있는 값(6)과 탐색하고자 하는 값(7)을 비교한다.
4. 값(7) > 중앙값(6)이기 때문에 찾고자 하는 값은 중앙값보다 오른쪽에 있을 것이므로 중앙값의 오른쪽 부분만 탐색한다. (7,13)
5. (7,13)의 중앙에 있는 값(8)과 탐색하고자 하는 값(7)을 비교한다.
6. 값(7) < 중앙값(8)이므로 탐색 범위 (7,7)
7. 탐색은 시작점이 끝점보다 커질 때까지 반복한다. (탐색값이 없을 경우도 존재하므로)

이렇듯 이분탐색은 중앙값을 기준으로 절반씩 버려지기 때문에 시간 복잡도는 log_2(n)이다. 이떄 중앙값은 배열의 길이가 짝수이든 홀수이든 결과와는 무관하다. [참고](https://charles098.tistory.com/133)

다시 구현해보았다. size 값을 가져오기 불편해서 vector를 사용했는데 아직도 어떤 자료구조를 선택해야 하는 건지는 어렵다.. 사실 이 정도 문제에선 array 그대로 사용해도 될 것 같긴 하다.
함수로 탐색 부분과 출력 부분을 분리해봤고, 찾고자 하는 x값도 직접 입력받도록 했으며 값이 존재하지 않는 경우도 구현해 두었다.
```cpp
#include<iostream>
#include <vector>
using namespace std;

int binary_search(vector<int> a, int x) {
  int start = 0;
  int end = a.size()-1;
  int mid = (start+end)/2;
  
  bool find = false;
  int idx;
  
  while(start<=end) {
    if (x<a[mid]) {
      end = mid;
      mid = (start+end)/2;
    }
    else if (x>a[mid]){
      start = mid+1;
      mid = (start+end)/2;
    }
    else {
      idx = mid;
      find = true;
      break;
    }
  }
  if (!find) idx = -1;
  return idx;
}

void print(int x, int idx) {
  if (idx != -1) {
    cout << x << "의 위치는 " << idx+1 << "번째 입니다." << endl;
  } else cout << x << "는 존재하지 않는 값입니다." << endl;
}

int main() {
	vector<int> a = { 10, 20, 30, 40, 50, 70, 100 };

  int x;
  cout << "탐색하고자 하는 값을 입력하세요." << endl;
  cin >> x;
  
  int idx = binary_search(a,x);
  print(x, idx);

  
  return 0;
}
```


mid 부분이 불필요하게 중복되고 있고 bool도 필요하지 않을 것 같아서 좀 더 다듬어 보았다!

```cpp
#include<iostream>
#include <vector>
using namespace std;

int binary_search(vector<int> a, int x) {
int start = 0;
int end = a.size()-1;

int idx = -1;

while(start<=end) {
int mid = (start+end)/2;
if (x<a[mid]) {
end = mid;
}
else if (x>a[mid]){
start = mid+1;
}
else {
idx = mid;
break;
}
}
return idx;
}

void print(int x, int idx) {
if (idx != -1) {
cout << x << "의 위치는 " << idx+1 << "번째 입니다." << endl;
} else cout << x << "는 존재하지 않는 값입니다." << endl;
}

int main() {
vector<int> a = { 10, 20, 30, 40, 50, 70, 100 };

int x;
cout << "탐색하고자 하는 값을 입력하세요." << endl;
cin >> x;

int idx = binary_search(a,x);
print(x, idx);


return 0;
}
```
간결하고 깔끔하게 코드를 작성하는 건 아직도 너무너무 어려운 것 같다.
과제 3은 나중에 나올 스도쿠에서 실컷 고칠 것 같으니까 우선 넘어가야겠다.

--- 

### ++ 
vector vs list [참고](https://sueaty.tistory.com/59)

vector 사용법 [참고](https://hwan-shell.tistory.com/119)