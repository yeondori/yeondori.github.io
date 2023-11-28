---
title: "[Docker,Linux] 명령어 정리"

excerpt: "docker, linux 명령어 정리"
categories: [Docker]
tags: [Docker, 현장실습]
date: 2023-09-19
last_modified_at: 2023-09-19
render_with_liquid: false
---
현장실습 중인 회사에서 docker를 사용해야 할 일이 생겼고, 팀장님께서 우리도 도커에 대해 공부해보면 좋을 것 같다고 말씀해주셔서 같이 정리해 두었던 자료. 
도커가 무엇이고 어떻게 사용하는지에 대해서는 조만간 다시 정리해보아야겠다. 별도로 표기하지 않은 모든 현장실습 관련 글은 현장실습을 수행했던 김무성(최고의 선배님)과 같이 공동작업한 자료! 

# Docker 명령어

### docker image
**이미지 설치**
```
docker pull [이미지 이름]
ex) docker pull bitnine/agensgraph
```

**이미지 목록 출력**
```
docker images 
```

**이미지  삭제**
```
docker rmi [이미지 이름]:[태그] / docker rmi [이미지ID]
docker rmi -f [이미지ID] - 이미지와 컨테이너까지 삭제
```

### docker container
**컨테이너 실행**
```
docker run [옵션] [이미지이름] [명령어] [인자]
docker exec -it [이미지 이름] /bin/bash
```
	[옵션]	
    -d: 컨테이너를 백그라운드에서 실행. detached 모드에서 실행, 실행결과로 컨테이너 ID만을 출력
	-it: -i 옵션과 -t옵션을 같이 쓰는 경우로 두 옵션은 컨테이너를 종료하지 않은 상태로 터미널의 입력을 계속해서 컨테이너로 전달하기 위해 사용
	--name: 컨테이너 이름을 부여하여 식별하기 용이
	-e:환경변수 설정
	-p: 호스트와 컨테이너 간의 포트 배포(publish)/바인드(bind)를 위해서 사용. 호스트 컴퓨터에서 컨테이너에서 리스닝하고 있는 포트로 접속할 수 있도록 설정. host os의 포트 하나에는 하나의 컨테이너만 연결 가능
	-v: 호스트와 컨테이너 간의 볼륨 설정을 위해 사용. 호스트 컴퓨터의 파일 시스템의 특정 경로를 컨테이너의 파일 시스템의 특정 경로로 마운트 해줌 -> 호스트 파일을 컨테이너에 공유 docker -v [호스트 경로]:[컨테이너 경로] [이미지이름]
	-w: Dockerfile의 WORKDIR 설정을 덮어쓰기 위해 사용
	--entrypoint: Dockerfile의 ENTRYPOINT 설정을 덮어쓰기 위해 사용
	--rm: 컨테이너를 일회성으로 실행할 때 사용. 컨테이너가 종료될 때 컨테이너와 관련된 리소스(파일 시스템, 볼륨)까지 깨끗이 제거


**컨테이너 목록 출력**
```
docker ps - 실행ㅇ 중인 컨테이너 목록 출력
docker ps -a,-all - 모든 컨테이너 목록 출력
```

**컨테이너 시작**
```
docker start [컨테이너이름] - 정지한 컨테이너를 다시 시작
```

**컨테이너 재시작**
```
docker restart [컨테이너이름] - os 재부팅처럼 컨테이너 재시작
```

**컨테이너 접속**
```
docker attach [컨테이너 이름 or ID] - 시작한 컨테이너에 접속

```
**컨테이너 정지**
```
docker stop [컨테이너 이름] 

```
**컨테이너 삭제**
```
docker rm [컨테이너 이름/컨테이너 ID] 
docker rm -f - 실행중인 컨테이너 강제 삭제
docker rm -f $(docker ps -aq) - 컨테이너 전체 삭제
```

**외부에서 컨테이너 안의 명령 실행**
```
docker exec [컨테이너이름] [명령] [매개변수]
```

**컨테이너 종료**
```
exit / Ctrl + d 
컨테이너 중지 및 종료. 종료 후 컨테이너의 status가 Exited가 된다. (docker pa -a로 확인)	

Ctrl + p, Ctrl +q 
컨테이너를 백그라운드 실행시킨 상태로 종료. status가 Up이다.
(docker ps -a로 확인)  
```

**컨테이너에서 파일 꺼내기**
```
docker cp [컨테이너이름]:[경로] [호스트 경로]
```

