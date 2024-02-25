# ch1-Getting Started
## 버전 관리 시스템이란
- 파일 변화를 시간에 따라 기록했다가 나중에 **특정 시점의 버전을 다시 꺼내올 수** 있는 시스템
    - 이슈 추적
    - 파일 변화 비교
    - 롤백 등을 쉽게 할 수 있다.
### 분산 버전 관리 시스템
- 이전 시스템(중앙 집중식 버전 관리)과 비교해서 서버에 문제가 생기더라도 클라이언트의 복제물로 작업할 수가 있다.
## GIT의 핵심
- **데이터**를 **스냅샵의 스트림**으로 취급하는 것.
## checksum
- 깃에서 사용하는 atomic 데이터 단위이자 철학
- 체크섬 기반으로 데이터 관리를 해서 무결성이 보장된다.
### 깃 파일의 세 가지 상태
- modified
    - 아직 staging area에 추가하지 않은 상태
- staged
    - staging area에 추가한 상태
- committed
    - git directory에 있는 상태
### git directory
- 깃이 프로젝트의 메타 데이터, 객체 데이터베이스를 저장하는 곳
- clone하면 이 git directory가 만들어진다.
    - 프로젝트의 히스토리를 전부 받아옴
### 깃으로 하는 일
- working tree에서 파일 수정
- staging area에 파일을 staging 해서 커밋할 스냅샵을 만든다.
- staging area에 있는 파일들을 커밋해서 git directory에 영구적인 데이터 스냅샵으로 저장한다.

>Q. 깃은 어떤 방식으로 무결성이 보장되나요?
# ch2-Git Basics
## working directory에 있는 파일의 상태
- untracked(관리 대상 X)
    - 스냅샵에도, staging area에도 포함되지 않은 파일
- tracked
    - unmodified
    - modified
    - staged(커밋으로 저장소에 기록할)
### 커밋하고 기록을 남기지 않고 커밋 내역을 수정하고 싶을 때
```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
- 커밋 한 개로 기록되고 두 번째 커밋이 첫 번째 커밋을 덮어 쓴다.
### 따로 커밋 하려고 했는데 같이 커밋 됐을 때
```
git add * 
git reset HEAD readme.md (--hard 워킹 디렉토리 파일까지 reset한다.)
```
-> add 명령어로 staging 상태로 만들었다가 reset 명령어로 다시 unstaging 상태로 바꾸는 것
## 리모트 저장소
- 리모트 저장소 추가하기
    - git remote add 단축이름 url 명령을 사용한다.
```
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
```

- 로컬 저장소에는 없는데 pb에 있는 걸 가져오려면?
```
git fetch pb
```

>Q. fetch와 pull 의 차이점은 무엇인가요?
# ch3-Git Branching
- Git은 데이터를 Change Set이나 변경사항(Diff)으로 기록하지 않고 **일련의 스냅샷으로 기록한다는 것**
- 커밋하면 Git은 현 Staging Area에 있는 **데이터의 스냅샷에 대한 포인터**, 저자나 커밋 메시지 같은 **메타데이터**, **이전 커밋에 대한 포인터** 등을 포함하는 커밋 개체(커밋 **Object**)를 저장한다.
- Git의 **브랜치**는 어떤 한 커밋을 가리키는 40글자의 SHA-1 **체크섬 파일**에 불과하기 때문에 만들기도 쉽고 지우기도 쉽다.
    - 새로 브랜치를 하나 만드는 것은 41바이트 크기의 파일을(40자와 줄 바꿈 문자) 하나 만드는 것에 불과하다.
## 브랜치와 Merge 의 기초
### Merge 방식 - “fast-forward”
- A 브랜치에서 다른 B 브랜치를 Merge 할 때 B 브랜치가 A 브랜치 이후의 커밋을 가리키고 있으면 그저 A 브랜치가 B 브랜치와 동일한 커밋을 가리키도록 이동만 시키는 것 (**Merge 과정 없이 그저 최신 커밋으로 이동**)
### Merge 방식 - "3-way Merge"
- Merge하는 두 브랜치에서 같은 파일의 한 부분을 동시에 수정하고 Merge하면 Git은 해당 부분을 Merge 하지 못한다. -> Conflict 발생

## 브랜치 관리
- 현재 브랜치 기준으로 --merged, --no-merged 옵션을 사용해서 브랜치 목록을 필터링할 수 있다.
```
git branch --merged
git branch --no-merged
```
## Rebase 이해하기
- merge와 rebase 둘 다 하나의 브랜치에서 다른 브랜치로부터의 변경 내용을 통합하기 위해 설계되었다.
### merge
```
git checkout feature
git merge main

