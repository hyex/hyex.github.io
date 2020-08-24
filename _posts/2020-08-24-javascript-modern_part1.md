---
layout: post
title:  "[Modern JS Tutorial] Part 1"
date:   2020-08-24 18:26:00 +0900
categories: javascript
---

# Part 1. 코어 자바스크립트
[Link](https://ko.javascript.info/)

## 1.1 자바스크립트란?

'웹페이지에 생동감을 불어넣기 위해' 만들어진 프로그래밍 언어
자바스크립트로 작성한 프로그램을 **스크립트**라고 부른다. 스크립트는 HTML안에 작성할 수 있으며, 웹페이지를 불러올 때 실행된다.

### 실행환경
브라우저, 서버, 자바스크립트 엔진이라는 특별한 프로그램이 들어있는 모든 디바이스
#### 엔진의 종류
**V8** Chrome, Opera
**SpiderMonkey** Firefox
IE는 버전에 따라 **Trident** 나 **Chakra**라 불리는 엔진을 사용한다. **ChakraCore**는 Microsoft Edge에서 사용되며, **SquirrelFish**는 Safari에 사용된다.

#### 엔진 동작 기본 원리
1. Parsing 엔진이 스크립트를 읽는다.
2. Compile 읽어 들인 스크립트를 기계어로 전환
3. 기계어로 전환된 코드가 실행 (속도가 빠르다)

### 브라우저에서 할 수 있는 일
웹페이지 조작, 클라이언트와 서버의 상호작용에 관한 모든 일

### 브라우저에서 할 수 없는 일
보안을 위해 몇 가지 제약을 걸어놓았다.
- 웹페이지 내 스크립트는 디스크에 저장된 파일을 읽기/쓰기/복사/실행 할 때 제약을 받을 수 있다.
- 브라우저 내 탭과 창은 대개 서로의 정보를 알 수 없다. 그러나 자바스크립트를 사용해 한 창에서 다른 창을 열 때는 예외가 적용된다. 하지만 이 경우에도 도메인, 프로토콜, 포트가 다르다면 불가능하다. 이 제약사항을 **동일 출처 정책(Same Origin Policy)** 라고 부른다.
- 타 사이트나 도메인에서 데이터를 받아오는 것은 불가능하다. 

다만, 모던 브라우저에선 추가 권한 허가를 요청하는 플러그인이나 익스텐션 설치가 허용된다.

### 자바스크립트만의 대표 강점
- HTML/CSS와 완전히 통합 가능
- 간단한 일은 간단하게 처리할 수 있다.
- 모든 주요 브라우저에서 지원하고, 기본 언어로 사용됨

### 자바스크립트 너머의 언어들
자바스크립트 문법이 모두를 만족시키지는 못하기 때문에, 브라우저에서 실행 되기 전에 자바스크립트로 transpile(변환) 할 수 있는 새로운 언어들이 등장했다.
최신 툴을 사용하면 transpile을 빠르고 명확하게 수행할 수 있따. 최신 도구는 자바스크립트 이외의 언어로 작성한 코드를 보이지 않는 곳에서 자바스크립트로 자동 변환해준다.
대표적으로 CoffeeScript, TypeScirpt(개발을 단순화하고 복잡한 시스템을 지원하려는 목적으로 Strict data typing에 집중, Microsoft가 개발)가 있다.

## 1.2 메뉴얼과 명세서

### 명세서
[ECMA-262 명세서](https://www.ecma-international.org/publications/standards/Ecma-262.htm)는 자바스크립트라는 언어를 정의한다.

### 메뉴얼
Mozilla 재단이 운영하는 MDN JavaScript Reference엔 다양한 예제와 정보가 있다. 특정 함수나 메서드에 대한 깊이 있는 정보를 얻고자 할 때 적합하다.
Microsoft가 운영하는 MSDN도 좋은 사이트이다. Internet Explorer에 관련된 정보를 찾고자 할 때 유용하다.

### 호환성 표
특정 브라우저나 엔진이 내가 사용하려는 기능을 지원하는지 확인할 땐, 아래 두 사이트가 좋다.
-   [http://caniuse.com](http://caniuse.com/)  에선 브라우저가 특정 기능을 지원하는지 (표 형태로) 확인할 수 있습니다. 암호화 관련 기능인 cryptography를 특정 브라우저에서 사용할 수 있는지 아닌지를 보려면  [http://caniuse.com/#feat=cryptography](http://caniuse.com/#feat=cryptography)를 확인하면 됩니다.
-   [https://kangax.github.io/compat-table](https://kangax.github.io/compat-table)  에선 자바스크립트 기능 목록이 있고, 해당 기능을 특정 엔진이 지원하는지 여부를 거대한 표를 통해 보여줍니다.


## 1.3 코드 에디터

코드 에디터는 크게 IDE와 경량 에디터로 나뉜다.

### 통합 개발 환경
프로젝트 전체를 관장하는 다양한 기능을 제공. 즉 프로젝트 레벨에서 작동한다.
- VS code (크로스 플랫폼, 무료)
- WebStrom (크로스 플랫폼, 유료)

### 경량 에디터
속도가 빠르고 단순하다는 장점이 있다. 파일 하나만 수정하고자 할 때 적합하다. Atom, VS Code, Sublime Text 등이 속한다.


## 1.4 개발자 콘솔
브라우저엔 **개발자 도구** 라는 것이 내장되어 있다. Chrome이나 Firefox에서 제공하는 개발자 도구는 굉장히 훌륭하다. 

### Chrome
**Shortcut** `Cmd+Opt+J` or `fn+F12`

 