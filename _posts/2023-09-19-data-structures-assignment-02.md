---
title: "[과제] 02 함수를 이용하여 배열 속 원하는 위치를 출력하기"

excerpt: "데이터 구조론 과제"
categories: [ERICA, Data Structures]
tags: [C++, Data Structures]
date: 2023-09-19
last_modified_at: 2023-09-19
render_with_liquid: false
---
2주차 과제는 무려 두 개였다.

- **과제1**:
  배열 a와 특정한 값 x가 주어졌을 때, 배열 a 내에 존재하는 x의 위치를 출력하는 프로그램 작성
- **과제2**:
  과제 1을 바탕으로 두 배열이 주어졌을 때, 한 배열에서의 특정한 값의 위치를 찾아내고 다른 한 배열의 해당 인덱스에 위치한 값을 출력하는 프로그램 작성

# 함수를 이용하여 배열 속 원하는 위치를 출력하기

## 2019-09-12

### 프로그램 리스트

#### 과제1

```cpp
#include<iostream>
using namespace std;
int a[10] = { 10, 50, 55, 100, 20, 40, 70, 101, 58, 3 };
int x = 70;

int search(int a[], int x);
void print(int i);

int search(int a[], int x) {  
  for (int j = 0; j < 10; j++) {
    if (a[j] == 70) {
      return (j+1);
    }
  }
}

void print(int i) {
  cout << x << "의 위치는 " << i << "째 입니다." << endl;
}

int main () {
  int i = search(a, x);
  print(i);

  return 0;
}
```

#### 과제2

```cpp
#include <iostream>
#include <string>
using namespace std;

int search(int age[], int size, int value) {
  for (int i = 0; i < size; i++) {
    if (age[i] == value) {
      cout << "나이는 " << value << "살 입니다." << endl;
      return i;
    }   
  }
}

int main() {
  string name[10] = { "Kim","Lee","Wu","Park","Cha","Dol","Sang","Choi", "Dal", "Sal" };
  int age[10] = { 10,50,55,100,20,40,70,101,58,3 };
  int i = search(age, 10, 70);
  cout << "이름은 " << name[i] << "입니다." << endl;
   
  return 0;
}
```

### 소감문

과제 #01은 10개의 배열 중 특정한 값 x(과제의 경우, x=70)를 찾아내는 프로그램을 작성하는 것이었습니다. 지난주 과제와 차이점이 있다면, 값을 찾는 search 함수와 그 값을 출력하는 print 함수를 만들어야 한다는 것이었습니다. 함수에 대한 기본적인 개념이 부족하여 과제를 시작하기까지 많은 시간이 걸렸습니다. 처음엔 print 함수를 따로 만들지 않고, search 함수에 `if (a[i]==70) {cout << x << “의 위치는 “ << I << “째 입니다.” ;}` 를 써서 바로 출력할 수 있도록 하였습니다. 코딩에 성공하고 난 후, print 함수를 만드는 것은 생각보다 더 간단하겠구나’ 라는 생각을 했습니다. 그러나 int search 함수에 저장한 리턴값을 void print 함수에서 어떻게 불러와야 하는가에 대한 고민이 많았습니다. 많은 시행착오 끝에 main 함수 안에 `int i = search(a,x);` 라는 구문 한 줄을 추가할 수 있었습니다. 남들은 3분 만에 끝낼 과제를 3시간을 족히 넘겨서 해냈지만 혼자 힘으로 문제를 해결해나갔다는 것만으로도 엄청난 성취감을 느꼈습니다.

과제 #02는 나이와 이름에 관한 두 개의 배열에서 특정한 나이(과제의 경우, 나이 70)를 입력하면 그에 대응하는 이름을 말하는 프로그램을 작성한 것이었습니다. 처음 시작은 함수를 이용하지 않고 a[i]==70이면 name[I]를 출력하는 프로그램을 작성해보았습니다. 과제 #01의 `int I = search(a,x);` 부분을 해결하고 나니, 두 번째 과제는 훨씬 더 수월하게 해결할 수 있었습니다. 지금은 추석 연휴라 전라남도 구례에 내려와 있는데, 노트북과 책, 공책을 한가득 들고 와서 친척분들이 모두 놀라고 뿌듯해하셨습니다. 카페에 오려면 차로 15분은 가야하므로 부모님의 도움을 받았습니다. 과제를 다 끝냈지만, 아직 생각해야 하는 과제가 남아 있어서 공부를 더 하고 갈 예정입니다. 솔직히 말하면, 데이터 구조론 과제를 하는 것이 마냥 즐겁지만은 않습니다. 많이 어렵기도 하고 스트레스도 받고 있습니다. 울고 싶었던 적도 있고 화가 난 적도 있었습니다. 아직 개강한지 2주차밖에 안됐다는 것이 매우 걱정되기는 하지만, 문제를 스스로 해결해냈을 때의 성취감과 배움의 즐거움이 무엇인지에 대해서는 새로이 느끼는 것이 정말 많습니다. 학교를 4학기나 되었지만 공강 시간에 과제나 시험 목적이 아닌 진짜 ‘공부’를 하는 것은 처음인 것 같아 부끄러우면서도 다행이라는 생각이 들었습니다.

