---
title: "[데이터 구조론][과제] 09 STL - Sort, Unique"

excerpt: "데이터 구조론 과제"
categories: [ERICA, Data Structures]
tags: [C++, Data Structures]
date: 2023-10-04
last_modified_at: 2023-10-04
render_with_liquid: false
---
# 08 STL - Sort, Unique

## 2019-11-16

### 프로그램 리스트

#### STL 사용
```cpp
#include<iostream>
#include<list>
#include<algorithm>
#include<iterator>

using namespace std;

int main() {
    int a[8] = { 10, 100, 200, 10, 30, 10, 40, 10};
    cout << "sort 후 배열 : ";
    sort(a, a + 8);
    copy(a, a + 8, ostream_iterator<int>(cout, " "));
    cout << endl << "unique 후 배열 : ";
    int* p = unique(a, a + 8);
    copy(a, p, ostream_iterator<int>(cout, " "));
    cout << endl;
return 0;
}
```

#### STL 미사용

```cpp
#include<iostream>
using namespace std;

void swap(int a[], int j)
{
	int temp = a[j];
	a[j] = a[j + 1];
	a[j + 1] = temp;
}

void BubbleSort(int a[], int size)
{
	int last = size;
	for (int i = 0; i < size; i++)
	{
		for (int j = 0; j < last - 1; j++)
		{
			if (a[j] > a[j + 1])
			{
				swap(a, j);
			}
		}
		last--;
	}
}

void print(int a[], int size)
{
	cout << "배열 " << endl;
	for (int i = 0; i < size; i++)
	{
		cout << a[i] << " " ;
	}
}

void unique(int a[], int size)
{
	int rep = 0;
	int last = size;
	int end = a[size];
	
		for (int i = 0; i < last; i++)
		{
			rep = a[i];
			for (int j = i + 1; j < last; j++)
			{
				if (rep == a[j])
				{
					for (int k = j; k < last; k++)
					{
						a[k] = a[k+1];
					}
					last--;
					i--;
				}
			}
		}
		print(a, last);
}

int main()
{
	int a[8] = { 10, 100, 200, 10, 30, 10, 40, 10 };
	print(a, 8);
	BubbleSort(a, 8);
	cout << endl << "Bubble Sort 이후 ";
	print(a, 8);
	cout << endl << "Unique 이후 ";
	unique(a, 8);
	return 0;
}

```
### 소감문

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/c1b4767c-5a66-481f-b71c-6cb8f4539856)

이번주 과제는 unique. 즉, 배열 내에 같은 원소가 존재한다면 중복되는 원소를 모두 제거하는 함수를 STL을 이용한 방식과 그렇지 않은 방식으로 만들어내는 것이었습니다. 
위의 사진은 제가 알고리즘을 구상할 때 쓴 종이입니다. STL을 이용하면 손쉽게 17줄만으로 원하는 결과값을 얻을 수 있으나, 직접 알고리즘을 구상하는 경우에는 쉽지 않았습니다. 
Unique를 하기 위해서는 우선 sort를 해야하기 때문에 2주 전 과제인 Bubble sort를 이용하여 배열을 정렬해줍니다. 배열 내의 중복되는 값을 찾아내기 위하여 rep라는 변수를 따로 지정해주었습니다. 
Rep와 a[i]가 같으면, 배열을 a[i]부터 마지막인 last까지 하나씩 앞으로 밀어서 써줍니다. 이때, last는 쓰레기 값이 되므로 last를 – 해줍니다. 
또, 중복되는 값이 하나 이상일 수 있으므로 a[i]는 ++되어서는 안됩니다. 따라서 a -를 해줍니다. 
마지막으로, print할 때의 size는 last로 지정해주면 중복되는 원소는 사라진 배열이 나오게 됩니다. 이렇게 코드를 구상했을 때는 총 71줄이 나왔습니다. 
이번주 과제 정도의 문제에서 직접 알고리즘을 구상하는 데는 큰 무리가 없지만, 훨씬 더 복잡한 문제를 해결해야 한다면 너무 비효율적일 것이라는 생각이 들었습니다. 
STL을 열심히 배워서 잘 활용해 보고싶다고 생각했습니다.

## 2023-10-04

스도쿠 이후로는 STL 진도를 나갔었는데 과제 난이도가 비교적 쉬워져서 좀 더 수월하게 과제를 했던 기억이 있다.

8번째 과제였던 Bubble Sort는 파일이 날라가서 찾지 못했는데 이번 과제에 코드가 아직 남아있어서 다행이다... 참고로 Bubble Sort는 순차적으로 배열 내에 있는 두 값을 비교하며 더 큰 값을 뒤로 보내는 방식의 정렬 방법이다. 
첫번째 탐색이 끝나면 가장 큰 값이 배열의 끝으로 오기 때문에 두번째 탐색을 할 때는 배열의 마지막 값은 비교하지 않아도 되고, 탐색이 수행될 때마다 탐색해야 할 값은 하나씩 줄게 되는 특징이 있다.

Bubble Sort 보다는 Unique 부분에서 불필요하게 for문이 중첩되어 있는 것 같아서 수정해보았다. 
정렬되어 있는 배열이 주어진다고 했을 때, 첫번째값을 x에 담아두고 순차적으로 탐색하면서 x와 값이 같지 않아지는 경우에 새로운 배열에 x값을 저장한 뒤 해당 값으로 x를 업데이트하는 식으로 수정했다.

코드를 수정하면서 포인트 부분에 대한 이해도가 많이 부족한 걸 느껴서 다음 게시물에서는 포인트를 본격적으로 공부하고 정리해볼 예정이다.

```cpp
#include<iostream>
using namespace std;

void swap(int a[], int j) {
int temp = a[j];
a[j] = a[j + 1];
a[j + 1] = temp;
}

void BubbleSort(int a[], int size) {
    int last = size;
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < last - 1; j++) {
            if (a[j] > a[j + 1]) {
                swap(a, j);
              }
        }
    last--;
    }
}

void print(const int * a, int size) {
    cout << "배열 " << endl;
    for (int i = 0; i < size; i++) {
        cout << a[i] << " " ;
    }
}

void unique(const int * a, int size) {

    int uniqueArr[size];
    
    int x = a[0];
    int idx = 0;
    
    for (int i=0; i<size; i++) {
    
        if (a[i] != x) {
          uniqueArr[idx] = x;
          x = a[i];
          idx++;
        }
    
        if (i==size-1 && a[i-1]!=a[i]) {
          uniqueArr[idx] = x;
          idx++;
        }
    }  
    print(uniqueArr,idx);
}

int main()  {
    int a[8] = { 10, 100, 200, 10, 30, 10, 40, 10 };
    print(a, 8);
    cout << endl << "Bubble Sort 이후 " << endl;
    BubbleSort(a, 8);
    print(a,8);
    cout << endl << "Unique 이후 " << endl;
    unique(a, 8);
    return 0;
}
```

## ++ STL
표준 템플릿 라이브러리(STL: Standard Template Library)는 C++을 위한 라이브러리로서 알고리즘, 컨테이너, 함수자 그리고 반복자라고 불리는 네 가지의 구성 요소를 제공한다.
[참고](https://ko.wikipedia.org/wiki/%ED%91%9C%EC%A4%80_%ED%85%9C%ED%94%8C%EB%A6%BF_%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC)