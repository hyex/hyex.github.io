---
layout: post
title:  "User Guide - Configuring ESLint"
date:   2020-07-21 21:27:00 +0900
categories: javascript
tags: eslint
---

# Configuring ESLint

ESLint는 완벽하게 구성 가능하도록 설계되었습니다. 즉, 모든 규칙을 해제하고 기본 구문 유효성 검증으로만 실행하거나 번들 규칙과 사용자 정의 규칙을 혼합하고 일치시켜 ESLint를 프로젝트에 적합하게 만들 수 있습니다. ESLint를 구성하는 기본 방법에는 두 가지가 있습니다.

1. Configuration Comments - JavaScript 설명을 사용하여 configuration 정보를 파일에 직접 포함시킵니다.
2. Configuration Files - JavaScript, JSON 또는 YAML 파일을 사용하여 전체 디렉토리 및 모든 서브 디렉토리에 대한 구성 정보를 지정하십시오. ESLint가 자동으로 찾아서 읽거나 명령 줄에서 구성 파일을 지정할 수있는 package.json 파일의 .eslintrc.*(js, json, yaml) 파일 또는 eslintConfig 필드 형식 일 수 있습니다.

구성 할 수 있는 몇 가지 정보가 있습니다.
- Environment - 스크립트가 실행되도록 설계된 환경. 각 환경에는 사전 정의 된 특정 글로벌 변수 세트가 있습니다.
- Globals - 실행 중 스크립트가 액세스하는 추가 전역 변수.
 - Rules - 활성화 된 규칙과 Error level
이러한 모든 옵션을 통해 ESLint가 코드를 처리하는 방식을 세밀하게 제어 할 수 있습니다.

## Specifying Parser Options 

ESLint를 사용하면 지원하려는 JavaScript 언어 옵션을 지정할 수 있습니다. 기본적으로 ESLint는 ECMAScript 5 구문이 필요합니다. 파서 옵션을 사용하여 JSX뿐만 아니라 다른 ECMAScript 버전을 지원하도록 해당 설정을 무시할 수 있습니다.

JSX 구문 지원은 React 지원과 다릅니다. React는 ESLint가 인식하지 못하는 JSX 구문에 특정 의미를 적용합니다. React를 사용하고 있고 React 의미를 원하면 eslint-plugin-react를 사용하는 것이 좋습니다. 
같은 토큰으로 ES6 구문을 지원하는 것은 새로운 ES6 전역을 지원하는 것과 다릅니다 (예 : Set과 같은 새로운 유형). 
ES6 구문의 경우 `{ "parserOptions": { "ecmaVersion": 6}};`
새로운 ES6 전역 변수의 경우 `{ "env": { "es6": true}}`를 사용하십시오. 
`{ "env": { "es6": true}}`은 ES6 구문을 자동으로 활성화하지만, `{ "parserOptions": { "ecmaVersion": 6}}`은 ES6 전역을 자동으로 활성화하지 않습니다. 파서 옵션은 `parserOptions` 속성을 사용하여 .eslintrc. * 파일에서 설정됩니다. 

사용 가능한 옵션은 다음과 같습니다.
 - `ecmaVersion` - 사용하려는 ECMAScript 구문의 버전을 지정하려면 3, 5 (기본값), 6, 7, 8, 9, 10 또는 11로 설정하십시오. 연도를 사용하기 위해 2015 (6과 동일), 2016 (7과 동일), 2017 (8과 동일), 2018 (9와 동일), 2019 (10과 동일) 또는 2020 (11과 동일)으로 설정할 수도 있습니다. 기반 명명.
 - `sourceType` - 코드가 ECMAScript 모듈 인 경우 `"script"`(기본값) 또는 `"module"`로 설정하십시오.
 - `ecmaFeatures` - 사용할 추가 언어 기능을 나타내는 객체입니다.
	- `globalReturn` - 전역 범위에서 return 문을 허용
	- `impliedStrict` - 전역 엄격 모드 사용 (ecmaVersion이 5 이상인 경우)
	- jsx-JSX 사용
 
`.eslintrc.json`  파일 예제

```js
{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "semi": "error"
    }
}
```
구문 분석 옵션을 설정하는 것은 ESLint가 구문 분석 에러가 무엇인지 결정하는 것을 돕습니다. 모든 언어 옵션은 기본으로 `false` 입니다.

## Specifying Parser

기본적으로 ESLint는 Espree를 파서로 사용합니다. 구문 분석기가 다음 요구 사항을 충족하는 한 구성 파일에서 다른 구문 분석기를 사용하도록 선택적으로 지정할 수 있습니다.

1. 구성 파일에서로드 할 수있는 노드 모듈이어야 합니다. 일반적으로 이것은 npm을 사용하여 구문 분석기 패키지를 별도로 설치해야 함을 의미합니다.
2. 파서 인터페이스를 준수해야합니다.

