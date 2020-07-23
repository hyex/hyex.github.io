---
layout: post
title:  "User Guide - Configuring ESLint (2)"
date:   2020-07-23 21:10:00 +0900
categories: javascript
tags: eslint
---

# Configuring ESLint


## Using Configuration Files

구성 파일을 사용하는 방법에는 두 가지가 있습니다.

구성 파일을 사용하는 첫 번째 방법은`.eslintrc. *` 및 `package.json` 파일을 이용하는 것입니다. ESLint는 lint가 될 파일의 디렉토리와 파일 시스템의 루트 디렉토리까지 연속적인 상위 디렉토리에서 자동으로 파일을 찾습니다 (`'root : true'`가 지정되지 않은 경우). 이 옵션은 프로젝트의 다른 부분에 대해 다른 구성을 원하거나 구성 파일을 전달할 필요 없이 다른 사람들이 ESLint를 직접 사용할 수 있도록하려는 경우에 유용합니다.

두 번째는 파일을 원하는 곳에 저장하고`-c` 옵션을 사용하여 CLI에 위치를 전달하는 것입니다.

```
eslint -c myconfig.json myfiletotest.js
```

하나의 구성 파일을 사용 중이고 ESLint가`.eslintrc. *`파일을 무시하도록하려면`-c` 플래그와 함께`--no-eslintrc`를 사용해야합니다.

각각의 경우 구성 파일의 설정이 기본 설정보다 우선됩니다.


## Configuration File Formats

ESLint는 여러 형식의 구성 파일을 지원합니다.

-   **JavaScript**  - use  `.eslintrc.js`  and export an object containing your configuration.
-   **JavaScript (ESM)**  - use  `.eslintrc.cjs`  when running ESLint in JavaScript packages that specify  `"type":"module"`  in their  `package.json`. Note that ESLint does not support ESM configuration at this time.
-   **YAML**  - use  `.eslintrc.yaml`  or  `.eslintrc.yml`  to define the configuration structure.
-   **JSON**  - use  `.eslintrc.json`  to define the configuration structure. ESLint's JSON files also allow JavaScript-style comments.
-   **Deprecated**  - use  `.eslintrc`, which can be either JSON or YAML.
-   **package.json**  - create an  `eslintConfig`  property in your  `package.json`  file and define your configuration there.

동일한 디렉토리에 여러 구성 파일이있는 경우 ESLint는 하나만 사용합니다. 우선 순위는 다음과 같습니다.

1.  `.eslintrc.js`
2.  `.eslintrc.cjs`
3.  `.eslintrc.yaml`
4.  `.eslintrc.yml`
5.  `.eslintrc.json`
6.  `.eslintrc`
7.  `package.json`


## Configuration Cascading and Hierarchy

When using  `.eslintrc.*`  and  `package.json`  files for configuration, you can take advantage of configuration cascading. For instance, suppose you have the following structure:
`.eslintrc. *`및`package.json` 파일을 구성에 사용할 때 구성 cascading을 활용할 수 있습니다. 예를 들어, 다음 구조를 가지고 있다고 가정하십시오.

```
your-project
├── .eslintrc
├── lib
│ └── source.js
└─┬ tests
  ├── .eslintrc
  └── test.js
```

configuration cascade는 가장 가까운`.eslintrc` 파일을 가장 우선 순위가 높은 파일에 lint한 다음 상위 디렉토리의 구성 파일 등을 사용하여 작동합니다. 이 프로젝트에서 ESLint를 실행하면`lib /`의 모든 파일은 프로젝트의 루트에 있는`.eslintrc` 파일을 구성으로 사용합니다. ESLint가`tests /`디렉토리로 이동하면`your-project / .eslintrc` 외에도`your-project / tests / .eslintrc`가 사용됩니다. 따라서 'your-project / tests / test.js'는 디렉토리 계층 구조에서 두 개의`.eslintrc` 파일의 조합을 기반으로 lint가 있으며 가장 가까운 파일이 우선합니다. 이러한 방식으로 프로젝트 수준 ESLint 설정을 사용하고 디렉토리 별 재정의를 수행 할 수 있습니다.

In the same way, if there is a  `package.json`  file in the root directory with an  `eslintConfig`  field, the configuration it describes will apply to all subdirectories beneath it, but the configuration described by the  `.eslintrc`  file in the tests directory will override it where there are conflicting specifications.
같은 방법으로 루트 디렉토리에`eslintConfig` 필드가있는`package.json` 파일이있는 경우, 설명하는 구성은 그 아래의 모든 하위 디렉토리에 적용되지만, tests 디렉토리에 있는`.eslintrc` 파일에 의해 기술 된 설정은 사양이 상충하는 부분을 덮어 씁니다.

```
your-project
├── package.json
├── lib
│ └── source.js
└─┬ tests
  ├── .eslintrc
  └── test.js
```

같은 디렉토리에`.eslintrc`와`package.json` 파일이 있으면`.eslintrc`가 우선권을 가지며`package.json` 파일은 사용되지 않습니다.

기본적으로 ESLint는 모든 상위 폴더에서 루트 디렉토리까지 구성 파일을 찾습니다. 이것은 모든 프로젝트가 특정 규칙을 따르기를 원할 때 유용하지만 때로는 예기치 않은 결과가 발생할 수 있습니다. ESLint를 특정 프로젝트로 제한하려면`package.json` 파일의`eslintConfig` 필드 또는 프로젝트의 루트 레벨에있는`.eslintrc. *`파일에` "root": true`를 넣으십시오. ESLint는 ` "root": true`로 설정을 찾으면 상위 폴더 검색을 중단합니다.

```
{
    "root": true
}
```

And in YAML:

```
---
  root: true
```

예를 들어, `lib /` 디렉토리의 `.eslintrc` 파일에 ` "root": true`가 설정된 `projectA`를 고려하십시오. 이 경우 `main.js`를 lint하는 동안 `lib /`내의 구성이 사용되지만 `projectA /`의 `.eslintrc` 파일은 사용되지 않습니다.

```
home
└── user
    └── projectA
        ├── .eslintrc  <- Not used
        └── lib
            ├── .eslintrc  <- { "root": true }
            └── main.js
```

가장 높은 우선 순위에서 가장 낮은 우선 순위까지 전체 구성 계층 구조는 다음과 같습니다.

1.  Inline configuration
    1.  `/*eslint-disable*/`  and  `/*eslint-enable*/`
    2.  `/*global*/`
    3.  `/*eslint*/`
    4.  `/*eslint-env*/`
2.  Command line options (or CLIEngine equivalents):
    1.  `--global`
    2.  `--rule`
    3.  `--env`
    4.  `-c`,  `--config`
3.  Project-level configuration:
    1.  `.eslintrc.*`  or  `package.json`  file in same directory as linted file
    2.  루트 디렉토리까지 또는 루트 디렉토리를 포함하여 또는 ""root ":``인 설정이 발견 될 때까지 상위 디렉토리 (부모가 우선 순위가 높고 조부모 등)에서`.eslintrc` 및`package.json` 파일을 계속 검색하십시오.
