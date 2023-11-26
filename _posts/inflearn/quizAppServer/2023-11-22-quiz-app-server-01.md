---
title: "[강의] 플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기 - Trouble Shooting"
excerpt: "인프런 강의 복습"
categories: [Inflearn, 플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기]
tags: [플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기 무작정 풀스택]
date: 2023-11-23
last_modified_at: 2023-11-23
render_with_liquid: false
---

---- 

# 플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기 [무작정 풀스택] [강의](https://www.inflearn.com/course/%ED%94%8C%EB%9F%AC%ED%84%B0-%EC%9E%A5%EA%B3%A0-%ED%80%B4%EC%A6%88%EC%95%B1-%EC%84%9C%EB%B2%84-%ED%92%80%EC%8A%A4%ED%83%9D/dashboard)

강의와의 버전 차이로 인해 발생하는 오류가 있어 기록해보고자 한다.

## screen_home.dart

### RaisedButton -> ElevatedButton

RaisedButton이 ElevatedButton으로 변경됨에 따라 버튼 색상을 
`style: ButtonStyle(backgroundColor: MaterialStateProperty.all<Color>(Colors.deepPurple))` 식으로 변경해주어야 한다.

## model_quiz.dart

```dart
class Quiz {
  String title;
  List<String> candidate;
  int answer;

  Quiz({this.title, this.candidate, this.answer})

  Quiz.fromMap(Map<String, dynamic> map)
      : title = map['title'],
        candidate= map['candidates'],
        answer = map['answer']
}
```

기존의 코드에서 생성자 부분에 required를 추가해주어야 한다.

`Quiz({required this.title, required this.candidate, required this.answer});`

참고로 모든 this 쪽에는 required를 붙여주어야 하며 이 부분은 앞으로 생략한다!

## flutter_swiper

강의에서는 화면을 옆으로 넘기는 기능때문에 flutter_swiper를 설치한다.
이때 버전 부분을 비운 채로 `flutter_swiper: 1.0.2`를 pubspec.yaml에 작성하게 되는데 다음과 같은 오류가 발생한다.

```
he current Dart SDK version is 3.2.0.

Because quiz_app depends on flutter_swiper any which doesn't support null safety, version solving failed.

The lower bound of "sdk: '>=2.0.0-dev.48.0 <3.0.0'" must be 2.12.0 or higher to enable null safety.
```

이는 null-safety를 지원하지 않아 발생하는 문제로, 찾아보니 null-safety를 지원하는 라이브러리가 존재했다.
=> `flutter_swiper_null_safety:` 를 추가해주면 된다!

[참고](https://velog.io/@ejayjeon/Flutter-Error-Cannot-run-with-sound-null-safety-because-the-following-dependenciesdont-support-null-safety-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EB%B2%95)

## screen_quiz.dart

앞의 screen_home과 마찬가지로 raisedButton의 textColor와 Color를 다음과 같이 수정해준다.


```dart
child: ElevatedButton(
  onPressed: _answers[_currentIndex] == -1
    ? null
      : () {
    if (_currentIndex == widget.quizs.length - 1) {// '결과보기' 버튼을 눌렀을 때의 동작
    } else {
      _answerState = [false, false, false, false];
      _currentIndex += 1;
      _controller.next();
    }
  },
  style: ElevatedButton.styleFrom(
    textStyle: TextStyle(color: Colors.white),
    primary: Colors.deepPurple,
    ),
  child: _currentIndex == widget.quizs.length - 1
    ? Text('결과보기')
    : Text('다음문제'),
)
```

## Null 관련
다음 문제 보기 버튼 만들기 과정에서 다음의 오류가 발생했다.

`Expected a value of type 'List<String>', but got one of type 'Null'`


이는 Quiz 클래스의 candidates 필드를 nullable로 선언해준 뒤(List<String>? candidates), (List<String>?)_buildCandidates 메서드에서 각 후보자의 텍스트를 가져올 때, 
Null 체크 로직을 추가해 해결해주었다.

즉. _buildCandidates 메서드에서 quiz.candidates가 null이 아닌지 확인한 후에 접근하도록 수정한다.
`text: quiz.candidates?[i] ?? '',  // null-aware 연산자 사용` 

## Heroku 배포

Heroku가 작년부터 유료로 전환되는 이슈로, 강의대로 Heroku로 배포할 수 없게 되었다..
대신 [레퍼런스](https://cocobi.tistory.com/248)를 참고해 Koyeb으로 배포해보려 했으나,

```
Build ready to start ▶️
>> Cloning github.com/yeondori/first-flutter-django.git commit sha 1417c5b6080047b91d26620359a8398bf373e204 into /workspace
Initialized empty Git repository in /workspace/.git/
From https://github.com/yeondori/first-flutter-django
 * branch            1417c5b6080047b91d26620359a8398bf373e204 -> FETCH_HEAD
HEAD is now at 1417c5b change python version
Previous image with name "registry01.prod.koyeb.com/k-5c4c1aed-38b3-4800-96c6-126bd4d637db/50f25658-4615-4107-8b8a-f941949fe5cf:latest" not found
heroku/python   0.0.0
heroku/procfile 2.0.1
Layer cache not found
-----> Using Python version specified in runtime.txt
 !     Requested runtime 'Python-3.8.18' is not available for this stack (heroku-20).
 !     For supported versions, see: https://devcenter.heroku.com/articles/python-support
ERROR: failed to build: exit status 1
exit status 51
```

이런 오류가 발생헀다... 

heroku의 공식 문서에 맞는 python 버전으로 변경해주어도 상황은 같았고, heroku 배포가 안돼서 buildpack 등의 옵션도 무의미했다.

[koyeb 공식 문서](https://www.koyeb.com/docs/build-and-deploy/deploy-with-git)에도 github을 이용한 배포에 관련된 설명이 있으며, 
[python](https://www.koyeb.com/docs/build-and-deploy/build-from-git/python) runtime 부분에 맞는 버전으로 변경해주었다.

빌드는 성공했는데요!

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/19306e0d-6111-47bc-922d-82a8126536af)

Instance에서 또다시 난관에 봉착..

가상환경에 gunicorn 설치가 되어 있지 않아 설치한 후 다시 빌드해주었고, Procfile도 `web: gunicorn quiz:app`로 수정했다.

=> 이래도 해결되지 않아 다른 방식 찾아보는 중

## screen_home.dart

로딩중을 표시하기 위한 SnackBar 활용에서 

`_scaffoldKey.currentState!.showSnackBar` -> `ScaffoldMessenger.of(context).showSnackBar` 로 수정했다.