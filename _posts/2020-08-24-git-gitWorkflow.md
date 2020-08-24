---
layout: post
title:  "Git workflow"
date:   2020-08-24 18:35:00 +0900
categories: git
---

# Git Workflow
형상관리도구인 Github를 어떻게 사용할 지에 대한 방법(?)이다. 유명한 예로 gitflow, git workflow, gitlab workflow 등이 있다.


## Git flow 
: 주로 대규모의 프로젝트에서 사용하는 엄격한 방식

항상 유지되는 메인 브랜치 - `master`, `develop`
일정 기간 동안만 유지되는 보조 브랜치 - `feature`, `release`, `hotfix`

1.  Master Branch - 제품으로 출시될 수 있는 브랜치
	-   배포(Release) 이력 관리를 위해 사용. 즉, 배포 가능한 상태만을 관리
2.  Develop Branch - 다음 출시 버전을 개발하는 브랜치
	-   기능 개발을 위한 브랜치들을 병합하기 위해 사용.
	-   모든 기능이 완성되면 `master` branch에 merge한다.
3.  Feature Branch - 기능 개발
	-   새로운 기능 개발 및 버그 수정이 필요할 때 `develop` branch로부터 분기
	-   개발 완료 후 `develop` branch에 merge
	-   naming : feature/기능요약
	-   `--no-ff` 옵션 : feature branch에 존재하는 커밋 이력을 모두 합쳐 하나의 새로운 커밋 객체를 만들어 merge
4. Release Branch - 이번 출시 버전을 준비하는 브랜치
	-   배포를 위한 전용 브랜치
	-   한 팀이 해당 배포를 준비할 때, 다른 팀은 다음 배포를 준비할수록 있도록 함
	-   직접 관련된 작업들을 제외하고, 새로운 기능을 추가로 merge하지 않는다.
5.  Hotfix Branch - 출시 버전에서 발생한 버그를 수정하는 브랜치
	-   배포한 버전에 긴급 수정이 필요한 경우, `master` branch에서 분기
	-   문제 해결 후 다시 `master` branch 에 merge
	-   `hotfix` 브랜치에서의 변경사항은 `develop` branch에도 merge