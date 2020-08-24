---
layout: post
title:  "[Node.js-readline] 콘솔에서 입력받기"
date:   2020-08-24 18:26:00 +0900
categories: javascript
tags: nodejs
---

# Node.js 콘솔에서 입력받기

사실 Javascript나 Node,js 모두 콘솔에서 입력받는 것은 흔치 않다. 주로 웹 프론트엔드에서 입력을 받아 사용하는 일이 대부분이기 때문이다. 그럼에도 이 글을 작성하는 이유는, 알고리즘을 풀 때나 JS 지식을 기르기 위한 공부를 할 때 입력이 필요한 경우마다 매번 구글링을 했던 과오를 청산하기 위해서이다.

## Readline
구글링 시에 가장 많이 나오는 샘플이며, 나또한 이 방법을 사용해왔다.

### 한 번 입력받고 종료하기
```js
const readline = require('readline')
const rl = readline.createInterface({
	input: process.stdin,
	output: process.stdout
})

rl.on("line", function(line) {
	console.log("input: ", line)
	rl.close()
})
rl.on("close", function() {
	process.exit()
})
```

### 여러 번 입력받기
```js
const readline = require('readline')
const rl = readline.createInterface({
	input: process.stdin,
	output: process.stdout
})

rl.setPrompt("> ")
rl.prompt()
rl.on("line", function(line) {
	console.log("input: ", line)
	switch(line) {
		case "quit":
			rl.close()
		default:
			rl.prompt()
	}
})
rl.on("close", function() {
	process.exit()
})
```
여러 번 입력받을 경우에는, `prompt` 함수를 사용하는 게 사용자가 입력하기에 좋은 것 같아 이렇게 사용해왔다. 또한 `switch` 문에서 알 수 있듯이, 사용자가 `quit` 를 입력하면 종료하고 그 외에도 `ctrl+C` 또는 `ctrl+D` 입력 시 종료된다.