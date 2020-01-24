# Git 전략
- Production 브랜치에서 새로운 release 브랜치를 딴 후, master 브랜치에서 변경된 점들을 cherry-pick을 이용하여 commit후, production에 배포한다.
- -> master 브랜치에서 변경된 점들은 내가 Merge한 PR이 될 것이다. 이 PR은 Master의 commit hash를 통해 cherry-pick 한다.

## 이렇게 하는 이유?
Q: master를 그냥 production에 배포하지 않는 이유가 있나요? 어차피 master브랜치에는 approve된 코드들만 있을텐데요?

A: master 브랜치는 stable한 코드임을 보장하기는 하나, 배포되어서는 안되는 코드들이 존재합니다

- API, microservice 간의 호환/의존성
- 인프라 준비
- 기타 비즈니스적인 이유

Q: 여기서 master 브랜치에 왜 그런 코드들이 존재하게 되는건가요? 그럼 master 브랜치는 약간 개발 브랜치라고 생각하면 되는걸까요?

A: - 답변 드리겠습니다.
```
코드가 개발이 되었으나 데이터베이스 스키마 변경이나 써드파티 서비스가 변경되어야 배포될 수 있는 코드들이 있죠
만약에 orm 을 사용하는데 orm 의 entity 가 코드상에서는 변경되었지만 실제 사용할 database 에서 스키마가 변경되어 있지 않다면
둘이 싱크가 안맞아서 앱이 깨질수도 있는 상황이 발생할 수 있죠
그럴 때는 데이터베이스에 먼저 스키마를 변경해주고 앱을 배포 해야하는 상황이 있을수 있는거죠
```
***- By Yonug Of Hungry***

## production에 배포하는 법

1. production branch 에서 release/veriosn branch 를 새로 땁니다.
2. master branch 에 있는 production 으로 올릴 commit 을 master branch 에서 release/version branch 로 cherry-pick 합니다.
3. release/verions branch 를 push 하고 release/verions branch 에서 production branch 로 PR 을 만듭니다.
4. 만든 PR 을 merge 합니다.
5. -끝-

**→ cherry-pick할 때, 스쿼시 된 pr의 커밋해시 말고 마스터에서 찾아서 써야한다.**

→ production과 master의 차이가 있으므로 compare & pull request는 계속 뜬다. 오류가 아니다.

-> production 브랜치를 pull 해올 때, 다음과 같이 pull 해오면 된다. (원격 저장소 pull 커맨드)
``
git checkout -t origin/production
``

## Refer
- Git 원격 저장소 pull
https://cjh5414.github.io/get-git-remote-branch/