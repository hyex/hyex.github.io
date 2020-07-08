---
layout: post
title:  "EGoing의 Web2 - Node.js 시작"
date:   2020-07-08 17:17:00 +0900
categories: nodejs
---
# WEB2 Node.js

## 발단
 1. 태초에 JavaScript는 웹 브라우저 위에서만 작동
 2. 2008, Google에서 JS 성능 개선을 위해  V8 엔진을 공개
 3. Ryan Lienhart Dahi가 V8을 기반으로 하는 <b>Node.js</b> 를 개발
 4. 웹 브라우저만 제어할 수 있던 JS가 이제 Node.js를 통해 컴퓨터 자체를 제어할 수 있게 됨 (마치 Pythoh, Java, PHP와 같이)
 5. JS에 익숙한 웹 개발자들의 폭발적인 반응, 웹 페이지를 자동으로 생성하는 Web Application을 만들고자 하면서 Node.js의 발전이 시작되었다.

## HTML의 한계
### 첫 번째
- 웹 페이지가 1억개라면?
- 수많은 페이지의 아주 작은 내용을 모두 바꿔달라는 요청이 생긴다면?

--> 1억개의 페이지를 하나하나 고쳐야 한다.
--> <b>Node.js 를 사용하면</b>, 한 번에 고칠 수 있다. 즉, <b>생산성을 높일 수 있다! </b> 

### 두 번째
- 사용자가 글을 작성하고 싶다면?
- 사용자가 글을 수정, 삭제하고 싶다면?

--> 원래는 불가
--> <b>Node.js를 사용하면</b>, 사용자가 직접 자신의 컨텐츠를 제어할 수 있다.

## Node.js