## 2023-09-19

누가 학교 과제에 저렇게까지 솔직하고 TMI 남발하냐고 하겠지만 교수님께서 분명 꼭 저런 시시콜콜한 이야기까지 다 쓰라고 하셨고.. 실제로 선배 중에서는 과팅한 것도 쓴 적 있다고 했었다😅. 3시간 걸려서 이걸 해냈다는 게 대단하기도 하고... 기분이 참.... 도대체 왜 3시간이 걸렸을까 궁금하네.. 과제 2번 같은 경우는 아마 이때는 아직 2주차여서 set, map 등의 자료 구조를 배우기 전이라 배열 두 개를 주고 해결하게 하셨던 것 같은데, 문제에서 원하는 게 단순히 주어진 나이에 해당하는 사람의 이름을 출력하는 거라면 더 단순히 풀어볼 수 있을 것 같다.

```cpp
#include<iostream>
#include <unordered_map>
#include <algorithm>
using namespace std;

void find_key(unordered_map<string, int> m, int value) {
  bool found = false;
  
  for_each(m.begin(), m.end(),
    [&value, &found](const pair<string, int> &p){
  
     if (p.second == value){
       cout << "이름은 " << p.first << "입니다." << endl;
       found = true;
    }
  });
 
  if (!found) {cout << "존재하지 않는 값입니다.";}
}

int main() {
  unordered_map<string, int> m = { {"Kim",10} ,{"Lee",50},{"Wu",55},{"Park",100},{"Cha",20},{"Dol",40},{"Sang",70},{"Choi",101}, {"Dal",58}, {"Sal",3} };
  int x; 
  
  cout << "찾고자 하는 사람의 나이를 입력하세요." << endl;
  cin >> x;

  find_key(m, x);
  return 0;
}
```

으음 단순하지 않았고... c++ 너무 오랜만이라 전부 다 생소하고... unordered_map, for_each, pair 등등 전부 다 하나하나 검색해서 다시 읽어봤다.

우선 참고한 코드에서는 unordered_map*을 사용해 그대로 사용했고, value로 key를 찾는 부분과 값을 출력하는 부분은 하나의 함수에 같이 구현했다. 지난 코드와 달리 값이 존재하지 않을 경우에는 존재하지 않는 값이라는 문구가 출력되도록 수정했고, value 값도 직접 입력받는 것으로 수정했다.


*unordered_map: hash\_table 기반의 hash container로, hash\_map과 거의 동일하나 MSDN에서는 STL 표준 컨테이너인 [unordered map](https://docs.microsoft.com/ko-kr/cpp/standard-library/unordered-map-class?view=vs-2019) 사용을 권장하고 있음.

함수 부분을 간략히 설명하자면 다음과 같은 방식이다.

1. `for_each(m.begin(), m.end(), lambda);`for_each로 맵의 처음부터 끝까지(m.end()는 포함되지 않음) 람다를 수행한다.
2. `[&value, &found](const pair<string, int> &p) {if (p.second == value){(key출력, found 변경)}}`  lambda: [캡쳐]'(매개 변수){함수 몸통}의 형태
   - 캡쳐 엉역`[&value, &found]` &: 참조변수의 형태로 변수들을 가져온다. 변수의 값을 변경할 수 있으며 참고로 =는 변수를 복사한다는 의미이다.(변수 변경 불가)
   - 매개 변수 `(const pair<string, int> &p)` for\_each문 내부로 들어가게 되면 첫 번째 인자 값으로 받은 주소 값을 바탕으로 사용자가 정의한 함수의 매개변수에 전달됨. 위 코드의 경우 map의 각 pair들을 p로 받겠다는 뜻(맞나...?)
   - 함수 `if (p.second == value){cout << "이름은 " << p.first << "입니다." << endl; found = true;}` map 탐색 중 입력받은 value값과 p의 value가 같다면 key값을 출력하고 found를 true로 변경한다. for_each는 break가 불가하며, found는 map 내에 존재하지 않는 value를 입력받았을 경우를 위한 변수

데이터 구조론 이후로는 python만 썼어서 c++은 지금 봐도 너무너무 어렵고, 사실 그땐 깊이 있게 배운 게 아니라 과제를 겨우 해낼 정도의 코딩 실력이었어서 더욱 더 힘든 것 같다 🤣🤣

#### 참고

* unordered_map 설명 [링크](https://dalgong2.tistory.com/27)
* map과 unordered_map 비교 [링크](https://gyong0.tistory.com/70)
* for_each 코드 참고 [링크](https://www.techiedelight.com/ko/reverse-lookup-stl-map-cpp/)
* for_each 설명 [링크](https://hwan-shell.tistory.com/88)
* const 변수의 위치와 의미 [링크](https://easycoding91.tistory.com/entry/C-%EA%B0%95%EC%A2%8C-const-%EC%9C%84%EC%B9%98%EC%9D%98-%EC%9D%98%EB%AF%B8%EC%99%80-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95)
* 람다 설명 링크 [링크](https://hwan-shell.tistory.com/84)
* 페어 설명 [링크](https://ya-ya.tistory.com/91)
