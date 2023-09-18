---
layout: post
title:  "My first post"
date:   2023-09-18 15:12:36 +0530
categories: Javascript NodeJS
---

github 블로그 진자 어렵다...

```C++
 //배열 속 특정한 값 x를 찾는 프로그램 작성

#include<iostream>
using namespace std;

int main() {
  int a[10] = {10, 50, 55, 100, 20, 40, 70, 101, 58, 3};
  int x;
  int i;
  cout << "숫자를 입력하세요." << endl;
  cin >> x;
  for (i=0; i<10; i++) {
  if (a[i]==x) {
    cout << "찾았습니다! " << x << "의 위치는 " << i << "번째 입니다." << endl;
  }
  else if (i==9 && a[i]!=x)
    cout << x << "은(는) 배열에 없는 숫자입니다." << endl;
  }
  return 0;
}
```
