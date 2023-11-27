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

=> 문제는 이게 아니라 requirements.txt에 gunicorn이 없었다...!
강의를 따라 다시 필요한 모듈을 설치하고 freeze해주었더니 gnuicorn 또한 requirements에 정상적으로 들어가 서버가 드디어 돌아간다~~!!!!

## koyeb 배포

이제 다음 난관은 admin으로 접속하려고 하면 서버 에러가 발생한다는 것인데...

```
User
ERROR:django.request:Internal Server Error: /admin/login/
Traceback (most recent call last):
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/backends/utils.py", line 89, in _execute
    return self.cursor.execute(sql, params)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/backends/sqlite3/base.py", line 328, in execute
    return super().execute(query, params)
sqlite3.OperationalError: no such table: auth_user
The above exception was the direct cause of the following exception:
Traceback (most recent call last):
  File "/app/.heroku/python/lib/python3.8/site-packages/django/core/handlers/exception.py", line 55, in inner
    response = get_response(request)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/core/handlers/base.py", line 197, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/utils/decorators.py", line 46, in _wrapper
    return bound_method(*args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/views/decorators/cache.py", line 62, in _wrapper_view_func
    response = view_func(request, *args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/contrib/admin/sites.py", line 441, in login
    return LoginView.as_view(**defaults)(request)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/views/generic/base.py", line 104, in view
    return self.dispatch(request, *args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/utils/decorators.py", line 46, in _wrapper
    return bound_method(*args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/views/decorators/debug.py", line 92, in sensitive_post_parameters_wrapper
    return view(request, *args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/utils/decorators.py", line 46, in _wrapper
    return bound_method(*args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/utils/decorators.py", line 134, in _wrapper_view
    response = view_func(request, *args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/utils/decorators.py", line 46, in _wrapper
    return bound_method(*args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/views/decorators/cache.py", line 62, in _wrapper_view_func
    response = view_func(request, *args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/contrib/auth/views.py", line 90, in dispatch
    return super().dispatch(request, *args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/views/generic/base.py", line 143, in dispatch
    return handler(request, *args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/views/generic/edit.py", line 152, in post
    if form.is_valid():
  File "/app/.heroku/python/lib/python3.8/site-packages/django/forms/forms.py", line 201, in is_valid
    return self.is_bound and not self.errors
  File "/app/.heroku/python/lib/python3.8/site-packages/django/forms/forms.py", line 196, in errors
    self.full_clean()
  File "/app/.heroku/python/lib/python3.8/site-packages/django/forms/forms.py", line 434, in full_clean
    self._clean_form()
  File "/app/.heroku/python/lib/python3.8/site-packages/django/forms/forms.py", line 455, in _clean_form
    cleaned_data = self.clean()
  File "/app/.heroku/python/lib/python3.8/site-packages/django/contrib/auth/forms.py", line 250, in clean
    self.user_cache = authenticate(
  File "/app/.heroku/python/lib/python3.8/site-packages/django/views/decorators/debug.py", line 42, in sensitive_variables_wrapper
  File "/app/.heroku/python/lib/python3.8/site-packages/django/contrib/auth/__init__.py", line 77, in authenticate
    user = backend.authenticate(request, **credentials)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/contrib/auth/backends.py", line 46, in authenticate
    user = UserModel._default_manager.get_by_natural_key(username)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/contrib/auth/base_user.py", line 54, in get_by_natural_key
    return self.get(**{self.model.USERNAME_FIELD: username})
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/models/manager.py", line 87, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/models/query.py", line 633, in get
    num = len(clone)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/models/query.py", line 380, in __len__
    self._fetch_all()
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/models/query.py", line 1881, in _fetch_all
    self._result_cache = list(self._iterable_class(self))
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/models/query.py", line 91, in __iter__
    results = compiler.execute_sql(
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/models/sql/compiler.py", line 1562, in execute_sql
    cursor.execute(sql, params)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/backends/utils.py", line 67, in execute
    return self._execute_with_wrappers(
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/backends/utils.py", line 80, in _execute_with_wrappers
    return executor(sql, params, many, context)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/backends/utils.py", line 89, in _execute
    return self.cursor.execute(sql, params)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/utils.py", line 91, in __exit__
    raise dj_exc_value.with_traceback(traceback) from exc_value
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/backends/utils.py", line 89, in _execute
    return self.cursor.execute(sql, params)
  File "/app/.heroku/python/lib/python3.8/site-packages/django/db/backends/sqlite3/base.py", line 328, in execute
    return super().execute(query, params)
django.db.utils.OperationalError: no such table: auth_user

```

대충  auth_user라는 테이블이 존재하지 않아서 발생한 문제인데,,, 
manage.py 에도 superUser가 등록되어 있는 것을 분명히 확인했고, migration도 이미   No migrations to apply. 상태였다.

혹시 DATA_URL path 인식이 안되는 문제와 연관이 있을까 싶어서  koyeb 배포 설정에서 환경변수로 등록해주었더니 이 부분에 대한 에러는 발생하지 않았다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/1a5d9f22-4c47-4965-b366-e910bb263e49)
 
대신 여전히 admin 관련한 문제가 발생했는데 python manage.py collectstatic로 정적파일을 수집해봤는데도 달라지는 게 없었고,

migrate를 실행해도 계속 같은 결과가 나와서 혹시 migrate에 이상이 있나 싶어 migrate파일과 db파일을 지우고 다시 migrate해주었다.

이래도 계속 migrate할 때마다 같은 결과가 반복됐고,, 로컬에선 migrate에 이상이 없는데 서버에서만 계속 migrate 부분에 문제가 있었다.

우선 조금 더 찾아보고 koyeb 측에도 물어봐야겠다.......

## admin 계정 찾기

그와중에 비밀번호를 까먹었는데요... 이 [레퍼런스](https://kitle.xyz/post/58/)를 통해 찾을 수 있었습니다.. 
요약하자면

**아이디 찾기**

1. 프로젝트 폴더에서 manage.py 가 있는 곳으로 이동
2. `python manage.py shell`로 쉘 진입
3. `from django.contrib.auth.models import User`로 사용자 정보 가져오기
4. `superusers = User.objects.filter(is_superuser=True)` 슈퍼유저 가져오기
5. `superusers` 출력하기 -> 이름 찾을 수 있음

**비밀번호 찾기**
1. manage.py 있는 곳에서 `python3 manage.py changepassword admin` 로 비밀번호 재설정

감사합니다!!! 

## screen_home.dart

로딩중을 표시하기 위한 SnackBar 활용에서 

`_scaffoldKey.currentState!.showSnackBar` -> `ScaffoldMessenger.of(context).showSnackBar` 로 수정했다.