**컨테이너 연결**
```
웹 서버 컨테이너와 DB컨테이너가 있을 때, 웹 서버 컨테이너에서 DB컨테이너에 접근할 수 있어야 함
```

**DB 컨테이너 생성**
```
docker run --name [DB컨테이너이름] -d [DB이미지이름]
```

**web 컨테이너 생성 및 db 컨테이너와 연결**
```
docker run --name [Web컨테이너이름] -d -p 80:80 --link [DB컨테이너이름]:[별칭] [Web이미지이름]
```

**컨테이너 커밋**
```
사용 중인 컨테이너를 이미지로 push
docker commit <옵션> <컨테이너 이름> <이미지 이름>:<태그>
docker hub에 가입 후 docker login로 로그인
docker tag <이미지 이름>:<태그> <Docker 레지스트리 URL>/<이미지 이름>:<태그>로 태그 지정(생략 가능)
docker push <Docker 레지스트리 URL>/<이미지 이름>:<태그>
```
### ++
**디렉토리 연결(마운트)**
```
docker run -it -v [호스트 디렉토리 경로]:[컨테이너 디렉토리 경로] [컨테이너]

```
**도커 파일 복사 (로컬 <-> 컨테이너)**
```
docker cp [로컬 파일 주소] [컨테이너ID]:[컨테이너 폴더 주소]
로컬 -> 컨테이너 파일 복사
docker cp [컨테이너ID}:[컨테이너 파일 주소] [로컬 폴더 주소]
컨테이너 -> 로컬 파일 복사

```
**도커 폴더 복사 (로컬 <-> 컨테이너)**
```
docker cp [로컬 폴더 주소+/.] [컨테이너ID]:[컨테이너 폴더 주소]
/.  <-  폴더 내의 모든 파일을 뜻함
컨테이너 안에 새로운 폴더 생성하여 폴더 내의 모든 파일을 복사하는 것이 가능
로컬 -> 컨테이너 폴더 복사
docker cp [컨테이너ID}:[컨테이너 폴더 주소+/.] [로컬 폴더 주소]
컨테이너 -> 로컬 폴더 복사

```

**주피터 노트북**

```
docker run --name jp -d -p 8080:8888 -it jupyter/datascience-notebook - - <주피터 노트북 컨테이너 생성>
ㅇ - <주피터 컨테이너 쉘 실행>
docker logs jp - <주피터 컨테이너 초기 비밀번호 확인>
localhost:8080 - <주피터 컨테이너 웹 실행>
```


# 리눅스 명령어
	**Linux commands cheat sheet** 을 검색하면 더 많은 정보가 있다.

### 파일 시스템 탐색을 위한 리눅스 명령어

**pwd** (Print Work Directory): 현재 작업 중인 디렉토리를 보여줌
**ls** (list segments): 파일과 디렉토리의 모든 정보를 제공. 특정 디렉토리와 특정 파일의 내용도 제공
**cd** (change directory): 현재 디렉토리를 기준으로 해당 디렉토리로 이동하는 방법
**mkdir** (make directory): 새 디렉토리(새 폴더)를 만듦
**rmdir** (remove directory): 빈 디렉토리를 삭제할 때 사용 (디렉토리가 비어있지 않을 경우 삭제x)
**lsblk**: linux 시스템에서 사용 가능한 블록 장치를 나열해야 하는 경우 사용. 블록 장치의 트리 구조를 나타냄
**mount**: 기존 파일 시스템으로 마운트
**df**: 파일시스템의 디스크 공간에 대한 필수 정보를 표시

### 시스템 조작을 위한 리눅스 명령어

**uname**: 이름, 버전 및 기타 시스템 특정 세부 사항과 같은 시스템 정보를 얻기 위한 명령어
**ps**: 현재 시스템에서 실행 중인 프로세스 시각화
**kill**: 자원 제한으로 인해 멈춘 프로세스를 중지하는 방법
**service**: 시스템 전체 서비스를 호출하기 위한 명령어
**batch**: 미리 정의된 일정에 따라 시스템 서비스를 실행하는 깔끔한 도구. 자동화 쉘 스크립트 작성을 위한 명령어
**shutdown**: 시스템을 종료하는 명령어. 현재 접속 중인 모든 사용자에게 시스템이 종료된다는 메시지를 보낼 수 있음

### 파일 관리를 위한 리눅스 명령어

