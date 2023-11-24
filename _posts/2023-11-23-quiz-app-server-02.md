---
title: "[강의] 플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기 - 프로젝트 환경 세팅"
excerpt: "인프런 강의 복습"
categories: [Inflearn, Quiz-App]
tags: [플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기 [무작정 풀스택]]
date: 2023-11-22
last_modified_at: 2023-11-22
render_with_liquid: false
---

---- 

# 플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기 [무작정 풀스택] [강의](https://www.inflearn.com/course/%ED%94%8C%EB%9F%AC%ED%84%B0-%EC%9E%A5%EA%B3%A0-%ED%80%B4%EC%A6%88%EC%95%B1-%EC%84%9C%EB%B2%84-%ED%92%80%EC%8A%A4%ED%83%9D/dashboard)

## 0. 환경 설정
인프런 강의 플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기를 학습하기 위해 환경 설정을 해주었다.

순서는 다음과 같이 진행했다.

1. IntelliJ에서 Android SDK 설치
2. IntelliJ flutter 플러그인 설치 
3. [flutter SDK](https://docs.flutter.dev/get-started/install/macos) 설치
4. flutter 환경변수 설정

다음의 사이트들을 참고해 진행했다.

[크로스플랫폼 개발을 위한 Flutter 시작 가이드 - IntelliJ 환경 설정과 기본 앱 구축](https://aday7.tistory.com/entry/%ED%81%AC%EB%A1%9C%EC%8A%A4-%ED%94%8C%EB%9E%AB%ED%8F%BC-%EA%B0%9C%EB%B0%9C%EC%9D%84-%EC%9C%84%ED%95%9C-Flutter-%EC%8B%9C%EC%9E%91-%EA%B0%80%EC%9D%B4%EB%93%9C-IntelliJ-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95%EA%B3%BC-%EA%B8%B0%EB%B3%B8-%EC%95%B1-%EA%B5%AC%EC%B6%95)  
[맥에서 플러터 설치 및 환경변수 설정하기](https://blog.naver.com/bluecrossing/222277992718)

정상적으로 설정이 완료된 줄 알았으나 

```
Unhandled exception:
Exception: Flutter failed to create a directory at "/Users/jeong-yeonseo/.config/flutter".
Please ensure that the SDK and/or project is installed in a location that has read/write permissions for the current user.
```

이런 오류가 발생했고,
https://www.fluttercampus.com/guide/286/flutter-failed-to-write-to-a-file/

여길 참고해 `sudo chown -R <your_username> /flutter_sdk_path/` 로 권한을 부여해도 해결되지 않았다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/b649f993-b2cb-4524-afac-056c18d4e289)

설정에서 '그래도 열기'를 클릭해도 달라지는 건 없었다..

`brew install flutter` 로 설치해봐도 같은 오류가 발생했고 `sudo chown -R jeong-yeonseo /Users/jeong-yeonseo/.config` 로 해결되었다 엉엉

에러 해결 후 터미널에 `flutter doctor`를 입력해 설치해야 할 패키지 목록을 확인할 수 있었고,

Xcode와 Android Studio 등은 다음의 [글](https://velog.io/@juheesvt/Flutter-%EB%A7%A5-OS%EC%97%90%EC%84%9C-%ED%94%8C%EB%9F%AC%ED%84%B0-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)을 참고해 설치했다.

나머지는 전부 설치가 됐는데, Xcode만 다음과 같은 문제가 있었다. 

```
[!] Xcode - develop for iOS and macOS (Xcode 15.0.1)
    ✗ Unable to get list of installed Simulator runtimes.
```

`xcodebuild -downloadPlatform iOS` 또는 Xcode를 실행시켜 simulator를 다운받아주면 해결된다.

+ brew로 flutter를 설치해봤는데 IntelliJ에서 Flutter 의 SDK 경로를 인식 못해서 삭제하고 위의 방식으로 다시 설치해 주었다. 
+ 사용자 계정 아래 develop 폴더를 만들고 그 안에서 압축을 푼 뒤 환경변수 설정을 해주었더니 위의 오류 없이 잘 인식되었다. 