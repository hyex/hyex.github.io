---
layout: post
title:  "[OpenSource Contribution] Eslint-plugin-jsdoc #569 (first)"
date:   2020-09-08 01:47:00 +0900
categories: unclassified
---

## Contents
- [How to start](#how-to-start)
- [ESLint-plugin-jsdoc](#eslint-plugin-jsdoc)
- [Issue 해결 순서](#issue-해결-순서)
- [Issue #569](#issue-569)
- [Issue 해결 과정 ](#issue-해결-과정)
- [Pull Request](#pull-request)
- [소감](#소감)

## How to start

이전부터 오픈소스에 대한 관심은 있었으나, 직접 컨트리뷰션을 해볼 엄두가 나지 않았다. 운좋게도 8월 초부터 [오픈소스 컨트리뷰톤](https://www.oss.kr/contributhon) 에 `ESLint` 프로젝트 팀에 합류하게 되었고, 멘토님에게 많은 도움을 받으며 컨트리뷰션에 성공할 수 있었다. 

## ESLint-plugin-jsdoc

먼저 내가 기여한 오픈소스인 `ESlint-plugin-jsdoc` 는  `ESLint` 의 플러그인 중 하나로,  `JSDoc` 에 좀 더 구체화된 규칙들을 가지고 있다. `ESLint` 의 몇몇 플러그인을 직접 클론받고 테스트해보면서 `ESLint-plugin-jsdoc` 의 다음과 같은 장점을 느낄 수 있었다.

1. **꼼꼼하고, 자동화가 잘되어 있다.** 예를 들어 `git push` 를 할 때 모든 테스트를 수행하고, README가 업데이트 되었는지 확인하고, 업데이트가 안되어있다면 자동 업데이트가 가능한 스크립트가 준비되어 있다. 물론 주관적인 견해이고 다른 플러그인보다 규모가 작아서 그럴 수도 있지만, 나와 같은 초보 컨트리뷰터가 놓치기 쉬운 부분을 잡아주어 좋았다.
2. **메인테이너가 굉장히 활발하다.** 몇 주 전에 다른 플러그인에 먼저 PR을 보냈으나, 아직 코멘트가 달리지 않았다. 반면 이 플러그인에 보낸 PR은 당일, 1시간 정도후에 바로 코멘트가 달렸으며 이후에도 빠른 상호소통이 가능했다. 뿐만 아니라 대기중인 PR의 개수가 매우 적고 commit log가 계속 업데이트되는 게 보였다.
 
 
## Issue 해결 순서

1. issue 이해하기
2. 개발 환경 준비 (fork, clone, branch 생성, `npm install`)
3. 프로젝트 디렉토리 파악
4. Testing
	- 전체 테스팅, 단일 파일 테스팅
	- 테스트 케이스 추가
5. 기능 구현/버그 수정 (크롬 디버깅)
6. 테스트 케이스 추가
7. Docs 수정
8. PR !

대략 이런 순서로 Issue를 해결했다. 단일 파일 테스팅이나 크롬 디버깅하는 게 각 프로젝트마다 명령어가 달라서 어려움을 겪었고, 거의 멘토님이 해결해주셨다 ㅎㅎ *(대부분 `babel-register` 쪽 문제였다. 더 공부하기!)*
 
## Issue #569

**이슈 [#569]([https://github.com/gajus/eslint-plugin-jsdoc/issues/569](https://github.com/gajus/eslint-plugin-jsdoc/issues/569)) 은 `require-param` 규칙에서 destructuring 된 파라미터에 대해 타입 검사를 하지 않아 잘못된 린트 에러가 발생한 이슈이다.**

`require-param` 규칙은 모든 함수의 parameter가 문서화되어야 한다는 규칙이다. 이 규칙은  

```js
checkTypesPattern=/^(?:[oO]bject|[aA]rray|PlainObject|Generic(?:Object|Array))$/
```  

을 기본값으로 가지고 있는데, 이 패턴에 일치하는 타입일 때 type 체크를 통해 destructuring을 할지 말지 결정한다는 의미로 이해하면 될 것 같다.

```js
/** * Description. 
* @param {object} options Options. 
* @param {FooBar} options.a A description.
*/ 
function foo({ a: { b } }) {}
```
이 예시에서, `FooBar` 라는 타입은 `checkTypesPattern` 에 일치하지 않는 타입이므로 해당 parameter의 destructuring이 필요하지 않다. 즉, 린트 에러가 발생하지 않아야 한다.

```bash
Missing JSDoc @param "options.a.b" declaration.
```
하지만 위 린트 에러가 발생했으며 따라서 에러가 안나도록 버그를 수정하면 된다.


## Issue 해결 과정

Javascript 자체를 입문한 지 얼마 안되어서 JSDoc, Destructruring, @param 모두 처음 접해보았다. 따라서 코드를 건드리기 전에, 노션에 정리를 하면서 개념부터 차근차근 공부했다. 

<img  src="/assets/img/unclassified/contribution1-1.png"  />

이슈에 관련된 규칙인 `require-param` 은 가지고 있는 옵션이 굉장히 많았다. 그래서 코드를 읽기도 어려웠고 실제 코드 작성 시간보다 코드를 이해하고 어떤 부분을 수정해야 할 지 파악하는데 더 많은 시간을 쏟았다. 
나는 멘토님께서 알려주신 Chrome inspect 를 통해 디버깅을 하는 방법이 코드를 이해하기에 가장 좋았다. 터미널에 아래 명령어를 치고 `chrome://inspect` 로 접속하면 된다.
```bash
# 원하는 break point에 debugger; 를 추가하고 아래 명령어를 실행
npm_config_rule=check-param-names npx mocha --inspect-brk -r @babel/register test/rules/index.js
```

디버깅을 거듭하다보니, 내가 수정해야 할 영역이 보였다. function의 parameter 이름과 jsdoc의 @param 이름을 비교하는 부분이 있는데, 한 번 destructuring된 parameter의 내부는 타입을 검사하지 않았다. 

```js
/** * Description. 
* @param {object} options Options. 
* @param {FooBar} options.a A description.
*/ 
function foo({ a: { b } }) {}
```

1. `options` vs `{ a: { b }}` 비교, 타입 검사 (o)
2. `options.a` vs `{ b }` 비교, 타입 검사 (x)

따라서 다음과 같이 작동하도록 코드를 수정했다.

1. `options` vs `{ a: { b }}` 비교, 타입 검사 (o)
2. `options.a` vs `{ b }` 비교, 타입 검사 (**o**), checkTypesPattern에 포함되지 않은 타입이면, `notCheckingNames` 에 이름(`options.a`)을 추가
3. 파라미터 이름이 `notCheckingNames` 에 있는 이름으로 시작한다면 해당 파라미터는 더이상 검사하지 않음.

코드를 수정하고, 발생할 수 있는 여러 테스트 케이스를 추가하면서 부족한 부분을 보완했다. 멘토님과 개별 스터디를 통해 코드를 개선하고 필요한 테스트 케이스를 추가했고, **Pull Request**을 보냈다 !

## Pull Request

위에서 말했듯이, [PR](https://github.com/gajus/eslint-plugin-jsdoc/pull/630)을 보내고 밥먹고 오니 코멘트가 달려있었다. 

<img  src="/assets/img/unclassified/contribution1-2.png"  />

> "이슈 #540 이 해결되기 전에는 코드를 추가하기 좀 꺼려지는데, 같이 해결할 수 있니? 할 수 없어도, 이 PR을 계속 살펴볼게"

대략 이런 내용이었다. 사실 이슈 #569 자체에도 #540에 대한 언급이 있었지만 내가 감당하기엔 너무 크고... 어려워보여서 일단 버그부터 해결하고 바로 PR을 날린 거였다 :b 한참을 고민했지만 부스트캠프로 평일을 다 쓰고 있어서 과감하게 못하겠다고 했다. (사실 엄청 찌질거리면서 나중에 해볼게! 라고 했다) 

그 뒤에, 작성한 코드에 대한 리뷰가 올 것이라고 예상했지만 생각하지도 못한 추가 코드 구현 요청이 돌아왔다. 다른 규칙에도 같은 옵션이 있으니 버그가 있으면 같이 수정해달라는 코멘트였는데, 빨리 머지되고 싶은 마음에 하루를 바쳐서 commit을 올렸고, 그로부터 하루 뒤에 Merge 되었다 :)

## 소감

- 컨트리뷰션에 필요한 모든 지식과 경험을 컨트리뷰션을 진행하면서 얻었다. OSS 컨트리뷰톤에 합류하게 되면서 ESLint 를 공부하고 사용하기 시작했고, 이슈 해결 과정에서 import/export, jsdoc, js 문법, git rebase 등을 새롭게 얻은 지식이 많다. 나도 이제 오픈소스 컨트리뷰터라는 뿌듯함도 얻고, 동시에 나 자신도 많이 성장할 수 있는 시간이었다. :)
- 멘토님이 없었더라면,,, 결코 성공하지 못했다. 다시 한 번 너무너무 감사합니다 ๑•‿•๑