git merge feature main
```
- 장점
    - 비파괴적인(non-destructive) 한 방식
        - 기존 브랜치는 어떤 방식으로도 변경되지 않음
          -> rebasing 에서 발생하는 문제 방지
- 단점
    - feature branch에 불필요한 merge commit이 생성된다.
      -> main 브랜치의 변경이 잦다면, 내 작업 브랜치 이력이 merge commit으로 인해 오염될 수 있다.
### rebase(브랜치의 base를 옮긴다.)
- 모든 featrue 브랜치를 main 브랜치의 끝 지점에서부터 시작하도록 옮기는 것으로 모든 새로운 커밋이 main 브랜치에 통합되도록 한다.
- merge commit을 사용하는 대신, rebase는 원본 브랜치 각각의 커밋에 대해 새로운 커밋으로 생성함으로써 프로젝트의 이력을 다시 쓴다.
```
git checkout feature
git rebase main
```
- 장점
    - 프로젝트 이력을 깔끔하게 관리할 수 있다.
        - merget commit 제거
- 단점
    - merge commit 이 제공하는 컨텍스트를 잃게 된다.(upstream changes가 언제 featrue에 통합되었는지 알 수가 없음)

> Q. merge가 유용한 경우와 rebase가 유용한 경우는 각각 언제일까요?

# ch4-Git 서버
- 프로토콜 종류: local, http, ssh, git

### local
장점
- 설정하기 쉽다.
- 동료가 작업한 코드를 가져오는 것도 쉽다.

단점
- 리모트 저장소가 있는 디스크를 마운트해야만 push 할 수 있는데 다른 프로토콜보다 느리고 어렵다.
- 파일 보호 전략이 없음

### http
장점
- 사용하기 쉬운 인증 시스템
- 빠르고 효율적

단점
- https 를 사용하도록 설정하는 것이 ssh로 설정하는 것보다 복잡한 서버가 있다는 것.

### ssh
장점
- 상대적으로 설정하기 쉬움.

단점
- 익명 접근 불가

### git
장점
- 전송 속도가 가장 빠르기 때문에 별도 인증이 필요 없고 읽기만 허용하는 프로젝트에서 유용하다.

단점
- 인증 메커니즘이 없음

> Q. 왜 git 의 대표 프로토콜은 ssh일까요? 

# ch5-분산 환경에서의 Git
## 분산 환경에서의 work-flow

### 중앙 집중식
- 중앙 저장소 하나
- 변경 사항은 모두 하나의 중앙 저장소에 집중된다.
- merge 전 fetch 를 해야 push 할 수 있다.

### Integration-Manager
- 프로젝트를 fork하고 수정사항을 반영하여 다시 모두에게 공개하기 좋은 구조
![image](https://git-scm.com/book/en/v2/images/integration-manager.png)

### Dictator and Lieutenants
- 일반적이진 않지만 리눅스 커널 프로젝트에서 사용하는 방식
![image](https://git-scm.com/book/en/v2/images/benevolent-dictator.png)

> Q. 좋은 커밋 메시지는 어떤 거라고 생각하시나요? 업무 시 커밋의 단위는 어떻게 가져가는 편인가요?
# 6. github
> Q. github에서 local로 clone 할 때 ssh, https 중 어떤 걸 이용하는 편이고, 그 이유가 뭔가요?