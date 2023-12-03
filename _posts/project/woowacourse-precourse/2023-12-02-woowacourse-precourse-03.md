---
title: "[우테코 프리코스] 숫자야구 미션 회고"
excerpt: "우테코 프리코스 도전기"
categories: [Project, woowacourse-precourse]
tags: [우테코 프리코스]
date: 2023-12-02
last_modified_at: 2023-12-02
render_with_liquid: false
---

---- 


# 숫자야구 미션⚾️

1주차 미션은 숫자야구 게임에서 문제를 출제하고, 볼/스트라이크를 판정하는 프로그램을 구현하는 것이었다. 

자세한 요구사항은 [이곳](https://github.com/yeondori/java-baseball-6/tree/yeondori)에서 확인 가능하다. 

첫번째 과제라 그런지 별다른 요구사항이 주어지지는 않았다. 구현해야 할 기능도 크게 까다롭지는 않았어서 백준 풀듯이 Application 내부에서 다 구현하려고 했던 것 같다.   

어디 보여주기 민망한 [제출 과제](https://github.com/yeondori/java-baseball-6/tree/yeondori/docs)

심지어 이때는 소감문도 따로 저장해 둔 게 없다. README를 작성하는 게 거의 처음이기도 해서 첫주차 과제에서는 구현해야 할 기능 목록을 정리하고 명세하는 것에 중점을 뒀던 것 같다.

그치만 지금 다시 보니 뭔가 이 짤 같고요...

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/c1d9674d-41f1-4b9c-bb31-536e896ee7d4)

예쁘게 꾸민다고 내용이 예쁘고 알차지는 게 아니지요...

---

그래서 이번에는 기능별로 클래스를 나눠보았다!

https://github.com/yeondori/java-baseball-6/tree/yeondori2

게임을 생성하는 GameBuilder와 결과를 판정하는 Judgement, 스트라이크와 볼 개수를 가지는 Result, 그리고 입출력과 관련해 클래스를 별도로 구분해주었고, 
입출력 로직을 제외하고는 단위 테스트도 진행해주었다. 

그러나 결과 출력 부분이 결과 부분과 너무 밀접해 분리하려던 부분도 사실 모호해졌고, 테스트 코드를 위해 getBall과 getStrike가 존재하는 것 같아 이 부분도 더 손봐야 한다.  
결과 상태를 나타내기 위해 Status라는 enum도 생성했는데 하다보니까 점점 뇌절 같고요

결국 이전에 제출한 것보다 나아졌냐 물으신다면.... 글쎄요........ 아닌 것 같은데......

우선은 2주차 과제까지 다시 풀어보고 돌아오겠읍니다..