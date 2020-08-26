---
layout: post
title:  "[M.JS.T Part1] 11. Promise와 async, await"
date:   2020-08-26 14:25:00 +0900
categories: javascript
---

# Part 1. 코어 자바스크립트 11. Promise와 async, await

[Link](https://ko.javascript.info/async)

> [11.1 콜백](#11.1-콜백)  
> [11.2 프라미스](#11.2-프라미스)  
> [11.3 프라미스 체이닝](#11.3-프라미스-체이닝)  
> [11.4 프라미스와 에러 핸들링](#11.4-프라미스와-에러-핸들링)  
> [11.5 프라미스 API](#11.5-프라미스-API)    
> [11.6 프라미스화](#11.6-프라미스화)  
> [11.7 마이크로태스크](#11.7-마이크로태스크)  
> [11.8 async와 await](#11.8-async와-await)  

## 11.1 콜백

**Callback Function** 나중에 실행할 함수. 비동기적으로 수행하고자 할 때 사용한다.

### Error Handling: error first callback
```js
function loadScript(src, callback) {
	let script = document.createElement('script')
	script.src = src
	
	script.onload = () => callback(null, script)
	script.onerror = () => callback(new Error("error"))

	document.head.append(script)
}

loadScript(src, (err, script) => {
	if (error) {
	// 에러 처리
	} else {
	// 스크립트 로딩이 성공적으로 끝남
	}
})
```
  
**오류 우선 콜백**
1. `callback`의 첫 번째 인수는 에러를 위해 남겨둔다. 에러가 발생하면 이 인수를 이용해 `callback(err)`가 호출된다.
2. 두 번째 인수(더 추가 가능)는 에러가 발생하지 않았을 때를 위해 사용한다. 원하는 동작이 성공한 경우엔 `callback(null, result1, result2, ...)` 가 호출된다.

이 방법을 사용하면, 단일 콜백 함수에서 에러 케이스와 성공 케이스를 모두 처리할 수 있다.

**그러나 콜백 기반 비동기 처리는 콜백 지옥에 빠지기도 하기 때문에 위험하다.**


## 11.2 프라미스

### Syntax
```js
let promise = new Promise(function(resolve, reject) {
	// executor: 비동기적인 실행
})
```

`resolve` 일이 성공적으로 끝난 경우, 그 결과를 나타내는 value와 함께 호출
`reject` 에러 발생 시, 에러 객체를 나타내는 error와 함께 호출

### Promise 객체 내부 프로퍼티
**State**
`pending` promise 객체가 만들어진 순간부터 resolve 또는 reject 가 호출되기 전까지의 상태
`fulfilled` resolve 호출 후 상태
`rejected`  reject 호출 후 상태
`settled` resolve 또는 reject된 상태를 아우러서 말하는 상태

**Result**
처음엔 `undefined` 이었다가, `resolve(value)`  가 호출되면 `value` 로, `reject(error)` 가 호출되면 `error` 로 변한다,
  
  
### `.then` 핸들러
result를 받아 처리하는 메서드. 첫 번째 인자는 프라미스 이행 성공 시에 실행되는 함수이고, 두 번째 인수는 프라미스가 거부되었을 때 실행되는 함수이다.
```js
promise.then(
	fucntion(result) {}, 
	[function(error) {}]
)
```

### `.catch` 핸들러
에러가 발생한 경우를 다루는 메서드. `then(null, errorHandlingFunc)` 으로 에러를 다루는 것 보다 이 메서드를 사용하는 것이 낫다.

### `.finally` 핸들러
프라미스가 `settled` 상태가 되면 항상 작동하는 메서드. 인자가 없고, 자동으로 다음 핸들러에 결과와 에러를 전달한다.
 
  

## 11.3 프라미스 체이닝

순차적으로 비동기 작업을 여러 개 해결해야 할 때 프라미스 체이닝을 이용한다. 이 방식은 `result` 가 `.then` 핸들러의 체인(사슬)을 통해 전달된다는 점에서 착안한 아이디어이다.
ex)
```js
new Promise((resolve, reject) => {
	setTimeout(() => resolve(1), 1000)
}).then(result => {
	alert(result) // execute
	return result * 2
}).then(result => {
	alert(result) // execute
	return result * 2
})
// 1, 2 출력
```

 프라미스 체이닝이 가능한 이유는 `promise.then` 을 호출하면 프라미스가 반환되기 때문이다. 따라서 계속하여 `.then` 을 호출할 수 있다. 
 핸들러가 값을 반환할 땐, 이 값이 프라미스의 `result` 가 된다. 따라서 다음 `.then` 은 이 값을 이용해 호출된다.

### fetch와 체이닝 응용하기
```js
fetch('/addr/...')
.then(response => response.json())
.then(user => { ... }) 
```  


## 11.4 프라미스와 에러 핸들링

가장 가까운 rejection 핸들러에서 에러를 처리한다.
프라미스 executor와 프라미스 핸들러 코드에는 '보이지 않는 `try..catch`' 가 있다. 예외가 발생하면 암시적 `try..catch` 에서 예외를 잡고, 이를 reject처럼 다룬다.
```js
new Promise((resolve, reject) => {
	throw new Error("error!")
}).catch(alert) 
// Error: error!
```
위 예시는 아래 예시와 똑같이 동작한다.
```js
new Promise((resolve, reject) => {
	reject(new Error("error!"))
}).catch(alert) 
// Error: error!
```

### 처리되지 못한 거부
브라우저 환경에서는 이런 에러를 `unhandledrejection` 이벤트로 등록시켜 잡을 수 있다.


## 11.5 프라미스 API

`Promise` 클래스에는 5가지 정적 메서드가 있다.

### Promise.all(promises)
모든 프라미스가 이행될 때 까지 기다렸다가 그 결과값을 담은 배열을 반환.
하나라도 실패 시 거부되고, 나머지 프라미스의 결과는 무시된다.

### Promise.allSettled(promises)
최근에 추가된 메서드로 모든 프라미스가 처리될 때까지 기다렸다가 그 결과(객체)를 담은 배열을 반환. 객체엔 `status`, `value` 등의 정보가 담긴다.

### Promise.race(promises)
가장 먼저 처리된 프라미스의 결과 또는 에러를 담은 프라미스를 반환

### Promise.resolve(value)/reject(error)
주어진 값/에러를 사용해 이행/거부 상태의 프라미스를 만든다.


## 11.6 프라미스화

콜백을 받는 함수를 프라미스를 반환하는 함수로 바꾸는 것을 말한다.


## 11.7 마이크로태스크

프라미스 핸들러 `.then/catch/finally` 는 항상 비동기적으로 실행된다. 프라미스가 즉시 이행되더라도 핸들러 아래에 있는 코드는 이 핸들러들이 실행되기 이전에 실행된다.

### 마이크로태스크 큐
ECMA에서는 `PromiseJobs` 라는 internal queue, V8 엔진에서는 microtask queue 라고 부른다.
- 마이크로태스크 큐는 FIFO로 동작한다.
- (실행 스택에서) 실행할 것이 아무것도 남아있지 않을 때만 이 큐에 있는 작업이 시작된다.

  
## 11.8 async와 await

### async 함수
`async` 키워드는 함수 앞에 위치한다.
```js
async function f() {}
```
function 앞에 `async` 을 붙이면 해당 함수는 항상 프라미스를 반환한다. 프라미스가 아닌 값을 반환하덜다ㅗ 이행 상태의 프라미스로 값을 감싸 이행된 프라미스가 반환된다.

### await 
`async` 함수 안에서만 동작한다.
```js
async function f() { 
	let value = await promise
}
```
자바스크립트는 `await` 키워드를 만나면 프라미스가 처리(settled)될 떄까지 기다린다. 결과는 그 이후 반환된다. 프라미스가 처리될 때까지 엔진이 다른 일(다른 스크립트를 실행, 이벤트 처리 등)을 할 수 있기 때문에, CPU 리소스가 낭비되지 않는다.
`await` 는 `promise.then` 보다 가독성 좋고 쓰기도 쉽다.