이러한 호환성에도 불구하고 외부 파서는 ESLint에서 올바르게 작동하고 ESLint는 다른 파서와의 비호환성 관련 버그를 수정하지 않을 것이라는 보장은 없습니다.

구문 분석기로 사용할 npm 모듈을 표시하려면 `.eslintrc` 파일에서 `parser` 옵션을 사용하여 모듈을 지정하십시오. 예를 들어 다음은 Espree 대신 Esprima를 사용하도록 지정합니다.

```js
{
    "parser": "esprima",
    "rules": {
        "semi": "error"
    }
}
```

다음 구문 분석기는 ESLint와 호환됩니다.

- Esprima
- Babel - ESLint-ESLint와 호환되도록하는 Babel 파서 래퍼입니다.
- @typescript-eslint/parser - TypeScript를 ESTree 호환 형식으로 변환하여 ESLint에서 사용할 수있는 파서입니다.

## Specifying Processor

플러그인은 프로세서를 제공 할 수 있습니다. 프로세서는 다른 종류의 파일에서 JavaScript 코드를 추출한 다음 ESLint가 JavaScript 코드를 lint 할 수 있습니다. 또는 프로세서가 특정 목적을 위해 전처리에서 JavaScript 코드를 변환 할 수 있습니다.

<b>구성 파일에서 프로세서를 지정</b> 하려면,  플러그인 이름의 문자열과 프로세서 이름이 슬래시로 연결된 `processor` 키를 사용하십시오. 예를 들어, 다음은 플러그인 `a-plugin`이 제공한 프로세서 `a-processor`를 활성화합니다.
```js
{
    "plugins": ["a-plugin"],
    "processor": "a-plugin/a-processor"
}
```
<b>특정 종류의 파일을 위해 프로세서를 지정</b> 하려면, `overrides` 키와 `processor` 키를 조합해서 사용하세요. 예를 들어, 다음은 `*.md` 파일을 위해 `a-plugin/markdown` 프로세서를 사용합니다.
```js
{
    "plugins": ["a-plugin"],
    "overrides": [
        {
            "files": ["*.md"],
            "processor": "a-plugin/markdown"
        }
    ]
}

```
프로세서는  `0.js`와 `1.js`와 같은 명명된 코드 블럭을 만들 수 있습니다. ESLint는 이러한 명명된 코드 블록을 원본 파일의 하위 파일로 처리합니다. 당신은 config의 `overrides` 섹션안에 명명된 코드 블록을 위한 추가 구성을 지정할 수 있습니다. 예를 들어, 다음은 마크다운 파일에서 .js로 끝나는 명명된 코드 블록에 대해  `strict` 규칙을 비활성합니다.
```js
{
    "plugins": ["a-plugin"],
    "overrides": [
        {
            "files": ["*.md"],
            "processor": "a-plugin/markdown"
        },
        {
            "files": ["**/*.md/*.js"],
            "rules": {
                "strict": "off"
            }
        }
    ]
}
```
ESLint는 명명된 코드 블록의 파일 경로를 확인하고 만약 어떠한 `overrides` 항목이 파일 경로에 일치하지 않는다면 이들을 무시합니다. 만약 `*.js`이외의 명명된 코드 블록을 lint하기를 원한다면 `overrids` 엔트리를 명확하게 하십시오.

## Specifying Environments
환경은 미리 정의된 글로벌 변수를 정의합니다. 가능한 환경을 다음과 같습니다:

