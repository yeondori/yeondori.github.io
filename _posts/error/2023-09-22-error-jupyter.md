---
title: "[Error] Jupyter Notebook 비밀번호 관련"
excerpt: "오류 해결 과정"
categories: [Error, Jupyter Notebook]
tags: [Jupyter Notebook]
date: 2023-09-22
last_modified_at: 2023-09-22
render_with_liquid: false
---

난 그저 github에 그동안 풀었던 백준을 commit 하던 중에 그동안 맞힌 문제 번호와 이름을 README 파일에 잘 정리해보고 싶었을 뿐이었고(알고 보니 백준에서는 크롤링과 스크래핑을 허용하지 않는다고 한다), 문제 번호랑 문제 정보를 스크래핑해서 가져오고 싶었을 뿐이었는데..

오랜만에 jupyter notebook을 실행했더니

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/2ed4b613-c136-4611-9b66-8b2dbb8c957b)

비밀번호요? 그게 머드라.....
아무튼 과거의 내가 할 법한 여러 비밀번호도 입력해보고 구글 선생님께도 여쭤보았지만 해결되지 않았다.

## **시도1** token과 password 없이 실행

`jupyter notebook --ip='*' --NotebookApp.token='' --NotebookApp.password=''`

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/2ce2b0f0-b43b-4f47-a8e1-f60b9554d64f)

하라는 대로 `python -m notebook.auth password` 해봤더니 비밀번호를 설정할 수 있었고

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/ad726d99-a2fe-4f5d-b1d6-82641bb42f0d)

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/2ed4b613-c136-4611-9b66-8b2dbb8c957b)
ㅋㅋ 어 그래도 안돼~

## **시도2** jupyter notebook password, jupyter server password

jupyter notebook password나 jupyter server password를 터미널에 입력하면 똑같이 비밀번호를 설정할 수 있는데 예 그래도 안됩니다..
/Users/jeong-yeonseo/.jupyter/에 있는 jupyter_server_config.json 파일이나 jupyter_notebook_config.json에 있는 비밀번호를 입력해도 바뀌지 않았다..

## **해결** 설정파일 재설정

터미널에 `jupyter notebook --config=custom_config.py` 를 입력하면 기본 설정파일이 재지정된다!
입력했더니 자동으로 jupyter notebook이 실행됐고, 정상적으로 수행되었다.
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/b7f2c592-4475-4430-8362-799e548f78fb)

종료하고 다시 jupyter notebook을 입력해 실행시키면 똑같이 비밀번호를 입력하는 화면이 나온다. 우선 사용할 때는 설정파일을 지정해 사용하면 되긴 하지만 근본적인 문제가 무엇인지는 잘 모르겠다. 여유가 되면 다시 시도해 봐야겠다. 왜 이러는 지 아시면 꼭 좀 알려주십쇼..

[참고](https://greeksharifa.github.io/references/2019/01/26/Jupyter-usage/)
