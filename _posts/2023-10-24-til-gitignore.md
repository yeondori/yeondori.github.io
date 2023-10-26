---
title: "[Keyword][Git] Gitignore"
excerpt: "2023.10.24"
categories: [1Day-1Keyword, Git]
tags: [git]
date: 2023-10-24
last_modified_at: 2023-10-24
render_with_liquid: false
---

---- 
**Keyword**

하루에 하나씩 키워드를 정해 공부해보려고 한다.

일단은 왜 이 키워드를 공부하려고 하는가(why), 그래서 이 키워드가 무엇인가(what), 어떻게 적용해나갈 것인가(how)로 나눠 정리하려고 한다!

----- 

# Why?

gitignore도 마찬가지로 프로젝트를 리뷰하면서 야호가 한번 찾아보면 좋을 것 같다고 알려준 주제!
그래서 이게 왜 필요한가? 를 알기 앞서 gitignore가 무엇인가부터 살펴보자

# What?
[git-scm](https://git-scm.com/docs/gitignore)에 따르면. gitignore은 `Specifies intentionally untracked files to ignore.` 라고 명시되어 있다. 

즉, 의도적으로 추적되지 않은 파일을 무시하도록 지정하는 것으로,
`.gitignore` 이라는 파일을 작성하면 규칙에 따라 특정 확장자를 가진 파일을 모두 무시하거나 특정 폴더 하위에 있는 모든 파일을 무시하는 등 Git이 무시해야 하는 파일을 지정할 수 있게 된다.

파일의 각 줄은 gitignore 패턴을 지정하며, Git은 일반적으로 여러 소스들에서 gitignore 패턴을 확인한 후 다음의 우선순위에 따라 무시할 것인지를 결정하게 된다.

## gitignore 우선 순위

1. `Patterns read from the **command line** for those commands that support them.`

    커맨드 라인의 패턴


2. `Patterns read from a **.gitignore file** in the same directory as the path, or in any parent directory (up to the top-level of the working tree), 
  with patterns in the higher level files being overridden by those in lower level files down to the directory containing the file. `
  
    .gitignore 파일의 패턴. 동일한 디렉토리 또는 부모 디렉토리(작업 트리의 최상위 수준까지)에서 읽으며 하위 디렉토리에 작성된 .gitignore은 상위 디렉토리에 작성된 .gitignore의 내용을 override 한다.
    

3. `Patterns read from **$GIT_DIR/info/exclude**.`
  
    $GIT_DIR/info/exclude의 패턴


4. `Patterns read from the file specified by the configuration variable core.excludesFile.`

    core.excludesFile 의 패턴

## Pattern Format

|       | 설명                                                                                                                                                                                                                                                                               |
|:------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| blank | no files, 가독성을 위한 구분자 역할                                                                                                                                                                                                                                                         |
| #     | 주석, 백슬래시("\")로 탈출할 수 있다. <br/>후행 공백은 백슬래시("\")로 따옴표를 붙이지 않는 한 무시된다.                                                                                                                                                                                                              |
| !     | 패턴을 부정. 이전 패턴에 의해 제외된 파일이 다시 포함된다.<br/> 해당 파일의 상위 디렉터리를 제외하면 파일을 다시 포함할 수 없다. <br/> Git는 성능상의 이유로 제외된 디렉터리를 나열하지 않으므로, 포함된 파일의 패턴은 정의된 위치에 상관없이 영향을 미치지 않는다.<br/> 문자 "!"로 시작하는 패턴(예: "\!important!.txt")은 첫 번째 "!" 앞에 백슬래시("\")를 넣는다.                                            |
| /     | 디렉토리 구분자. <br/>시작이나 중간에 구분자가 있는 경우, 패턴은 특정 .gitignore 파일 자체의 디렉터리 수준과 상대적이다. 그렇지 않으면 패턴은 .gitignore 수준 아래의 임의 수준에서도 일치할 수 있다.<br/>끝에 구분자가 있으면 패턴은 디렉토리만 매칭하며, 그렇지 않으면 패턴은 파일과 디렉토리 모두와 매칭할 수 있다.                                                                                 |
| *     | /를 제외한 모든 문자에 매칭.                                                                                                                                                                                                                                                                |
| ?     | /를 제외한 모든 한 문자와 매칭.                                                                                                                                                                                                                                                              |
| **    | 디렉토리 사이의 전체 경로에 매칭. <br/>/의 앞에 오는 경우, 모든 디렉토리에 매칭한다. 예를 들어 "`**/foo`"는 파일 또는 디렉토리 "foo"와 매칭하며, 이는 패턴 `"foo"`와 동일하다.<br/> /뒤에 오는 경우 내부의 모든 파일을 무시한다. 예를 들어, `"abc/**"`는 .gitignore 파일의 위치와 관련된 디렉토리 "abc" 내부의 모든 파일을 무한한 깊이로 무시한다. `"a/**/b"`는 "a/b", "a/x/b", "a/x/y/b" 등을 무시한다. |

이해가 잘 되지 않는 부분은 아래 두 블로그를 참고하자.

[Git : .gitignore 문법 및 사용법 정리 (추가: '!' 패턴 동작 안될때 해결법)](https://jw910911.tistory.com/136)

[[git] .gitignore 문법 및 규칙 정리](https://sh-hyun.tistory.com/22)


## Notes
gitignore 파일의 목적은 Git에 의해 추적되지 않은 특정 파일이 추적되지 않은 채로 남아 있도록 하는 것이다.

현재 추적되는 파일을 더 이상 추적하지 않으려면 gitrm --cacheed를 사용하여 인덱스에서 파일을 제거한 다음 .gitignore 파일에 파일 이름을 추가하여 나중에 커밋에서 파일이 다시 도입되지 않도록 중지할 수 있다.

Git는 작업 트리의 .gitignore 파일에 액세스할 때 심볼릭 링크를 따르지 않으므로 파일이 인덱스 또는 트리에서 액세스할 때와 파일 시스템에서 액세스할 때의 동작이 일관되게 유지될 수 있다.
+ 심볼릭 링크: 특정 파일이나 디렉터리에 대한 참조를 포함하는 특별한 파일이다. 간단하게 Windows 운영체제에서 ‘바로 가기’ 와 같은 기능을 한다고 보면 된다. [심볼릭 링크(symbolic link)](https://madplay.github.io/post/what-is-a-symbolic-link-in-linux)

# How?
그래서 어떻게 쓰는 거냐면요 

## Examples

| 사용 예시                     | 설명                                                                                                                                   |
|:--------------------------|:-------------------------------------------------------------------------------------------------------------------------------------|
| `hello.*`                 | hello로 시작하는 파일이나 디렉토리 무시 <br/>하위 디렉토리가 아닌 디렉토리에만 제한하려면 패턴 앞에 슬래시(/hello.*)를 붙인다. 이는 hello.txt, hello.c는 무시하나 a/hello.java는 무시하지 않는다. |
| `foo/ `                   | 디렉터리 foo와 그 아래의 경로를 무시하지만 일반 파일이나 심볼릭 링크 foo는 무시하지 않는다.                                                                              |
| `doc/frotz`, `/doc/frotz` | 같은 의미이다. 패턴에 이미 중간 슬래시가 있는 경우 선행 슬래시는 관련이 없으며,                                                                                       |
| `foo/* `                  | foo/test.json (파일), foo/bar (디렉토리) 등을 무시한다. 그러나 foo/bar/hello.c (파일)은 무시하지 않는다.                                                      |
| `*.json `                 | 모든 json 확장자 파일을 무시한다.                                                                                                                |
 

## Conclusion
원격 저장소에서는 관리하지 않아도될 idea나 gradle 관련 파일들이 함께 push되는 것을 막기 위해서 gitignore를 사용한다. 
다시 말해서, 프로젝트에서 관리가 필요하지 않은, 로컬 개발 환경에 종속적인 파일들을 git 추적 대상에서 제외시키기 위해 gitignore 파일을 이용하여 관리한다.

[.gitignore 적용하기](https://velog.io/@psk84/.gitignore-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)

.idea나 .gradle 모두 저장소에 필요한 프로젝트의 일부겠지 건들지 말자 라고 넘기면서 왜 존재하고 어디에 쓰이는 지 관심조차 가지지 않았던 과거를 반성하며.... 
앞으로는 이런 것 하나하나 허투루 넘기지 않겠습니다.. 갈 길이 정말 멀다!