-   `browser`  - browser global variables.
-   `node`  - Node.js global variables and Node.js scoping.
-   `commonjs`  - CommonJS global variables and CommonJS scoping (use this for browser-only code that uses Browserify/WebPack).
-   `shared-node-browser`  - Globals common to both Node.js and Browser.
-   `es6`  - enable all ECMAScript 6 features except for modules (this automatically sets the  `ecmaVersion`  parser option to 6).
-   `es2017`  - adds all ECMAScript 2017 globals and automatically sets the  `ecmaVersion`  parser option to 8.
-   `es2020`  - adds all ECMAScript 2020 globals and automatically sets the  `ecmaVersion`  parser option to 11.
-   `worker`  - web workers global variables.
-   `amd`  - defines  `require()`  and  `define()`  as global variables as per the  [amd](https://github.com/amdjs/amdjs-api/wiki/AMD)  spec.
-   `mocha`  - adds all of the Mocha testing global variables.
-   `jasmine`  - adds all of the Jasmine testing global variables for version 1.3 and 2.0.
-   `jest`  - Jest global variables.
-   `phantomjs`  - PhantomJS global variables.
-   `protractor`  - Protractor global variables.
-   `qunit`  - QUnit global variables.
-   `jquery`  - jQuery global variables.
-   `prototypejs`  - Prototype.js global variables.
-   `shelljs`  - ShellJS global variables.
-   `meteor`  - Meteor global variables.
-   `mongo`  - MongoDB global variables.
-   `applescript`  - AppleScript global variables.
-   `nashorn`  - Java 8 Nashorn global variables.
-   `serviceworker`  - Service Worker global variables.
-   `atomtest`  - Atom test helper globals.
-   `embertest`  - Ember test helper globals.
-   `webextensions`  - WebExtensions globals.
-   `greasemonkey`  - GreaseMonkey globals.

이러한 환경은 상호 배타적이지 않기에, 당신은 한 번에 둘 이상을 정의할 수 있습니다.
환경은 파일 내부, 구성 파일, 또는 `--env` 명령 줄 플래그를 사용해서 지정될 수 있습니다.

### 파일 내부
당신의 자바스크립트 파일안에서 주석을 사용해 환경을 지정 하려면, 다음 형식을 사용하세요:
```
/* eslint-env node, mocha */ 
```
이 것은 Node.js 와 Mocha 환경을 사용 가능하게 합니다.

### 구성 파일 내
구성 파일에서 환경을 지정하려면, `env`키를 사용하고 `true`로 설정할 수 있도록 사용 가능한 환경을 지정하세요. 예를 들어, 다음은 browser와 Node.js 환경을 사용 가능하게 합니다.
```js
{
    "env": {
        "browser": true,
        "node": true
    }
}
```

`package.json` 파일:
```js
{
    "name": "mypackage",
    "version": "0.0.1",
    "eslintConfig": {
        "env": {
            "browser": true,
            "node": true
        }
    }
}
```
YAML:
```js
---
  env:
    browser: true
    node: true
```

플러그인에서 환경을 사용하기 원한다면, `plugins` 배열에 플러그인 이름을 확실히 지정한 다음 접두사 뒤에 붙지 않은 플러그인 이름, 슬래시, 환경 이름을 사용하세요. 예를 들면 다음과 같습니다.
```js
{
    "plugins": ["example"],
    "env": {
        "example/custom": true
    }
}
```
또는 `package.json` 파일 안에서는
```json
{
    "name": "mypackage",
    "version": "0.0.1",
    "eslintConfig": {
        "plugins": ["example"],
        "env": {
            "example/custom": true
        }
    }
}
```
YAML:
```yaml
---
  plugins:
    - example
  env:
    example/custom: true
```

## Specifying Globals

no-undef 규칙은 동일한 파일 내에서 접근되었지만 정의되지 않은 변수에 대해 경고합니다. 파일내에서 전역 변수를 사용하는 경우  ESLint가 그 사용에 대해 경고하지 않도록 전역 변수를 정의하는 것이 좋습니다. 파일 내부 또는 구성 파일의 주석을 사용하여 전역 변수를 정의할 수 있습니다.
JavaScript 파일 내부의 주석을 사용해서 전역 변수를 지정하려면, 다음과 같은 형식을 따르세요.
```js
/* global var1, var2 */
```
이 것은 두 개의 전역 변수, `var1` 와 `var2`를 정의합니다. 선택적으로 이러한 전역 변수를 쓸 수 있도록(읽을 수 있는 대신) 지정하려면, `"writable"` 플래그를 사용하여 각 전역 변수를 설정할 수 있습니다.
```js
/* global var1:writable, var2:writable */
```

구성 파일 내에서 전역 변수를 구성하려면, `globals` 구성 속성을 사용하려는 전역 변수에 대해 명명된 키가 포함된 오브젝트로 설정하세요. 각 전역 변수 키에 대해, 해당 값을  `"writable"`으로 설정하여 변수를 덮어 쓰거나 `readonly`으로 덮어 쓰기를 허용하지 않습니다. 예를 들면 다음과 같습니다.
```js
{
    "globals": {
        "var1": "writable",
        "var2": "readonly"
    }
}
```
YAML:
```yaml
---
  globals:
    var1: writable
    var2: readonly
```
이 예제에서 `var1`은 코드에서 덮어 쓰기가 가능하지만 `var2`는 불가능합니다.

전역 변수는 `"off"` 문자열로 비활성화 할 수 있습니다. 예를 들어 대부분의 ES2015 전역 변수를 사용 할 수 있지만 `Promise`는 사용할 수 없는 환경에서, 이 설정을 사용할 수 있습니다:
```js
{
    "env": {
        "es6": true
    },
    "globals": {
        "Promise": "off"
    }
}
```
역사적인 이유로, boolean 값인 `false`와 string 값인 `"readable"`은 `"readonly"`와 같습니다. 유사하게, boolean 값인 `true`와 string 값인 `"writable"`은 `"writable"`과 같습니다. 그러나, 이전 값의 사용은 더 이상 사용하지 않습니다. <br />

<b>Note</b>: 코드에서 read-only 전역 변수에 대한 수정을 허용하지 않으려면 no-global-assign	규칙을 활성화하십시오.

## Configuring Plugins