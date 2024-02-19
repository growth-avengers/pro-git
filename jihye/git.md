https://github.com/growth-avengers/pro-git

> [PRO GIT](https://git-scm.com/book/ko/v2)
> - 7주 목표
> - 1/29~3/17

# 4. Git 서버
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

# 5. 분산 환경에서의 Git
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