**touch**: 유효한 빈 파일을 작성하기 위한 명령어
**cat**: 새 파일을 작성하고 터미널에서 파일내용을 보고 출력을 다른 명령행 도구나 파일로 리디렉션하는데 사용
**head**: 터미널에서 직접 파일 또는 파이프 된 데이터의 시작을 볼 수 있음
**tail**: 파일의 마지막 행을 기준으로 지정한 행까지의 파일 내용 일부를 출력
**cp** (copy): 시스템에서 파일이나 디렉토리를 한 폴더에서 다른 폴더로 복사하도록 지시하는 명령어
**mv** (move): 하나 또는 여러 파일을 한 위치에서 다른 위치로 이동하는 명령어
**comm**: 두 개의 파일을 공통 행과 구별되는 행으로 비교. 터미널에서 많은 양의 파일을 처리해야할 때 필수적인 명령어
**less** (cat이랑 유사): 파일의 내용을 볼 때 편리. 터미널 세션을 방해하지 않으면서 파일 내에서 양방향으로 탐색
**ln**: 특정 파일에 대한 심벌릭 링크를 만들기 위한 가장 편리한 명령어. 디스크 공간의 특정 파일이나 디렉토리에 대한 심벌릭 링크의 여러 인스턴스를 생성할 수 있음
**cmp** (comm이랑 유사): 두 파일을 비교하고 결과를 표준 출력 스트림에 인쇄
**dd**: 파일을 한 유형에서 다른 유형으로 복사 및 변환하기 위해 사용
**alias**: 터미널에서 직접 파일의 다른 문자열로 단어를 바꿀 수 있음
쉘을 사용자 정의하고 환경 변수를 조작할 수 있음

### 리눅스 명령어 검색 및 정규 표현식

**find**:
터미널에서 파일을 검색하는데 가장 많이 사용되는 명령어
파일 권한, 소유권, 수정 날짜, 크기 등과 같은 특정 기준에 따라 파일 검색
**which**:
검색하려는 모든 파일이 실행 파일인 경우 유용
특정 매개 변수를 취하여 $ PATH 시스템 환경 변수에서 이진 파일을 효과적으로 검색
**locate**:
특정 파일의 위치를 찾는데 사용되는 명령어
**grep**:
대량의 텍스트 파일에서 패턴을 검색할 때 사용할 수 있는 정규식 터미널 명령
찾고자 하는 패턴을 입력 받아 특정 패턴에 대해 지정된 파일 검색
**sed**:
지정된 부분을 교체하여 파일 또는 스트림의 각 줄을 조작하는데 가장 많이 사용되는 명령어
많은 양의 텍스트 데이터를 다루고 이동 중에도 변경할 때 사용

### I / O 및 소유권을 다루는 리눅스 명령어

**clear**:
기존 터미널 화면을 지우는데 사용
**echo**:
터미널 콘솔에 특정 텍스트를 출력할 수 있는 명령어
출력을 다른 터미널 명령으로 파이프 할 수 있음
**sort**:
사전 순, 역순으로 파일을 정렬할 때 사용
**sudo**:
권한이 없는 사용자를 권한이 필요한 파일에 액세스하고 수정할 수 있게 해줌
일반 사용자 계정에서 루트에 엑세스 함
**chmod**:
시스템 파일 or 객체의 액세스 권한을 변경하거나 수정하는데 사용
사용자로부터 매우 다양한 매개 변수 세트를 취하는게 가능
**chown**:
chmod와 유사, 사용자가 파일 or 디렉토리의 소유권을 변경
chmod 및 chown 터미널 명령은 모두 루트 권한이 필요

### 일상적인 사용을 위한 기타 명령어

**man**: manual을 나타냄
**tar**: 파일을 아카이브하고 추출하는데 사용. 파일을 압축하는데 사용되는 명령
**whatis**: 사용자가 제공한 간단한 설명으로 데이터베이스 세트를 순회하며 해당 데이터베이스 명령과 일치하는 시스템 명령을 인쇄



## 참조 목록

[초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
[Docker 기본 사용법](http://pyrasis.com/Docker/Docker-HOWTO)
[Docker 명령어 사용법](https://www.daleseo.com/docker-run/)
[Linux 명령어 사용법](https://dora-guide.com/linux-commands/)
[윈도우 폴더 공유](https://pythonkim.tistory.com/55)
[포트 개념](https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=alice_k106&logNo=220278762795)